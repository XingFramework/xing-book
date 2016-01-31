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
    drwxrwxr-x+ 19 hannah  rvm     646 Jan 31 13:49 .
    drwxrwxr-x+ 34 hannah  rvm    1156 Jan 31 13:10 ..
    -rw-rw-r--+  1 hannah  rvm      25 Jan 31 13:07 .gitattributes
    -rw-rw-r--+  1 hannah  rvm     120 Jan 31 13:07 .gitignore
    -rw-rw-r--+  1 hannah  rvm     233 Jan 31 08:43 .jshintrc
    drwxrwxr-x+  3 hannah  rvm     102 Jan 31 08:43 .pivotal-brancher
    -rw-rw-r--+  1 hannah  rvm       5 Jan 31 13:07 .ruby-version
    -rw-rw-r--+  1 hannah  rvm     195 Jan 31 08:43 .travis.yml
    -rw-rw-r--+  1 hannah  rvm      67 Jan 31 08:43 Gemfile
    -rw-rw-r--+  1 hannah  rvm     627 Jan 31 08:43 Gemfile.lock
    -rw-rw-r--+  1 hannah  rvm     100 Jan 31 08:43 Gruntfile.js
    -rw-rw-r--+  1 hannah  rvm    4130 Jan 31 08:43 build.config.js
    drwxrwxr-x+  4 hannah  rvm     136 Jan 31 08:43 config
    drwxrwxr-x+  3 hannah  rvm     102 Jan 31 08:43 karma
    -rw-rw-r--+  1 hannah  rvm  361908 Jan 31 08:43 npm-shrinkwrap.json
    -rw-rw-r--+  1 hannah  rvm    2487 Jan 31 08:43 package.json
    drwxrwxr-x+  7 hannah  rvm     238 Jan 31 08:43 src
    drwxrwxr-x+  6 hannah  rvm     204 Jan 31 08:43 test
    drwxrwxr-x+  3 hannah  rvm     102 Jan 31 08:43 test-help
    
Let's dive in!