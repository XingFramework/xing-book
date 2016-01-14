# Configuration

## Backend Config

Copy the database.yml.example and secrets.yml.example file

    $ cd backend/
    backend$ cp config/database.yml.example config/database.yml
    backend$ cp config/secrets.yml.example config/secrets.yml
    
Make any necessary edits to your database.yml and secrets.yml files.

If you want the Ruby module for the application to match the name of the app, change the module and APP_MODULE.
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

