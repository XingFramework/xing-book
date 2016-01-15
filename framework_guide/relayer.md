# Relayer

Xing builds on a RESTful protocol, using the Xing JSON media type, in order to connect the frontend and the backend.

Relayer is a simple to use client side interface for talking to hypermedia API's. Like JS Data or Ember Data, it provides an interface to define data structures to represent your server's responses.  However, it's tailored for Xing's hypermedia conformant API and as a result more powerful. Other client side libraries consume an API by making calls to specific endpoints known ahead of time. Relayer instead consumes an API much like a user interacts with a web browser. Relayer starts at a single URL it knows ahead of time, then follows links to get to other resources. This means you can actually change all your API's endpoints without breaking your client.

Relayer makes talking to the Ruby On Rails Xing Server from your Angular.js Xing frontend easy. You define JSON resources and the relationships between those resources using a simple DSL. Then Relayer manages communication with your backend server. It figures out how to find resource URLs
