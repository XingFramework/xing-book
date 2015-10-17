# The Xing Work Flow - Setup
Welcome to Xing!! We hope you enjoy your visit.

This is a general guide to getting a Xing project up and running, as well as some tips and tricks for using Xing.

## Let's Get Started
1. [Redis](#redis)
2. [tmux](#tmux)
3. [Initial Setup](#setup)
4. [rake-develop](#rake)

### <a name="redis"></a>Redis
You will need [redis](http://redis.io/) to use Xing.
If you don't already have it installed:
* If you use homebrew ```brew install redis```.
* If not, refer here ... http://redis.io/topics/quickstart

If you do not setup redis to auto-start, you will have to make sure you have redis running, using ```redis-server``` whenever you want to run a Xing project.

### <a name="tmux"></a>tmux
You will also need [tmux](https://tmux.github.io/).
If you don't already have it installed:
* If you use homebrew ```brew install tmux```.
* If not, refer here ... https://tmux.github.io/

### <a name="setup"></a>Initial Setup
Once you have redis and tmux installed, time to clone the project.  After you clone the project, you will probably need to:
* Copy yml.example files
* Then bundle at the root of the project, in the frontend directory and in the backend directory.
* From the backend, do the typical rails setup, i.e. run rake db:create, db:migrate, seeds and sample data.


```
rake develop
Evan wrote a dependency checker
karma test runner, auto-runs tests
quick changes, second change will not register for FE
localhost:9292
does some cool shit that I don’t understand, i.e. npm, jslint, grunt build/watch
rake spec:full
builds FE, runs all FE tests, then all BE tests
integration specs and final merge
switch to backend
run rspec as usual
for integration specs, rake spec:full to rebuild FE if you make changes
tmux stuff
what are the panes in the first place
copy example-lrd-dev-tmux.conf
ctrl-b ctrl-x, kill all sessions
ctrl-b z, zoom in on pane
ctrl-b [, search mode
pauses the server while active
upper right for indicator
q to exit search mode
ctrl-b #, switch windows
lately, disconnects on msw, just refresh karma instance
SIMBL and MouseTerm??
Test logs in BE
Request spec, change a video_response_controller.rb
API Docs
Hard to keep accurate.  Today, I changed VideoResponseSerializer, updated video_response API-Doc, but also needed to change video_response-survey API-Doc, because it also uses that serializer (nested)
```
