The Gitbook online editor is super convenient, but editing locally lets you use your own tools - the editor you use to code in all day, for instance. Plus, you can commit when you've done a meaningful change, rather than after every save. It might seem like you have to give up live updates of your edits, but it isn't so.

Gitbook provides a little editing server. If you

```
> npm install gitbook-cli -g
```

then NPM will install it for you. Then you can

```
> gitbook serve -s .
```

and it'll start a webserver on port 4000, complete with livereload. Edit files locally as you like, and they'll update in the browser when you save.
