# HTTP Resources

Xing transmits data using JSON resources. Every resource in Xing contains two keys: ```links``` and ```data```. ```links``` provides hypermedia links between resources, and ```data``` contains the actual data of the resource itself. 

When creating a resource in a Xing application, we recommend documenting the format in ```/API_DOC``` for later reference.

## Examples

The code blocks below are examples of API documentation for a Xing project, but also should help visualize the JSON response data in a Xing application.

### Resource

This resource describes a simple project, with only a few attributes.

```JSON
# GET /projects/:id

{
  links: {
    self: '/projects/:id',
    tickets: '/projects/:id/tickets'
  }, 
  data: {
    name: <string>,
    description: <string>,
    deadline: <datetime>
  }
}
```

### Nested Resource

A resource can contain other resources as nested items.  When they do, the nested resources should have fully-fledged resource structure, complete with their own links.

```JSON
#GET /projects/:id

{
  links: {
    self: '/projects/:id'
  },
  data: {
    name: <string>,
    description: <string>,
    deadline: <datetime>,
    tickets: {
      links: {
        self: '/tickets/:id/'
      },
      data: {
        title: <string>,
        description: <string>
      }
    }
  }
}
```