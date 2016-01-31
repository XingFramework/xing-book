# Displaying the projects list

Now that we can load our projects, we'd like to show them to the user somewhere. For the moment, let's just list them on the homepage.

## About States

Xing applications use UI-router to describe the user interface as a tree of states. States are a convenient way to organize both functionality and appearance and styling.  The default state tree of a new Xing project looks like this:

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









