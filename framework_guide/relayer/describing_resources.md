## Defining the Top Level


Relayer starts consuming the API from a single well known URL.

The first step to using Relayer is to define some resources to represent responses from your backend API. You start by importing Relayer as RL.

To do this, we need to define a top level resource that represents response return from the well known URL. Usually, in relayer, the well known URL should return a response that is basically a list of links to other resorces. Here's an example top level resource:

```javascript
class Resources extends RL.Resource {

}

RL.Describe(Resources, (desc) => {
  var blogPosts = desc.hasList("blogPosts", BlogPost);
  blogPosts.linkTemplate = "blogPost";
  blogPosts.canCreate = true;
  var authors = desc.hasList("authors", Author);
  authors.linkTemplate = "author";
  authors.canCreate = false;
})
```
## Other Resources

The Resources resource description refers to other classes. Where do they come from? They get described in exactly the same way. Usually, they're simpler descriptions, but the fundamentals are the same.

```javascript
import RL from "relayer";
```

Then you define some resource classes:
```javascript
class Author extends RL.Resource {

}
class BlogPost extends RL.Resource {

}
class Comment extends RL.Resource {

}
```
These classes stay empty unless you need to define computed properties or other helper methods.

Instead, the bulk of the work in defining resources is when you use Relayer to describe the structure of the JSON for each of these resources on the backend.

```javascript
RL.Describe(Author, (desc) => {
  desc.property("nickname", "");
  desc.property("name", "");
  desc.hasList("blogPosts", BlogPost);
});

RL.Describe(Blog, (desc) => {
  desc.property("publishedAt", new Date());
  desc.property("body", "");
  desc.hasOne("author", Author);
  desc.hasList("comments", Comment);
});

RL.Describe(Comment, (desc) => {
  desc.property("body", "");
  desc.property("postedAt", new Date());
  desc.hasOne("blogPost", BlogPost)
})
```

## The Describe Interface

RL.Describe takes two parameters-- the Resource Class you're describing and a callback function used to describe the resource. The callback functions only parameter is the object you'll be calling methods on to describe the resource. Some of the methods available to you are:

`property` - set a data property for the resource. This is the standard way to access simple data properties for this type of resource. It takes the name of the property (in `lowerCamel` -- it will get converted to `snake_case` before it goes out to the backend), and an initial value.

`hasOne` - tell relayer that this resource is linked to a single instance of another type of resource. This means the JSON returned from the server will either embed the other resource or provide a link to it.

`hasList` - tells relayer that this resource is linked to another resource which is itself a list of resources. This means the JSON returned from the server will either embed the list of resources or provide a link to get the list of resources
