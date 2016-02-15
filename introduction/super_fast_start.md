# Super Fast Start

Feeling Lucky? Want to get going without reading anything?

1. Check that [prerequisites](/tutorial/prerequisites.md) are installed.
2. ```$ gem install xing-framework --pre```
3. ```$ xing new my_application_name```
4. ```$ cd my_application_name```
5. ```$ rake initialize  # see NOTES below```
6. ```$ rake develop```

Congratulations, your Xing app is running! 

Didn't work? Almost certainly our fault. [File a ticket!](https://github.com/XingFramework/xing-framework/issues)

NOTES: `rake initialize` is just a convenience to create a PostgreSQL database and migrate it with a users table.  If you don't want the default Xing DB schema, want to use something other than PostgreSQL, or want to manually manage your DB migrations, just skip step 5 and instead use standard Rails techniques to set up and manage your database. 

