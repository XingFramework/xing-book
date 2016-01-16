# Running The Application

## Migrate the database

After configuration, we will need to set up the database. While in the /backend dir:

    backend$ rake db:create
    backend$ rake db:migrate
    backend$ rake db:seed
    backend$ rake db:test:prepare

## Build and boot the app
    
Once your DB is created and migrated, you are ready to boot the application! Go back to the root directory and run ```rake develop```.

    backend$ cd ..
    backend$ rake develop
    
Five processes will start up: a Grunt JS build/watch, a compass CSS build/watch, a Webrick assets server, Rails, and a Sidekiq queue.  Two browser windows will launch: a Karma window (used by the JS build) and http://localhost:9292, the address of your assets server.  The latter should load index.html, which will pull in your compiled Javascript bundle and CSS.  If everything worked, your terminal should look like this:



And your application should look like this:
