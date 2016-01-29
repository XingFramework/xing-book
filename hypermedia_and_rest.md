# Hypermedia and REST

The heart of Xing is a hypermedia JSON format for sending and receiving data between a browser client and an API server. This chapter provides a brief introduction to the core concepts behind hypermedia, REST, and the structure of our data format.

## Hypermedia

Hypermedia decouples client and server code from dependence on rigid URL structure, easing API versioning and similar difficulties. In a hypermedia application, the frontend and backend need only agree on the contents and structure of data. To find resources, a hypermedia application works the same way humans do when surfing the web: it follows URL links. Because the client application follows links rather than hard-coded URLs, the backend can restructure the API independent of the frontend.

Xing makes every attempt to be a true [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)-driven web development platform. Xing applications have a single HTTP API resource, called ```/resources```, which lists all the top-level resources available to the front-end client. The client "surfs" those links to access resources. Individual resources themselves link to related resources, but related resources can also be embedded inside larger resources to save requests. Often a properly structured Xing frontend will work either way. The backend can make these optimization decisions at any time... even on the fly!

## REST

REST (as originally intended, in the Roy Fielding sense) is about "properly" using HTTP to transfer data between client and server. Correctly structured RESTful resources implement the HTTP specification faithfully and as a result gain many benefits, including consistency, predictability of behavior, and *cacheability*.

## Further Reading

* [Learn REST: A Tutorial](http://rest.elkstein.org/)
* [Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)
* [Hypermedia](https://en.wikipedia.org/wiki/Hypermedia)