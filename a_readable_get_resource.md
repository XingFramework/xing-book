# A readable GET resource

Lets start with just the individual ```/project/{id}``` resource.

## Test first! (write a request spec)

We're going to start by writing a spec.  (You do write your specs first, don't you?) We'll start with an integration-level spec that tests that the entire backend produces JSON as desired when given the proper HTTP request, and use that as a framework to guide our unit tests and code.

We'll use FactoryGirl to manufacture a Project, and submit an HTTP request to to the proper URL, and inspect the returned JSON.

##### ```backend/spec/requests/project_spec.rb```



## Model and Migration

TODO 

## Serializer

TODO

## Controller

TODO