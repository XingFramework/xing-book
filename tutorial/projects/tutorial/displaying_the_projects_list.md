# Displaying the projects list

Now that we can load our projects, we'd like to show them to the user somewhere. For the moment, let's just list them on the homepage.

## About States

Xing applications use UI-router to describe the user interface as a tree of states. States are a convenient way to organize both functionality and appearance and styling.  The default state tree of a new Xing project looks like this:

You're not tied to this structure at all: you can write your application's states however you like. This is just a set of defaults Xing gives you to get you started. States have a full name created from their position in the tree, separated by dots, for example `root.homepage`, or `root.inner.sessions`.  

We'll talk more about states in a bit, but for now you just need to know that the first "page" you see when viewing Crowdfundr in a browser is the `root.homepage.show` state.

We're going to add our list of projects to the user interface for that state.

## The homepage state.

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


