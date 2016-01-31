# Projects on the Frontend

In this section of the tutorial, we'll work in AngularJS, building views for the user to see and interact with the projects that get served by the Rails backend. 

We'll be working in the ```frontend/``` directory now, instead of the ```backend/``` directory, and using a whole different set of commands.

We're going to build several things in this section:
* Instructions for the AngularJS frontend for communicating with the backend via Hypermedia JSON.
* An application State using ui-router; this is similar conceptually to a "page" that the user might view.
* A table on that state that outputs the list of projects supplied by the back-end.

## Getting Oriented

If you've only worked in Rails before, the ``frontend/`` directory will look very unfamiliar:

    $ cd frontend
    
    $ ls -la
    drwxrwxr-x+ 19 you  staff     646 Jan 31 13:49 .
    drwxrwxr-x+ 34 you  staff    1156 Jan 31 13:10 ..
    -rw-rw-r--+  1 you  staff      25 Jan 31 13:07 .gitattributes
    -rw-rw-r--+  1 you  staff     120 Jan 31 13:07 .gitignore
    -rw-rw-r--+  1 you  staff     233 Jan 31 08:43 .jshintrc
    drwxrwxr-x+  3 you  staff     102 Jan 31 08:43 .pivotal-brancher
    -rw-rw-r--+  1 you  staff       5 Jan 31 13:07 .ruby-version
    -rw-rw-r--+  1 you  staff     195 Jan 31 08:43 .travis.yml
    -rw-rw-r--+  1 you  staff      67 Jan 31 08:43 Gemfile
    -rw-rw-r--+  1 you  staff     627 Jan 31 08:43 Gemfile.lock
    -rw-rw-r--+  1 you  staff     100 Jan 31 08:43 Gruntfile.js
    -rw-rw-r--+  1 you  staff    4130 Jan 31 08:43 build.config.js
    drwxrwxr-x+  4 you  staff     136 Jan 31 08:43 config
    drwxrwxr-x+  3 you  staff     102 Jan 31 08:43 karma
    -rw-rw-r--+  1 you  staff  361908 Jan 31 08:43 npm-shrinkwrap.json
    -rw-rw-r--+  1 you  staff    2487 Jan 31 08:43 package.json
    drwxrwxr-x+  7 you  staff     238 Jan 31 08:43 src
    drwxrwxr-x+  6 you  staff     204 Jan 31 08:43 test
    drwxrwxr-x+  3 you  staff     102 Jan 31 08:43 test-help

Like a Rails application, Xing frontend applications come with a pre-built directory structure to help organize your project.

The two directories you'll be working in with are ``src/`` and ``test/``. ``src/`` contains all of the source files for your frontend, including javascript code, html, stylesheets, and static assets. ``test/`` is for your frontend tests.

Let's take a closer look at the ``src/`` directory:

    src/
        app/
        common/
        styles/
        assets/
        index.html

In Xing, you put the javascript code for your frontend application in two folders: ``src/app/`` and ``src/common/``. ``app/`` is for the top level user interface of your application, while ``common/`` is for services and small reusable interface components. The distinction between the two will become clearer as we write the our frontend application.

Now that we've gotten a quick tour of the ``frontend/`` directory, let's dive in!