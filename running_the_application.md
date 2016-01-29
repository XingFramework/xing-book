# Running The Application

After configuration, we will need to set up the database. Xing provides a rake task to initialize this:

    $ rake initialize

Rake initialize is equivalent to creating, migrating, and seeding your development database.  If you prefer to do it by hand, all of Rails' standard rake tasks are available if you are inside the ```backend/``` directory.

    $ cd backend
    backend$ rake db:create db:migrate db:seed db:test:prepare

_Note: there's no need to do both! If you run `rake initialize` it will do everything that the above, more traditional, Rails command would do_

## Meet The Xing Developer's Console

Boot a Xing project in development requires five different processes: three servers and two build watchers. Xing ships with a launcher toolkit that will start all five of these simultaneously and open your app in a browser.

If you've completed configuration, you should be able to run just this one command from the root of the project directory:

    $ rake develop

The launcher will start up all five of these, opening them all in different panes of a Xing

* Assets server
* Rails/backend server
* Grunt watch / JavaScript build / Jasmine frontend tests
* Compass watch / CSS build
* Redis / Sidekiq

![](/xing console panel.png)

If tmux is not available, Xing's development tools will still launch the processes for you, but their output will be interleaved in a single console.

## Navigating the Console with tmux

If you have tmux installed, it's worth understanding its common command structure.  Most tmux commands are of the form [prefix] [keystroke], and CTRL-b is the default command prefix, if you haven't changed tmux preferences.
Here are a few commands to get you around the basics.

* [prefix] x  - kill your current pane
* [prefix] &  - kill your current window
* [prefix] [arrow key] - move between panes
* [prefix] z - zoom current pane (toggle between pane and fullscreen)
* [prefix] [ - enter history mode (arrow keys will now scrollback)
* [prefix] n - next window (particularly useful if your screen is too small for tmux to show all five panes at once)

A few minutes with the tmux documentation and/or a tutorial will go a long way to helping you get more out of Xing's developer console.

### A handy tmux config tip

All of Xing's core developers have the following line in a ```~/.tmux.conf``` file. If you have it, ```[prefix] CTRL-x``` will prompt you to terminate your entire Xing session:

```bind-key C-x confirm-before -p "kill-session? (y/n)" kill-session```
