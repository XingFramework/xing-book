# Sample Data

Available rake tasks include:

* ```rake db:sample_data:load``` loads all data
* ```rake db:sample_data:wipe``` erases all tables
* ```rake db:sample_data:reload``` wipes tables and runs :load
* ```rake db:sample_data:recycle``` destroys and rebuilds the whole DB then runs ```:load```

To add more sample data tasks, define files in ```backend/db/sample_data```. Each should have a ```.rake``` extension and the following structure. If your database table is called 'books' and you have an ```ActiveRecord``` model ```Book```, it would look like

##### ```backend/db/sample_data/books.rake

    namespace :db do
      namespace :sample_data do
        namespace :books do
            task :wipe do
              Book.delete_all
              # plus any other logic necessary for a clean delete
            end
            
            task :load do
              # any logic necessary to manufacture some random records
              # for this database table
              10.times do 
                Book.create( ... values ...)
              end
            end
        
        task :wipe => 'books:wipe'
        task :load => 'books:load'
      end
    end
  
The last two lines attach ```db:sample_data:books:wipe``` and ```db:sample_data:books:load``` as dependencies of the overall ```db:sample_data:load``` and ```:wipe``` tasks.


TODO: Further documentation on the Sample Data architecture.

