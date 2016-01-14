# Relayer

Xing builds on a RESTful protocol, using the Xing JSON media type, in order to connect the frontend and the backend.

Relayer is a simple to use client side interface for talking to hypermedia API's. It's analogous to JS Data or Ember Data, but significantly more powerful in some respects because it assumes a backend that produces a hypermedia conformant API-- namely, your API embeds links in each JSON response to get to other resources. In that respect, where most client side libraries consume an API by making calls to specific endpoints known ahead of time, Relayer instead consumes an API much like a user interacts with a web browser -- it starts at a single URL it knows ahead of time, then follows links to get to other resources. This means that in a best case scenario, you can actually change all your API's endpoints without breaking your client.
