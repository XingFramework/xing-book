# Configuration

A freshly generated bare Xing Framework project will contain some generic configuration that you may want to customize for your project. However, the default configuration will probably work for you. If you're in a hurry, you can skip this page.

## Backend Config

### YAML files

The backend is mostly a standard Ruby on Rails 4.2 application, and so needs configuration in backend/config/database.yml and backend/config/secrets.yml.  It's bad practice to commit these files to your version control repository, so Xing templates those files for you but also lists them in .gitignore.  

So that future developers who clone the project have an easy source to work from, Xing also templates the two files backend/config/database.yml.example and backend/config/secrets.yml.example. These files should be kept in version control, so
in your own projects you may want to edit the *.example versions to provide useful but safe defaults as a starting point for the rest of your team.
    
The defaults should be suitable for basic development, but go ahead and examine your database.yml and secrets.yml files and make any necessary edits. Defaults look like the following:

###### backend/config/database.yml
```yaml 
development: &development
  adapter: postgresql
  encoding: unicode
  database: crowdfundr_dev 
  pool: 5
  host: 127.0.0.1

test:
  <<: *development
  database: crowdfundr_test

production:
  adapter: postgresql
  encoding: unicode
  database: crowdfundr_dev
  pool: 5
  host: 127.0.0.1
```

### Application config

A few places in the application use generic names you might want to customize to reflect the actual name of your app. 

## Ruby application module

If you want the Ruby module for the application to match the app name,  modify backend/config/application.rb to create a properly-named module (instead of XingApp) and set the constant APP_MODULE. 

######  backend/config/application.rb

    module Crowdfundr; end
    APP_MODULE = Crowdfundr

## Frontend Config

A couple of configuration files in the frontend are worth customizing as well, so that (for example) your compiled JavaScript application will have a name like Crowdfundr-0.0.1.js instead of XING-BASE-0.0.1.js.

Change the following files as follows:

###### frontend/package.json

    "name": "Crowdfundr",
    "version": "0.0.1",
    "author": "Your Name",
    "homepage": "http://crowdfundr.com"
   
###### frontend/src/common/config.js

    export var appName = "Crowdfundr";

## Ruby and Gems configuration

By default, Xing will create a .ruby-version file in the root of your application, as well as in frontend/ and backend/.  If you are using a different version than the default, you should edit all three files. In addition, if you are using a system like RVM that manages separate gemsets, you should create identical .ruby-gemset files in all three directories. 