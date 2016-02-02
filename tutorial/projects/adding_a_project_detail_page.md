# Adding a project detail page

Now that we can list our projects, we'd like a user to be able to click on one for more information. 

## Module

In the last section, we added some code and HTML to an existing state: the homepage.

The project detail page, however, is likely get pretty large over time, with a lot of UX components. You can imagine information about pledge levels, history of the project, a thermometer showing how close the project is to its fundraising goal, list of people who have pledged, etc.

Because of this, we're going to want to put the project detail interface files into their own subdirectory, with a dedicated module to group the JavaScript files together.

Create a directory `frontend/src/app/project/` and in it place a file `project.js` with the following content:

##### `frontend/src/app/project/project.js`

```JavaScript
import {Module} from 'a1atscript';
import * as ProjectControllers from './projectControllers.js';
import * as ProjectStates from './projectStates.js';

var Project = new Module('project', [
  ProjectControllers,
  ProjectStates
]);

export default Project;
```

This file doesn't do much by itself: only imports the code from `projectControllers` and `projectStates` (which we haven't written yet), and groups them into an ES6 module called `Project`. By doing so, our top-level application file will only have to import that one `Project` module instead of each individual file. As we expand the capabilities of the project interface, we'll update this file to collect all the sub-modules. 

## State

Now let's add a new state to our UI-Router state tree, representing the project detail user interface.

`frontend/src/app/projects/projectsStates.js`

```JavaScript
import {State, Resolve, Inject} from "stateInjector";

@State("root.inner.project")
export class ProjectState {
  constructor() {
    this.url = "/project/:id";
    this.templateUrl = "project/project.tpl.html";
    this.controller = "ProjectCtrl";
    this.controllerAs = "project";
  }
}
```

This should look fairly familiar from the previous states we worked on: it adds a new state to the tree at `root.inner.project`, and associates it with a template and a controller. The one new element is the addition of the `:id` parameter to the URL. This does what you'd expect: the ID of each project will get plugged into the visible URL, like `http://crowdfundr.com/project/1234` so that each project can have a unique address.

We'll explain where that ID comes from in a bit.

##Project Resource

Like our homepage state, our project detail state needs to load a project from the backend. But before do that, we have to tell our frontend application how to access a single project on the backend. Remember our frontend finds all its resources initially by requesting `/resources` from the backend. When the backend responds to request at `/resources` response will look something like this:

`JSON response to GET /resources`

```javascript
{
    data: {
    },
    links: {
      ...
      projects: "/projects"
      project: "/projects/{id}"
      ...
    }
}
```

The backend will provide a 'project' resource link. However, instead of providing a direct link to get a specific project, the backend provides a link template for getting ANY project. The link template is in the RFC6570 URI template format we described earlier.

So how does the frontend use this link? Let's return back to `frontend/src/common/resources.js` where our top level resource description right now looks like this:

```javascript
RL.Describe(Resources, (desc) => {
  // put top level links to resources here
  desc.hasList('projects', Project, [])
});
```

If our "project" resource were a direct link to a single project we could just add one line:

```javascript
RL.Describe(Resources, (desc) => {
  // put top level links to resources here
  desc.hasList('projects', Project, [])
  desc.hasOne('project', Project);
});
```

In order to tell frontend that the project resource is a link template, we add one additional line:

```javascript
RL.Describe(Resources, (desc) => {
  // put top level links to resources here
  desc.hasList('projects', Project, [])
  var project = desc.hasOne('project', Project);
  project.templated = true;
});
```

Whenever you call Relayer methods to describe a resource, the method returns an object you can use to further customize the description. In this case, we took the return value of our "hasOne" and told Relayer this resource is templated. 

In order to load actually load an individual project, we need to pass parameters to complete the template. Let's return to the  project detail state and add a resolve to load the individual project:

```javascript
import {State, Resolve, Inject} from "stateInjector";

@State("root.inner.project")
export class ProjectState {
  constructor() {
    this.url = "/project/:id";
    this.templateUrl = "project/project.tpl.html";
    this.controller = "ProjectCtrl";
    this.controllerAs = "project";
  }
  
  @Resolve('resources', '$stateParams')
  project(resources, $stateParams) {
    return resources.project($stateParams).load();
  }
}
```

To load our project, we utilize our resources API as well as a special object provided by UI-Router called `$stateParams`. At any given time, `$stateParams` describes the parameters given to the current state in the URL. So recall when we described the url for the project state, we said it was `/project/:id`. So if the current frontend URL becomes `/projects/5` we'll enter the project state, and $stateParams will be `{ id: '5' }`. So 

```javascript
  return resources.project($stateParams).load();
```

is equivalent to:
```javascript
  return resources.project({id: '5'}).load();
```

We've named our project detail state's parameter the same as the 'project' link template. $stateParams allow us to fill in the link template, which produces a final request to `/projects/5` on the backend.

## A Simple Frontend Test

Up to this point, we

## Controller

Let's make a controller for our new State

## Template

TODO