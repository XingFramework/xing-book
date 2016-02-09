# Adding a project detail page

Now that we can list our projects, we'd like a user to be able to click on one for more information. 

## Module

In the last section, we added some code and HTML to an existing state: the homepage.

The project detail page, however, is likely get pretty large over time, with a lot of UX components. You can imagine information about pledge levels, history of the project, a thermometer showing how close the project is to its fundraising goal, list of people who have pledged, etc.

Because of this, we're going to want to put the project detail interface files into their own subdirectory, with a dedicated module to group the JavaScript files together.

Create a directory `frontend/src/app/projects/` and in it place a file `project.js` with the following content:

##### `frontend/src/app/projects/project.js`

```JavaScript
import {Module} from 'a1atscript';
import * as ProjectsControllers from './projectsControllers.js';
import * as ProjectsStates from './projectsStates.js';

var Project = new Module('project', [
  ProjectsControllers,
  ProjectsStates
]);

export default Project;
```

This file doesn't do much by itself: only imports the code from `projectsControllers` and `projectsStates` (which we haven't written yet), and groups them into an ES6 module called `Project`. By doing so, our top-level application file will only have to import that one `Project` module instead of each individual file. As we expand the capabilities of the project interface, we'll update this file to collect all the sub-modules. 

## State

Now let's add a new state to our UI-Router state tree, representing the project detail user interface.

`frontend/src/app/projects/projectsStates.js`

```JavaScript
import {State, Resolve, Inject} from "stateInjector";

@State("root.inner.project")
export class ProjectsState {
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
    this.templateUrl = "projects/project.tpl.html";
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

Up to this point, we haven't done any testing in our frontend application. However, this resolve function contains enough logic for a simple test. In our `frontend/test` directory, add a `projects` folder, then make a `projectsStates.js` file and put the following test in it:

`frontend/test/projects/projectsStates.js`

```javascript
import {ProjectsState} from "../../src/app/projects/projectsStates.js";

describe("ProjectsState", function() {
  var projectsState,
    mockResources,
    mockProjectResource,
    mockStateParams;

  beforeEach(function() {
    mockStateParams = {
      id: '5'
    }

    mockProjectResource = jasmine.createSpyObj('project resource', ['load']);

    mockResources = {
      project: jasmine.createSpy('project').and.returnValue(mockProjectResource)
    }

    projectsState = new ProjectsState();
  });

  it("should resolve a project", function() {
    projectsState.project(mockResources, mockStateParams);
    expect(mockResources.project).toHaveBeenCalledWith({id: '5'})
  });
});

```

Since this is our first experience writing tests on the frontend, let's a spend a few minutes looking at what's going on here. Frontend tests in Xing use the Jasmine test framework. You can read more about Jasmine [here](http://jasmine.github.io/). Our goal in this test is to verify that when the project detail state resolves a project, the state parameters will be passed to the resource API. You'll notice however, in this test, angular, UI-router, and $stateParams are no where in sight. 

If we setup a test that used all of these libraries, it'd be much closer to an integration test. The creators of Xing learned through experience that testing states this way is highly failure prone. Instead, we use write some simple javascript "mock" objects that behave like the actual objects that will be used by the project state. We create a `mockStateParams` object that has an `id` property of "5", just like the real `$stateParams` object is likely to have when the application runs. We create a jasmine "spy" for the `project` method of the resource API. This creates a simple function that will record that the function was called and what parameters got passed. Finally the actual test verifies that when we call the `project` resolve on a ProjectsState, the `project` method of the the resources API gets passed the state parameters.

It might seem like a lot of setup to test such a simple function. However, using simple mock objects makes our test a true unit test, meaning it tests only the logic of the class we're testing (ProjectsState) and very little else. One great side effect of testing this way is that tests run extremely quickly -- so quickly that when rake develop is running, frontend tests run automatically every time you make a change.

Now that you're familiar with Xing frontend concepts and the way we test our code, we'll write tests first any time we're going to write logic that is complex enough to merit testing.

## Controller

Let's make a controller for our new State:

`frontend/src/app/projects/projectsController.js`

```javascript
import {Controller} from 'a1atscript';

@Controller('ProjectsCtrl', ['project'])
export class ProjectsController {
  constructor(project) {
    this.project = project;
  }
}
```

Like the homepage controller, the constructor here just saves the project from the project detail state to the instance of the controller.
 
## Template

Finally, let's make a template to display our project:

`frontend/src/app/projects/project.tpl.html`

```html
<h1>{{project.project.name}}</h1>
<h1>{{project.project.name}}</h1>

<dl>
  <dt>Description</dt>
  <dd>{{project.project.description}}</dd>

  <dt>Deadline</dt>
  <dd>{{project.project.deadline}}</dd>

  <dt>Goal</dt>
  <dd>{{project.project.goal}}</dd>
</dl>

```

## Hooking up the projects

Our projects module is done, but we need to import it into our app and hook up the homepage so we can get from the projects list to an individual project.

First, we import our Projects module in `frontend/src/app/app.js`. This file describes the "top level" module for our application. Everything that goes into a Xing frontend gets imported into the app.js and put in the top level module.

Take a look at the file right now:

`frontend/src/app/app.js`:

```javascript
import {appName} from 'config';
import {} from 'build/templates-app.js';
import {} from 'build/templates-common.js';

