# Displaying the projects list

Now that we can load our projects, we'd like to show them to the user somewhere. For the moment, let's just list them on the homepage.

## About States

Xing applications use UI-router to describe the user interface as a tree of states. States are a convenient way to organize both functionality and appearance and styling.  The default state tree of a new Xing project looks like this:

![](images/xing-default-state-tree.png)

You're not tied to this structure at all: you can write your application's states however you like. This is just a set of defaults Xing gives you to get you started. States have a full name created from their position in the tree, separated by dots, for example `root.homepage`, or `root.inner.sessions`.  

We'll talk more about states in a bit, but for now you just need to know that the first "page" you see when viewing Crowdfundr in a browser is the `root.homepage.show` state.

We're going to add our list of projects to the user interface for that state.

## The homepage state

Open the file `frontend/src/app/homepage/homepageStates.js`. This file contains a description of all the states under the 'homepage' part of the tree.  We recommend that you group all the code related to a state in a directory with that name.  When the number of states gets too big and the file gets too long, you can break some of them out into a subdirectory.  For now, we just have the homepage state and the homepage show state, each of which has a specification for its controller (where interactive code for this state lives), its template (HTML for the state), and the user-visible URL for the state.  

#### `frontend/src/app/homepage/homepageStates.js`

```javascript
import {State, Resolve} from 'stateInjector';
import {AdminOnlyState, TrackAdminState} from 'stateClasses';

@State('root.homepage')
export class HomepageState extends TrackAdminState {
  constructor() {
    super();
    this.controller = 'HomepageCtrl';
    this.templateUrl = 'homepage/homepage.tpl.html';
    this.abstract = true;
    this.url = 'home';
  }
}

@State('root.homepage.show')
export class HomepageShowState {
  constructor() {
    this.url = '';
    this.controller = 'HomepageShowCtrl';
    this.templateUrl = 'homepage/homepage-show.tpl.html';
  }
}
```

These state descriptors set up our states in a fairly straightforward syntax. The URL entries get concatenated together: the root state has a URL of "/" (you can see this in `rootState.js`), root.homepage is 'home', and root.homepage.show is '', so a user visiting root.homepage.show will see '/home' in their browser bar.

The `abstract = true` setting in `root.homepage` means a user cannot actually visit this state: `root.homepage` only exists to be an ancestor of other states. (You might wonder why the state is even there.  It's because Xing also has a prebuilt content management module. We didn't use the CMS for this tutorial, but when it is included, there's a `root.homepage.edit` state that allows side administrators to modify the content of the homepage.)

Finally, both states have associated controllers and templates.  

## Loading the projects 

We want our frontend to fetch the list of projects from the backend whenever a user visits the `root.homepage.show` state. There are a number of ways we could do that, for now we'll create a `Resolve` function: a function that must execute successfully for the application to enter that state, and which makes its return value available to controllers for this state and any sub-states.  

Change the homepage show state entry to look like this:

```javascript
@State('root.homepage.show')
export class HomepageShowState {
  constructor() {
    this.url = '';
    this.controller = 'HomepageShowCtrl';
    this.controllerAs = 'HomepageShowCtrl';
    this.templateUrl = 'homepage/homepage-show.tpl.html';
  }

  @Resolve("resources")
  projects(resources){
    return resources.projects().load();
  }

}
```

The `@Resolve` function uses the `resources` object - the same one we were working on with `resources.js`.  It tells Angular to fetch a resource named 'projects' based on the URL provided by the back-end.  Because the function itself is named 'projects', it will create an object also called 'projects' and make it available to the controller for this state.  (Again, it's a convenient coincidence that it's called `projects` in both cases.  So let's review which is which: the `@Resolve` function called `projects` creates a `projects` object we can refer to in the controller.  The other function of the same name in `resources.**projects()**.load()` refers to the HTTP JSON resource of that name we set up in `resources.js`.  Clear?  great! Still confusing?  Don't worry about it for now, it'll make more sense with experience.)

The other thing we did in this snippet is add a `controllerAs` specification to our state.  All this does is give our controller an explicit name so we can refer to its data and functions by name in the template.  H[ere's a nice introduction to this syntax convention](https://toddmotto.com/digging-into-angulars-controller-as-syntax/) if you're curious.

### Loading the projects over the network

That's all we need to load the projects list whenever a user visits the homepage. 

We haven't displayed them yet, so if you fire up `rake develop` you'll see the same homepage as before.  But if you're feeling intrepid, open the network tab of your browser's developer tools, and reload the page. You should see a whole bunch of requests to the backend, including `GET /resources` and `GET /projects`, the JSON requests that happen because of that `resources.projects().load()` command.

## Getting our projects into the controller

Now that we've loaded the projects, displaying them takes two steps. First, they must be stored in the controller so that they are accessible to our HTML template. 

Open the controller file for this state at `frontend/src/app/homepage/homepageControllers.js` and replace the entire contents with the following:

(TODO: the homepage controllers in the default application still use the older controller function style instead of classes.  Hence I'm asking the tutorial user to completely overwrite the file.)

#### `frontend/src/app/homepage/homepageControllers.js`

```javascript
import {Controller} from 'a1atscript';

@Controller('HomepageShowCtrl', ['projects'])
export class HomepageShowController {
  constructor(projects) {
    this.projects = projects;
  }
}

@Controller('HomepageCtrl', [])
export class HomepageController {
}
```

This creates two controllers: one for the parent homepage state (which does nothing in particular), and one for the homepage show state.  In that controller, the `['projects']` argument to the annotation specifies that the projects object is a dependency to be injected into this controller. This is the same `projects` object created by our `@Resolve` function back in the state descriptor. (We could inject other objects here as well, like various angular tools, or other resolves from other ancestor states, etc.) If you're not familiar with [dependency injection in AngularJS](https://docs.angularjs.org/guide/di), we strongly recommend you spend some time learning about it: you'll use it everywhere in Xing applications.  

All our dependencies get passed to the controller's constructor function.  Here we only do one thing with it: we store it as a property of the controller.  That's what will let us refer to it in the template.

## Displaying the projects on the page

The file `frontend/src/app/homepage/homepage-show.tpl.html` contains the HTML template for the root.homepage.show state. We're just going to make a table with one row for each project in our projects list.  While we're at it, let's update the headline of our homepage to reflect the name of our application, Crowdfundr.  Open that file and replace the contents with this:

#### `frontend/src/app/homepage/homepage-show.tpl.html`

```html
<h1>Crowdfundr</h1>
<p>Below you can see the list of projects available to be funded!</p>

<table id='projects'>

  <tr>
    <th>Project Name</th>
  </tr>
  <tr ng-repeat="project in HomepageShowCtrl.projects">
    <td>{{project.name}}</td>
  </tr>
</table>
```

## That's it!

If we haven't led you astray, you should be able to run the application now and see the list of projects on your homepage.  They'll have random names as generated by your sample data script in the `backend` part of the app, so yours should be similar but not exactly like this screenshot.











