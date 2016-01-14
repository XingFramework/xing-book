# Creating The Application

## Installing the Xing Framework rubygem

We'll start by installing xing-framework. Make sure you're using a compatible version of Ruby (we prefer >= 2.2).  If you're using a Ruby manager with gemsets (like RVM), you probably want to [create a gemset now](https://rvm.io/gemsets/basics) and switch to it. If you're not using gemsets, don't worry about it.

## Generating the application

    $ gem install xing-framework
    Fetching: xing-framework-0.2.0.gem (100%)
    Welcome to the exciting new world of Xing web development!
    Successfully installed xing-framework-0.2.0
    1 gem installed
    
Let's create a new Xing project! Be aware that `xing new` will automatically generate directories and bundle a lot of gems.

    $ xing new crowdfundr

This command should take a few minutes to run.  It will generate a blank application in a subdirectory called ```crowdfundr```, change to it, and proceed to install the rubygems and npm modules necessary to support Xing development.

## Your empty application

Let's take a quick look at the files that were created.

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
