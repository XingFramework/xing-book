# Editing the Xing Gitbook

Thanks so much for contributing to the Xing documentation. It's an essential task and role that we absolutely value and appreciate. Here's a few tips and guidelines to hopefully make the process easier.

## Local Editing

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

## File Structure

Generally, the directory structure of the book mirrors the outline. So the file for *2.5.1 Projects on the backend* is at `tutorial/projects/projects_on_the_backend.md.` The Xing book is already large (at the time of this writing close to 20,000 words, across 72 files), and it looks like it's only going to get bigger. Please try to keep to this structure.

## File Format

We've opted to use Markdown, which the original team is all very familiar with. For a variety of reasons, the text is _soft wrapped_, so please keep that in mind, especially if you edit locally.
