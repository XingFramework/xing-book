# HTTP Resources

Xing transmits data using JSON resources. Every resource in Xing contains two keys: ```links``` and ```data```. ```links``` provides hypermedia links between resources, and ```data``` contains the actual data of the resource itself. 

You should document the format of your particular resources in /API_DOC.

## Examples

### A plain resource

This resource describes a project, with only a couple of attributes.

```JSON
{
  links: {
    self: '/projects/3',
    tickets: '/projects/3/tickets'
  }, 
  data: {
    name: "Webappalizer",
    description: "Like Uber, but for bad startup ideas",
    deadline: "Jan 1 16:30:00 PST 2028"
  }
}
```

### A resource with nested contents

A resource can contain other resources as nested items.  When they do, the nested resources should have fully-fledged resource structure, complete with their own links.

Todo...