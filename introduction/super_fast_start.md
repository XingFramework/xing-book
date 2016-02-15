# Super Fast Start

Feeling Lucky? Want to get going without reading anything?

1. Check that [prerequisites](/tutorial/prerequisites.md) are installed.
2. ```$ gem install xing-framework --pre```
3. ```$ xing new my_application_name```
4. ```$ cd my_application_name```
5. ```$ rake initialize```
6. ```$ rake develop```

Congratulations, your Xing app is running! 

Didn't work? Almost certainly our fault. [File a ticket!](https://github.com/XingFramework/xing-framework/issues)

NOTES: `rake initialize` is just a convenience to create a PostgreSQL database and migrate it with a users table.  If you aren't ready to create a DB or want to use something other that PostgreSQL, simply skip step 5. 

