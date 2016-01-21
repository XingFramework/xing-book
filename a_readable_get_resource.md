# A readable GET resource

Lets start with just the individual ```/project/{id}``` resource.

## Test first! (write a request spec)

We're going to start by writing a spec.  (You do write your specs first ... right?) We'll start with an integration-level spec that tests that the entire backend produces JSON as desired when given the proper HTTP request, and use that as a framework to guide our unit tests and code.

We'll use [FactoryGirl](https://github.com/thoughtbot/factory_girl) to manufacture a Project, and submit an HTTP request to to the proper URL, and inspect the returned JSON.

##### ```backend/spec/requests/project_get_spec.rb```

    require 'spec_helper'

    describe 'GET /project', :type => :request do
      let! :project do
        FactoryGirl.create(:project,
                           :name => "The Xing Framework",
                           :description => "Cool new web framework!"
                          )
      end

      let! :resource_url do
        "/projects/#{project.id}"
      end

      it 'returned JSON is correctly formatted' do
        # fetch the resource using Xing's json_get helper
        json_get resource_url

        # Response should have a 2XX HTTP code
        expect(response).to be_success


        body = response.body

        # check that the self link, name, and description appear at the proper locations
        # within the returned JSON
        expect(body).to be_json_string(resource_url).at_path('links/self')
        expect(body).to be_json_string(project.name).at_path('data/name')
        expect(body).to be_json_string(project.description).at_path('data/description')
      end
    end

What's going on in this example:  the first let block creates a project in the database (assuming a table and ActiveRecord model exist), and the second assembles the URL for the resource based on  the format we identified in our API doc.  

In the expectation:
* json_get is a Xing helper that makes a GET request for the specified URL, while setting the correct HTTP headers to specify a JSON response.
* be_json_string is a Xing matcher that looks for a properly-quoted  string in the JSON response, optionally at a specified location if you attach .at_path().  This is an extension of the matchers in the (excellent) [json_spec](https://github.com/collectiveidea/json_spec) gem.

Note that we didn't test for every value in the JSON part of the spec. The level of detail you want to test is up to you.  Our practice (the authors of Xing) is to check the overall success of the response and spot-check a bit of the data in the request spec, and then test the full detail of the JSON   

If you run this spec, it'll error out because the Rails app has no routes for '/project/{id}', no Project model, no database table, no Factory, etc.  Cool, a failing spec is what we want at this point. 

## Model and Migration

Let's use Rails' model generator to quickly make us a Project model.

    $ cd backend
    backend$ bundle exec rails generate model project name:string description:text deadline:datetime goal:decimal
    
That'll generate a model, spec, and empty factory file.  There will be no behavior in the Project model (at least not yet), so there's nothing to test in the backend/spec/models/project_spec.rb. We can leave those files alone for the time being.  

This would be a good time to migrate the database to populate our new model:

    backend$ bundle exec rake db:migrate

Let's fill out that factory, then, so our spec can run:

##### backend/spec/factories/project_factory.rb

    FactoryGirl.define do
      factory :project do
        name "The Xing Framework"
        description "A neat new integrated web development platform."
        deadline Date.today + 30.days
        goal 20000.00
      end
    end

With that file written, we should be able to run our spec and get a clean failure complaining about the route:

    $ rspec spec/requests/project_get_spec.rb 
    Failures:

    1) GET /project returned JSON is correctly formatted
     Failure/Error: json_get resource_url
     ActionController::RoutingError:
       No route matches [GET] "/project/1"
       
Exactly what we expect.  The app can't serve our request because Rails doesn't know how to route it.

## Route and Controller

Add this line to backend/config/routes.rb:

    ```resources :projects, :only => :show```

And create this controller file:

##### backend/app/controllers/projects.rb

    class ProjectsController < ApplicationController

      # GET /projects/{id}
      def show
        project = Project.find(params[:id])
        render :json => ProjectSerializer.new(project)
      end

    end

NOTE to Rails users: That's typically all there is to an action in a Xing controller.  If you're using user-based authorization, you might have one more line to ensure the current user is permitted to see this resource.  But that's all. In Xing, the controller exists only to find the database row(s) necessary and pass them to a Serializer for output. (Or on write actions, a Mapper is used instead of a serializer) No logic goes there.  As a result, we don't usually even bother to test controllers in a Xing project ... there's no point!  

With these two written, our request spec will get further before failing:



## Serializer

TODO

## Controller

TODO