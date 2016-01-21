# Prerequisites

On MacOS, you should be able to install most or all of these with homebrew.  On Linux, refer to the package manager documentation for your distribution. 

## Prerequisite Knowledge

Most of this tutorial expects that you have at least a passing knowledge of Ruby on Rails and/or AngularJS. We will refer to basic constructs, conventions, and patterns of both frameworks.  If you're unfamiliar with either tool, we recommend spending some time with the many excellent tutorials and documentation available elsewhere.

* [Michael Hartl's Rails Tutorial](https://www.railstutorial.org/)
* [AngularJS Tutorial](https://docs.angularjs.org/tutorial)

When we refer to concepts without explaining them, this tutorial will always try to provide outside links and references.  However, it's a work in progress

## Prerequisite Software

Your system will need the following software installed in order to begin this tutorial.

### [Ruby](https://www.ruby-lang.org/en/)

Your system must have a compatible Ruby installed. Xing should work on any system that can run Rails 4.2. However, at this time we have only tested it with Ruby 2.2.

In the application generation process, Xing will create .ruby-version files with a '2.2' specification.  You may alter these to your liking.

### [npm](https://www.npmjs.com/)

A current version of npm is required.

### [postgresql](http://www.postgresql.org/) 

By default, Xing projects use a PostgreSQL database as their primary data storage. However, Xing should be compatible with any database that can be used by Ruby on Rails.

### [Redis](http://redis.io/)

Xing will, by default, assume your app will be using Redis and [Sidekiq](http://sidekiq.org/) for asynchronous and delayed job processing.  To enable that, Redis >= 2.8 should be installed on your system. (Xing tools will check to confirm that it is installed).  If you do not wish to use these tools you may need to modify config files in your application before running. 

### [tmux](https://tmux.github.io/) (terminal multiplexer)

tmux is not strictly required, but is highly recommended.  If present, the Xing development launcher will build a five-pane interface simultaneously showing the console output from all five processes a Xing application needs:

* Assets server
* Rails/backend server
* Grunt watch / JavaScript build / Jasmine frontend tests
* Compass watch / CSS build
* Redis / Sidekiq

If tmux is not available, Xing's development tools will still launch the processes for you, but their output will be interleaved in a single console.

If you have tmux installed, it's worth understanding its common command structure (CTRL-b is the default command prefix, if you haven't changed tmux preferences) and knowing the following commands:

* <prefix> CTRL-x  -- kill your session
* <prefix> <arrow keys> -- move between panes
* <prefix> z -- zoom current pane (toggle between pane and fullscreen)
* <prefix> [ -- enter history mode (arrow keys will now scrollback)
* <prefix> n -- next window (particularly useful if your screen is too small for tmux to show all five panes at once)

A few minutes with the tmux documentation and/or a tutorial will go a long way to helping you get more out of Xing's developer console.



 

