# Running The Application

After configuration, we will need to set up the database. Xing provides a rake task to initialize this:

    $ rake initialize
    
Rake initialize is equivalent to creating, migrating, and seeding your development database.  If you prefer to do it by hand, all of Rails' standard rake tasks are available if you are inside the ```backend/``` directory.

    $ cd backend
    backend$ rake db:create
    backend$ rake db:migrate
    backend$ rake db:seed
    backend$ rake db:test:prepare