# Your First Resource: Projects

Conceptual design of a Xing project works best when centered around your HTTP REST resources. Resources are chunks of data that your application passes between the server and the user ... or the backend and the frontend.

What data will your application transmit and why?  How will the resources relate to each other?

## Backend resources

On the backend, individual resources might map onto database tables ... or they might not.  Users of Rails often have the habit of thinking of a single ActiveRecord model as being synonymous with a 'resource'.  This is often true, but not always. In Rails, the resource concept maps more closely to your *routes* than to your models.  A GET to a URL (e.g. GET /my_cool_thing/42) fetches a single resource ... but the data that comes out might be provided by multiple database tables ... or none!  

## Frontend resources

Likewise, it's easy to fall into the trap of thinking a resource maps to a 'page', or to a single user interface.  But again, that's not always true.  A dashboard page might show snippets of data from dozens of different resources.

## Defining the resource format

With that in mind, we'll start by conceiving of a resource, and documenting its format.  By Xing Framework convention, documents describing our resources go in the API_DOC directory.

