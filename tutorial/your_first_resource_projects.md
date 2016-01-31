# Your First Resource: Projects

Conceptual design of a Xing project works best when centered around your HTTP REST resources. Resources are chunks of data that your application passes between the server and the user ... or the backend and the frontend.

What data will your application transmit and why?  How will the resources relate to each other?

## Backend resources

On the backend, individual resources might map onto database tables ... or they might not.  Users of Rails often have the habit of thinking of a single ActiveRecord model as being synonymous with a 'resource'.  This is often true, but not always. In Rails, the resource concept maps more closely to your *routes* than to your models.  A GET to a URL (e.g. GET /my_cool_thing/42) fetches a single resource ... but the data that comes out might be provided by multiple database tables ... or none!  

## Frontend resources

Likewise, it's easy to fall into the trap of thinking a resource maps to a 'page', or to a single user interface.  But again, that's not always true.  A dashboard page might show snippets of data from dozens of different resources.

## Defining the resource format

With that in mind, we'll start by conceiving of a resource, and documenting its format.  By Xing Framework convention, documents describing our resources go in the API_DOC directory.

### A Project

Resources in Xing are a JSON object containing two keys: ```data```, which contains the actual contents of the resource, and ```links```, which identify URLs associated with the resource.  Every resource must contain a ```self``` link, identifying its own URL.  The frontend will use this URL whenever it needs to check for or fetch an updated version of the resource.

Our description of a crowdfunding project will include (for now) the following fields and types:

* Project Name
* Project Description
* Deadline
* Fundraising Goal

These files are documentation, not code, so the format is not strict. Write what is the clearest indication of meaning and use to you.

Here's our documentation for an individual project:

##### API_DOC/project
    
    // GET /project/{id} 
    //
    // A project represents a single fundraising goal: what it's
    // for and how much money it wants. As yet, it has no related
    // resources.
    {
      links: {
        self: '/project/1'
      },
      data: {
        name: 'The Xing Framework',
        description: 'A block of text describing the project',
        deadline: '2020-08-15T15:52:01+00:00',
        goal: 20000.00
      }
    }

Note the url template format /project/{id}, representing a URL with an interpolatable identifier.  This is analogous to the Rails format /project/:id, but is using the specific URI template format described in [RFC 6570](https://tools.ietf.org/html/rfc6570). We'll use the RFC 6570 format a lot in Xing, so it's worth familiarizing yourself with it. 

Here we've documented the resource by giving an example of it. You could, if you preferred, document the format by specifying the format of each value without an example:

##### API_DOC/project (alternate form)

    // GET /projects/{id} 
    //
    // A project represents a single fundraising goal: what it's
    // for and how much money it wants. As yet, it has no related
    // resources.
    {
      links: {
        self: <RFC 6570: /projects/{id}>
      },
      data: {
        name: <String>,
        description: <String>,
        deadline: <String W3C DateTime>
        goal: <Decimal number>
      }
    }
    
How you'd prefer to do that is up to you and your team.    
    
    
### A List of Projects

We're also going to need a resource representing the list of all available projects.  This one will be more complex, because the list will embed partial versions of the individual resources as well.

##### API_DOC/projects

    // GET /projects
    //
    // Represents the list of all projects in the system.  As yet
    // it does not have a defined order.
    {
      links: {
        template: '/projects/{id}'
        self: '/projects'
      },
      
      // The data in a /projects list resource is an array
      // of other resources.
      data: [
        { links: { self: '/projects/1'},
          data:  { name: 'The Xing Framework' } 
        },
        { links: { self: '/projects/2'},
          data:  { name: 'The Xing Book' } 
        },
        { links: { self: '/projects/3'},
          data:  { name: 'The Xing Website' } 
        }
      ]
    }

Notice how we are embedding the /projects/{:id} resources into the array in /projects. However, only a subset of the data is included.  This won't be a problem, however: the frontend's resource layer, however, knows how to follow that ```self:``` link in each resource to fetch the complete version of the resource when necessary.  In a Xing project, you can decide how much or how little of a related resource to embed -- usually to optimize the number of requests the frontend needs to make to retrieve important data.

A list resource in Xing aslo includes a "template" link in the link section. You'll recognize here that is has the same [RFC 6570](https://tools.ietf.org/html/rfc6570) template we used to document the individual project resource. We'll return to the purpose of this link later.