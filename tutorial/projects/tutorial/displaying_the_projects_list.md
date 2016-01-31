# Displaying the projects list

Now that we can load our projects, we'd like to show them to the user somewhere. For the moment, let's just list them on the homepage.

## About States

Xing applications use UI-router to describe the user interface as a tree of states. States are a convenient way to organize both functionality and appearance and styling.  The default state tree of a new Xing project looks like this:



You're not tied to this structure at all: you can write your application's states however you like. This is just a set of defaults Xing gives you to get you started.

States have a full name created from their position in the tree, separated by dots.  For example, the default "starting state" for this application is `root.homepage.show`. 

Homepage and Inner are different because most applications have a homepage or landing page that's very different in layout and style from the rest of the site, and Xing's default approach to CSS makes it easy to add rules that apply to all of the states within one branch.

We'll talk more about states in a bit, but for now we're going to add code t
