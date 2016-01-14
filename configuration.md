# Configuration

A freshly generated bare Xing Framework project will contain some generic configuration that you'll want to customize for your project.  Here we'll do the necessary customization for our Crowdfundr application. 

## Backend Config

### YAML files

The backend is mostly a standard Ruby on Rails 4.2 application, and so needs configuration in backend/config/database.yml and backend/config/secrets.yml.  It's bad practice to commit these files to your version control repository, so we list them in .gitignore and provide example files you can copy and edit:

    $ cd backend/
    backend$ cp config/database.yml.example config/database.yml
    backend$ cp config/secrets.yml.example config/secrets.yml
    
Make any necessary edits to your database.yml and secrets.yml files. Most of the secrets.yml file should be fine for development, until you need to test sending outgoing email. In database.yml you'll want to create a unique database for development and test.  Something like this should be sufficient:

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
  host: 127.0.0.1```

### Application config

If you want the Ruby module for the application to match the name of the app,  modify backend/config/application.rb to create a properly-named module (instead of XingApp) and set the constant APP_MODULE. 
######  backend/config/application.rb

    module Crowdfundr; end
    APP_MODULE = Crowdfundr

## Frontend Config

Change the following fields
###### frontend/package.json

    "name": "Crowdfundr",
    "version": "0.0.1",
    "author": "Your Name",
    "homepage": "http://crowdfundr.com"
   
Change the appName 
###### frontend/src/common/config.js

    export var appName = "Crowdfundr";

