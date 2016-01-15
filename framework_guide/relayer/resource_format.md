# Data Format

Xing uses a custom hypermedia JSON type to transmit data between client and server. The json format is as follows:

1. All JSON data has two top level keys -- data and links. Data contains the actual information about the primary resource. Links contains urls for related resources. Links should have at least one key -- a "self" property -- which has the canonical URL for the resource.

2. Inside the data key, in addition to primary resource data, any key which itself has data and links keys is considered an embedded resource, and must be a partial or whole representation of a resource at a different HTTP endpoint.

So for example, a blog post with comments and an author. The author is embedded, the comments are only linked:

```javascript
{
  data: {
    published_at: "02/21/1848",
    body: "In bourgeois society capital is independent and has individuality, while the living person is dependent and has no individuality.",
    author: {
      data: {
        nickname: "karlm",
        name: "Monsieur Ramboz",
      },
      links: {
        self: "/authors/karlm",
        blog_posts: "/authors/karlm/blog_posts"
      }
    }
  },
  links: {
    self: "/blog_posts/1",
    comments: "/blog_posts/1/comments",
    author: "/authors/karlm"
  }
}
```

3. The "self" link is the "primary key". While resources may have attributes named "id" in the body, Relayer always assumes the unique identifier for a resource is the provided link to itself. After all, at the end of the day, a URL remains the cononical unique id for a resource on the web.

4. The API must provide URI templates for looking up specific resources from smaller bits of data. This provides short cuts to get to specific resources, primarily so that a Javascript client app can translate it's routed URLs to backend api calls easily, without sacrificing the flexibility of the API. Here is an example of a resource that returns URI templates for other endpoints in the app:

```javascript
{
  data: {},
  links: {
    blog_posts: "/blog_posts",
    blog_post: "/blog_posts/{id}" // URI Template to lookup a single blog post,
    authors: "/authors",
    author: "authors/{nickname}" // URI template to lookup a single author
  }
}
```

A resource can have a "self_template" link, which is a template for shortening it's "self" link.

#### Supported Collection Formats

Xing supports three ways of describing collections of resources:

1. Xing Lists

A Xing list is a Xing resource where the data key is an array of resources. In a Xing list, the links section should include a "template" key which is a template for individual items in a Xing list.

Example:

```javascript
{
    data: [
        {
            data: {
                published_at: "02/21/1848",
                body: "In bourgeois society capital is independent and has individuality, while the living person is dependent and has no individuality."
            },
            links: {
                self: "/blog_posts/1",
                comments: "/blog_posts/1/comments",
                author: "/authors/karlm"
            }
        },
        {
            data: {
                published_at: "06/01/1970",
                body: "You will not be able to stay home, brother. You will not be able to plug in, turn on and cop out. You will not be able to lose yourself on skag. And skip out for beer during commercials. Because the revolution will not be televised."
            },
            links: {
                self: "/blog_posts/2",
                comments: "/blog_posts/2/comments",
                author: "/authors/gilscotth"
            }
        }
    ],
    links: {
        self: "/blog_posts",
        template: "/blog_posts/{id}"
    }
}
```

2. Xing Arrays

A Xing Array is an array of resources embedded as a key inside of another resource. Unlike a Xing list, the array is not itself a resource on its own, and must be embedded in another resource.

Example (comments key):

```javascript
{
    data: {
        published_at: "06/01/1970",
        body: "You will not be able to stay home, brother. You will not be able to plug in, turn on and cop out. You will not be able to lose yourself on skag. And skip out for beer during commercials. Because the revolution will not be televised.",
        comments: [
            {
                data: {
                    body: "The revolution will not fight the germs that may cause bad breath",
                    likes: true
                },
                links: {
                    self: "blog_posts/2/comments/3"
                }
            },
            {
                data: {
                    body: "The revolution will be no re-run brothers",
                    likes: true
                },
                links: {
                    self: "blog_posts/2/comments/4"
                }
            }
        ]
    },
    links: {
        self: "/blog_posts/2",
        author: "/authors/gilscotth"
    }
}
```

3. Xing "Maps"

A Xing "Map" is a javascript object where each key in the object is a resource. The object is itself embedded as a key inside of another resource. A Xing Map is a not a resource on its own -- it must be embedded.

Example (a blog post composed of different blocks of content, each content is a resource):

```javascript
{
    data: {
        contents: {
            left_column: {
                data: {
                },
                links: {
                }
            },
            right_column: {
                data: {
                },
                links: {
                }
            },
            css: {
                data: {
                },
                links: {
                }
            }
        },
        published_at: "06/01/1970",
    },
    links: {
    }
}
```
#### Why This Format?

In order for relayer to work its magic, the API it talks to must provide a minimum level of information so that Relayer can consume the entire API from a single well known endpoint. The data format here was chosen because it is used in an upcoming Rails and AngularJS web framework we are also writing called Xing. While all of Xing is not yet released, if you are using Rails, the xing-backend gem will make it very easy to generate API's in the format expected by Relayer. It's also a great tool for building API backends in Rails.

*** This is the format for the first version -- in time Relayer will support well-known formats like JSON API, JSON-LD, and HAL ***
