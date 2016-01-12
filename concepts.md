# Concepts: Hypermedia and REST

## Introduction

At the heart of Xing is a Hypermedia JSON format for sending and receiving data between a browser client and an API server. This is the basic building block upon which we've built Xing's Ruby on Rails backend and its AngularJS frontend. This chapter introduces the core concepts and structure of our data format.

A full discussion of Hypermedia and REST are far beyond the scope of this document, but we provide a brief introduction here 

## Hypermedia

At its core, Hypermedia is about decoupling client and server code from dependence on rigid URL structure: this sort of coupling leads to difficult API versioning and all sorts of development difficulty down the road.  Instead, in a hypermedia application front and back-end code need only agree on the contents and structure of data. To find resources, a hypermedia application works the same way humans do when surfing the web: it follows URL links. Because the client application follows links rather than visits hardcoded URLs, the back-end can restructure the API to a large extent, independently of the frontend. 

In a Xing application, a single HTTP API resource called /resources lists the top-level resources available to a front-end client.  The client "surfs" those links to access resources. Individual resources themselves link to related resources.  Related resources  can be separate (and linked to), or embedded inside larger resources (to save requests), and in many cases a properly structured Xing frontend will work either way.  The back-end can make these optimization decisions at any time ... even on the fly!

If you're familiar with the acronym HATEOAS (hypermedia as the engine of application state), Xing makes every attempt to be a true HATEOAS-driven web development platform.

## REST

REST (as originally intended, in the Ray Fielding sense) is about "properly" using HTTP to transfer data between client and server.  Correctly structured RESTful resources implement the HTTP specification faithfully and as a result gain many benefits, including consistency, predictability of behavior, and *cacheability*.

## Further Reading

* [Learn REST: A Tutorial](http://rest.elkstein.org/)
* [Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)
* [Hypermedia](https://en.wikipedia.org/wiki/Hypermedia)