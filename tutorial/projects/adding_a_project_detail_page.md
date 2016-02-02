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

Like our homepage state, our project detail state needs to load a project from the backend. But before do that, we have to tell our frontend application how to access a single project on the backend. Let's return to our 

## Controller

Let's make a controller for our new State

## Template

TODO