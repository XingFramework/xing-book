# Running The Application

After configuration, we will need to set up the database. While in the /backend dir:

    $ cd backend
    backend$ rake db:create
    backend$ rake db:migrate
    backend$ rake db:test:prepare