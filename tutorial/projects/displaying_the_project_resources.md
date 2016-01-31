# Displaying a list of resources

We're going to build several things in this section:
* Instructions for the AngularJS frontend for communicating with the backend via Hypermedia JSON.
* An application State using ui-router; this is similar conceptually to a "page" that the user might view.
* A table on that state that outputs the list of projects supplied by the back-end.

## Resource definition with Relayer

With Xing hypermedia JSON, our frontend doesn't actually need to know the URL where the projects can be found. Instead, it only needs to know the name of the resource: the backend will tell the frontend what the URLs are.

All Xing projects have a single resource, that lives at the URL  /resources, that describes all the available resources that can be discovered over the API.  For now, we just need to tell the frontend

## State

TODO  No new state - we'll add it to the homepage show state. Introduction to the homepage show state here.

## Controller

TODO  No new controller, add to the homepage show controller.

## Template

TODO Add to the homepage show template
