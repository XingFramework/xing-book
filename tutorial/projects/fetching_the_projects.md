# Fetching the projects via JSON

With Xing hypermedia JSON, our frontend doesn't actually need to know the URL where the projects can be found. Instead, it only needs to know the name of the resource: the backend will tell the frontend what the URLs are.

## The top-level resources list

All Xing projects have a single resource, with URL /resources, that lists all the available resources that can be discovered over the API.  For now, we just need to tell the frontend that there's a resource called 'projects' available from the backend.

Open the file `frontend/src/common/resources.js`, the file that describes the top-level resources list.  Most of this is boilerplate you can ignore for the moment:  some module imports at the top, some code at the bottom that makes the entire resources heirarchy available to the rest of the application.  For the moment, we're interested in just this one function in the middle:

```javascript
RL.Describe(Resources, (desc) => {
  // put top level links to resources here
});
```

This is where we'll tell our frontend app which resources -- by name -- it can expect to find information about in `/resources`.  Let's add a single list resource, called 'projects'.  Change that function to look like this:

```javascript
RL.Describe(Resources, (desc) => {
  // put top level links to resources here
  desc.hasList('projects', Project, [])
});
```

This says there is an array of other objects available, called `'projects'`, and that each item in that list is described by the `Project` class.  It doesn't say what the URL is: it's something of a coincidence that the backend serves that resource at `'/projects'`.  It could have the URL `/list-of-projects` or anything else; the frontend doesn't really care because it's going to look the URL up in `/resources`.   

```javascript
```

## Describing a Project

Now we need to tell the frontend what a project looks like.  Create a file called `frontend/src/common/resources/Project.js` and paste in this code:

```javascript
import RL from "relayer";

export default class Project extends RL.Resource{}
RL.Describe(Project, (desc) => {
  desc.property("name", "");
  desc.property("description", "");
  desc.property("deadline", "");
  desc.property("goal", "");
});
```

TODO  No new state - we'll add it to the homepage show state. Introduction to the homepage show state here.

## Wrapping Up

