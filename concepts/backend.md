# The Xing Backend

Xing serves actual HTTP responses from a specialized Ruby on Rails application.  This lets us leverage experience, documentation, and the functionality provided by Rails. In order to streamline common activities within Xing, we provide tools to be used with Rails in a ruby gem called 'xing-backend'.

## A nomenclature note

What Xing calls "the backend" might be called "the server" or "the API" elsewhere. We adopted the backend/frontend in order to try to avoid confusing clashes of name reuse. For instance, if the backend were called "the server" then the frontend should be "the client" - but usually we build web apps for people, to their specifications, and they're the client. Additionally, libraries have client code, the backend is a client of the database...  Likewise, "server" can mean not just the backend software, but also the system httpd software (like Apache) or the computer it runs on. 

For these reasons and more, we settled on frontend/backend to describe the main components of a Xing application.

## The job of the backend

The Xing backend is responsible for moving data in and out of a database, and providing functionality that can't be done safely or at all in the end-user's device. This may include:

* Authentication
* Authorization
* Integration with external APIs
* Data processing 
* Other stuff

## Architecture of the backend

The xing backend uses 'Serializers' to convert database data to JSON format for transmission, and 'Mappers' to consume JSON data and make writes to the database.  These are classes that live in backend/app, parallel to Rails' models and controllers. 

![](Xing-backend-architecture.png)

In the diagram above, arrows show the direction of data flow: incoming JSON 

Nearly all of the interesting logic in a Xing application happens in these classes - generally the models and controllers are thin.



