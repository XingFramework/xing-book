# Super Fast Start

Feeling Lucky? Want to get going without reading anything?

1. Check that [prerequisites](/introduction/prerequisites.md) are installed
2. ```$ gem install xing-framework```
3. ```$ xing new my_application_name```
4. ```$ cd my_application_name/backend```
5. ```$ cp config/database.yml.example config/database.yml```
6. ```$ cp config/secrets.yml.example config/secrets.yml```
5. edit ```database.yml``` to make a database
7. ```$ rake db:create db:migrate db:seed```
8. ```$ cd ..```
9. ```$ rake develop```

Congratulations, your Xing app is running! 

Didn't work? Almost certainly our fault. [File a ticket!](https://github.com/XingFramework/xing-framework/issues)
