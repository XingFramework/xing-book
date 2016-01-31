# Sample Data

We're not going to build the resources for creating and editing our projects just yet, because we'd like to get to the frontend so you can have a working application to look at.

But we'd like to have some projects in our database when we get there. Fortunately the Xing includes a standard pattern for creating rake tasks that generate sample data: synthetic, randomized data you can use to view and demo your application as if the database were already populated by users.

There's more on how sample data tasks work [later on in the Framework guide](framework_guide/backend/sample_data.md). For now, just create the following file:

##### ```backend/db/sample_data/projects.rake```

```ruby
namespace :db do
  namespace :sample_data do
    namespace :projects do

      desc "Remove all projects"
      task :wipe do
        Project.delete_all
      end

      desc "Populate the DB with fake projects."
      task :load do
        return if Project.count > 0
        10.times do |n|
          Project.create(
            :name =>        Faker::Lorem.words(2..5).join(" ").titleize,
            :description => Faker::Lorem.sentences(1..6).join(" "),
            :deadline =>    (Date.today + rand(20).days).end_of_day,
            :goal =>        (2 + rand(20)) * 5000.00
          )
        end
      end
    end

    task :wipe => 'projects:wipe'
    task :load => 'projects:load'
  end
end
```


Then go to your command line (still in the backend/ subdirectory) and run this command:

    backend$ bundle exec rake db:sample_data:load
    
When you're done, your DB should have 10 fake projects in it. Let's go build a front-end to see them!

## Other Reading

* This sample data task uses the excellent gem [faker](https://github.com/stympy/faker).


