# The Xing Backend

Xing serves actual HTTP responses from a specialized Ruby on Rails application.
This lets us leverage experience, documentation, and the functionality provided
by Rails. In order to streamline common activities within Xing, we provide
tools to be used with Rails in a ruby gem called 'xing-backend'.

## A nomenclature note

What Xing calls "the backend" might be called "the server" or "the API"
elsewhere. We adopted backend/frontend in order to try to avoid confusing
clashes of name reuse. For instance, if the backend were called "the server"
then the frontend should be "the client" - but usually we build web apps for
people, to their specifications, and they're the client. Additionally,
libraries have client code, the backend is a client of the database...

For these reasons and more, we settled on frontend/backend to describe the
pieces of a Xing application.
