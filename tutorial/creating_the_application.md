# Creating The Application

## Getting the Code (optional)

TODO:  Once a canonical repository has been created with a completed version of the app, instructions for cloning the repo and checking out the tag for this page go here.

## Installing the Xing Framework rubygem

We'll start by installing xing-framework. Make sure you're using a compatible version of Ruby (we prefer >= 2.2).   

RVM USERS NOTE: If you use RVM's [gemset](https://rvm.io/gemsets/basics) feature, you can invoke xing new with the flag --with-gemset so xing will create .ruby-gemset files with the same name as your application. If you're doing that, you might also want to create the gemset before you even install the xing-framework gem, switch to it, and install xing-framework there so everything is in a single set. If you're not using gemsets, don't worry about it.

RBENV USERS NOTE: If you are using rbenv, you will need to run `rbenv rehash` AFTER installing xing-framework.

## Generating the application

    $ gem install xing-framework
    Fetching: xing-framework-0.2.7.gem (100%)
    Welcome to the exciting new world of Xing web development!
    Successfully installed xing-framework-0.2.7
    1 gem installed
    
Let's create a new Xing project! CAUTION: the `xing new` will automatically generate directories and bundle a lot of gems. If you're worried about a tool automatically installing gems on your system, you should read ahead or practice on an isolated system.

    $ xing new crowdfundr
    
    # OR if using rvm with gemsets:
    $ xing new --with-gemset crowdfundr

This command should take a few minutes to run.  It will generate a blank application in a subdirectory called ```crowdfundr```, change to it, and proceed to install the rubygems and npm modules necessary to support Xing development.

*** When you run xing new, you'll be asked whether you want to add a Code of Conduct to your project. If you don't want one, just say no. If you're curious why you might want to add a Code of Conduct to your project, you can read more about them [here](http://contributor-covenant.org/).***

## Your empty application

Let's take a quick look at the files that were created.
    
    $ cd crowdfundr

    $ ls -la
    total 120
    drwxr-xr-x  22 you  staff   748 Jan 13 16:59 .
    drwxr-xr-x   7 you  staff   238 Jan 13 15:36 ..
    -rw-r--r--   1 you  staff    11 Jan 13 15:41 .ruby-gemset
    -rw-r--r--   1 you  staff     6 Jan 13 16:58 .ruby-version
    -rw-r--r--   1 you  staff  4987 Jan 13 15:35 CHANGELOG.md
    -rw-r--r--   1 you  staff   798 Jan 13 15:35 Capfile
    -rw-r--r--   1 you  staff   136 Jan 13 15:35 Gemfile
    -rw-r--r--   1 you  staff  3704 Jan 13 15:35 Gemfile.lock
    -rw-r--r--   1 you  staff  1421 Jan 13 15:35 README.md
    -rw-r--r--   1 you  staff   647 Jan 13 15:35 Rakefile
    drwxr-xr-x  19 you  staff   646 Jan 13 16:58 backend  
    -rw-r--r--   1 you  staff   181 Jan 13 15:35 codeclimate.yml
    drwxr-xr-x   5 you  staff   170 Jan 13 15:35 config
    drwxr-xr-x   5 you  staff   170 Jan 13 15:35 doc
    -rw-r--r--   1 you  staff   232 Jan 13 15:35 dump.rdb
    -rw-r--r--   1 you  staff   561 Jan 13 15:35 example-lrd-dev-tmux.conf
    drwxr-xr-x  20 you  staff   680 Jan 13 16:59 frontend
    drwxr-xr-x   3 you  staff   102 Jan 13 15:35 lib
    -rw-r--r--   1 you  staff   121 Jan 13 15:35 static-app.ru
    -rwxr-xr-x   1 you  staff  2041 Jan 13 15:35 tmux-windows
    -rw-r--r--   1 you  staff  2078 Jan 13 15:35 tmux-windows.txt

The most important subdirectories to notice for now are:
* **frontend**, which contains the AngularJS frontend code
* **backend**, which contains the Rails API backend code
