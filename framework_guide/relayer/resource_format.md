# Resource Format

In it's very first version, Relayer only supports a single custom JSON format for your API server. The json format is as follows:

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

{
  data: {},
  links: {
    blog_posts: "/blog_posts",
    blog_post: "/blog_posts/{id}" // URI Template to lookup a single blog post,
    authors: "/authors",
    author: "authors/{nickname}" // URI template to lookup a single author
  }
}

#### Why This Format?

In order for relayer to work its magic, the API it talks to must provide a minimum level of information so that Relayer can consume the entire API from a single well known endpoint. The data format here was chosen because it is used in an upcoming Rails and AngularJS web framework we are also writing called Xing. While all of Xing is not yet released, if you are using Rails, the xing-backend gem will make it very easy to generate API's in the format expected by Relayer. It's also a great tool for building API backends in Rails.

*** This is the format for the first version -- in time Relayer will support well-known formats like JSON API, JSON-LD, and HAL ***