import { Module, Injector } from "a1atscript";
import SessionLinks from 'components/sessionLinks/sessionLinks.js';

import {
  Fallback,
  Toast,
  ResponsiveMenu,
  UnimplementedDirective,
  StateAttrs,
  uiRouteLogger,
  stateFallback,
  ExampleForm
} from 'xing-frontend-utils';

import appConfig from './appConfig.js';

import Auth from './auth/auth.js';
import Homepage from './homepage/homepage.js';

import * as RootStates from './rootStates.js';
import RootCtrl from './rootController.js';

import Resources from "common/resources.js";

var app = new Module(appName, [
  'templates-app', 'templates-common', 'ui.router',
  'picardy.fontawesome',
  StateAttrs,
  uiRouteLogger,
  stateFallback,
  Homepage,
  Auth,
  Fallback,
  ResponsiveMenu,
  UnimplementedDirective,
  ExampleForm,
  SessionLinks,
  Toast,
  appConfig,
  RootStates,
  RootCtrl,
  Resources
]);

var injector = new Injector(appName);
injector.instantiate(app);
```

Essentially, app.js imports all of the other modules in the app, then instantiates the top level application module. Let's import our Projects module below the other imports:

```javascript
import Projects from "./projects/projects.js";
```

And add it to the app module:
```javascript
var app = new Module(appName, [
  'templates-app', 'templates-common', 'ui.router',
  'picardy.fontawesome',
  StateAttrs,
  uiRouteLogger,
  stateFallback,
  Homepage,
  Auth,
  Fallback,
  ResponsiveMenu,
  UnimplementedDirective,
  ExampleForm,
  SessionLinks,
  Toast,
  appConfig,
  RootStates,
  RootCtrl,
  Resources,
  Projects
]);
```

Our projects module is now part of our application. Now lets provide a way to get from the projects list to the project detail page. Let's go back to our projects template and make some changes:

`frontend/src/app/homepage/homepage-show.tpl.html`

```javascript
<h1>Crowdfundr</h1>
<p>Below you can see the list of projects available to be funded!</p>

<table id='projects'>

  <tr>
    <th>Project Name</th>
    <th></th>
  </tr>
  <tr ng-repeat="project in homepageShow.projects">
    <td>{{project.name}}</td>
    <td><a ui-sref="root.inner.project(id: project.shortLink)">View</a></td>
  </tr>
</table>
```

We've added a second column to our table with a link to view a project in detail. However, instead of an `href` tag, our link has a `ui-sref` tag, a  UI-Router tag that tells the application to transition to a new state when the link is clicked. We pass ui-sref the name of the state as well as parameters for that state. Then UI-Router handle transitioning the state as well as updating the url.

In this case, we're transitioning to `root.inner.project`, our project detail state. However, recall that the project detail state needs an "id" parameter in order to load a project from the server. Here we use a special method `shortLink` on the project to get the id parameter. What does the method do? In our project detail state, we used a parameter in combination with URI template to produce a regular URL. `shortLink` does this in reverse -- it uses a URI template in combination with a real URL to extract a shorthand version of the URL. Recall that our projects list resource looks something like this:

```javascript
{
  links: {
    self: `/projects`,
    template: `/projects/{id}`
  },
  data: [
    ...
    {
      links: {
        self: "/projects/5"
      },
      data: {
        name: "My Project"
      }
    },
    ...
  ]
}
```

When we call `shortLink` on a project in a list, the project's `self` link is matched up against the `template` link for the list, to extract a shortened version of the URL. In the example above, `/projects/5` is compared against `/project/{id}` to extract a shorthand of `5` to refer to the project.

If everything has gone well so far, we've completed setting up a detail page for projects, and you can now get to a project detail from the projects list.

## Wrapping up - Stylesheets And Formatting

Our project detail page looks a little dry. Let's go ahead an add a stylesheet for it.

`frontend/src/styles/state/_root_inner_project.sass`

```scss
#root_inner_project
  line-height: 2em
  dt
    font-weight: bold
```

Since this is a new sass file, we need to import it in `frontend/src/styles/main.sass`. Add the line:

    @import "states/root_inner_project"

at the bottom of the file.

Now our styles are looking better, but the display of the project deadline looks pretty terrible. We can solve this by adding a method to our controller for project detail to create a formated date:

`frontend/src/app/projects/projectsControllers.js`

```javascript
import {Controller} from 'a1atscript';

@Controller('ProjectCtrl', ['project'])
export class ProjectController {
  constructor(project) {
    this.project = project;
  }
  
  get formattedDeadline() {
    return new Date(this.project.deadline).toDateString();
  }
}
```

Then we'll update the template to use the formatted date:

`frontend/src/app/projects/project.tpl.html`

```javascript
<h1>{{project.project.name}}</h1>

<dl>
  <dt>Description</dt>
  <dd>{{project.project.description}}</dd>

  <dt>Deadline</dt>
  <dd>{{project.formattedDeadline}}</dd>

  <dt>Goal</dt>
  <dd>{{project.project.goal}}</dd>
</dl>

<a ui-sref="root.homepage.show">Go Back</a>

```

Note here we also added another `ui-sref` link at the bottom of the page to go back to the homepage.

This brings us to the end of our introduction to the frontend. We now have a working app that displays projects. So far, we've focused on reading data from the backend and displaying it on screen. In next section, we'll learn writing data to the backend, and creating and updating resources in Xing.
