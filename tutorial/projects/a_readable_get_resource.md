# A readable GET resource

Lets start with just the individual ```/projects/{id}``` resource.

## Test first! (write a request spec)

We're going to start by writing a test.  (You do write your tests first ... right?) We'll start with an integration-level test that verifies that the entire backend produces JSON as desired when given the proper HTTP request, and use that as a framework to guide our unit tests and code.

We'll use [FactoryGirl](https://github.com/thoughtbot/factory_girl) to manufacture a Project, then submit an HTTP request to to the proper URL and inspect the returned JSON.

***Xing uses RSpec as its primary testing framework for the backend. If you're unfamiliar with RSpec, you can read about it [here](http://rspec.info/).***

##### ```backend/spec/requests/project_get_spec.rb```

    require 'spec_helper'

    describe 'GET /projects/:id', :type => :request do
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
    
That'll generate a model, spec, and empty factory file.  There will be no behavior in the Project model (at least not yet), so there's nothing to test in the model spec file at ```backend/spec/models/project_spec.rb```. We can leave those files alone for the time being.  

This would be a good time to migrate the database (and the test database) to populate our new model:

    backend$ bundle exec rake db:migrate
    backend$ bundle exec rake db:test:prepare

TODO: manually preparing the test database should not be necessary in a contemporary Rails project, and a pull request has been submitted to xing-application-base to remove the necessity.  The second command above should be removed from the tutorial once that PR has been merged and a new version of xing-framework released.

Let's fill out that factory, then, so our spec can run.  Open that file and replace the auto-generated contents with:

##### backend/spec/factories/projects.rb

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
       No route matches [GET] "/projects/1"
       
Exactly what we expect.  The app can't serve our request because Rails doesn't know how to route it.

## Route and Controller

Add this line to backend/config/routes.rb:

    ```resources :projects, :only => :show```

And create this controller file:

##### backend/app/controllers/projects_controller.rb

    class ProjectsController < ApplicationController

      # GET /projects/{id}
      def show
        project = Project.find(params[:id])
        render :json => ProjectSerializer.new(project)
      end

    end

NOTE to Rails users: That's typically all there is to an action in a Xing controller.  If you're using user-based authorization, you might have one more line to ensure the current user is permitted to see this resource.  But that's all. In Xing, the controller exists only to find the database row(s) necessary and pass them to a Serializer for output. (Or on write actions, a Mapper is used instead of a serializer) No logic goes there.  As a result, we don't usually even bother to test controllers in a Xing project ... there's no point!  

With these two written, our request spec will get further before failing because we have no Serializer:

    $ rspec spec/requests/project_get_spec.rb 

    Failures:

    1) GET /projects/:id returned JSON is correctly formatted
     Failure/Error: json_get resource_url
     NameError:
       uninitialized constant ProjectsController::ProjectSerializer

## Serializer

Serializers are the "views" of the Rails half of a Xing project. When producing a readable Xing resource, almost all of the work is done by the Serializer. If there's any interesting logic to be performed, say to transform the database records into a presentable format, or to perform any computations to add the results to the JSON resource, it would happen here.

One of the nice aspects of putting the logic in the Serializer is that Serializers are lightweight and fast Ruby objects that don't depend on much of the Rails stack. Unlike Controllers and ActiveRecord models, tests on Serializers run very quickly so when we test Serializers in isolation we can afford to test them in great detail.

Let's start with a spec for the serializer. It will look a lot like the request spec, but will be testing just one class in isolation. In the serializer test, we will test the full structure of the JSON resource we wish to produce.

##### backend/spec/serializers/project_serializer_spec.rb

    require 'spec_helper'
    
    describe ProjectSerializer, :type => :serializer do
    
      let! :project do
        FactoryGirl.create(:project,
                           :name => "The Xing Framework",
                           :description => "Cool new web framework!",
                           :deadline => Date.today + 30.days,
                           :goal => 15000.00
                          )
      end
    
      let :serializer do
        ProjectSerializer.new(project)
      end
    
      describe "as_json" do
        let :json do serializer.to_json end
    
        it "should have the correct links" do
          expect(json).to be_json_string("/projects/#{project.id}").at_path("links/self")
        end
    
        it "should have the correct structure and content" do
          expect(json).to be_json_string("The Xing Framework")     .at_path("data/name")
          expect(json).to be_json_string("Cool new web framework!").at_path("data/description")
          expect(json).to be_json_string(project.deadline.as_json) .at_path("data/deadline")
          expect(json).to be_json_string("15000.00")                  .at_path("data/goal")
        end
      end
    end

This should look a lot like the request spec, and there's a reason for that: in Xing, nearly all of the work of output happens in the Serializer. Most of the time, your models and controllers will be extremely lightweight. 

The key difference is that instead of submitting an HTTP request to the entire Rails stack, we are just passing a project to an instance of the Serializer and calling ```#to_json```.   

Of course ,that spec will fail because we haven't written ProjectSerializer. In this case, ProjectSerializer is trivially easy. Xing serializers inherit from [ActiveModel::Serializers](https://github.com/rails-api/active_model_serializers), so they gain all the default behavior of easily converting ActiveModel attributes to fields in the JSON Record.  Xing::Serializers::Base adds default behavior to wrap the data in a ```data: {}``` object and create a ```links: {}``` metadata object.  

For the most part, you just need to subclass ```Xing::Serializers::Base```, list the attributes you want serialized, and add a ```#links``` method to define this resource's links.

Future resources will get more interesting as we add behavior and nested resources.  But for now, all you need is this:

##### backend/app/serializers/project_serializer.rb

    class ProjectSerializer < Xing::Serializers::Base
      attributes :name, :description, :deadline, :goal

      def links
        {
          self: routes.project_path(object)
        }
      end
    end

The attributes method is simply the one inherited from ```ActiveModel::Serializers```, except that it wraps the contents in a data: {} object. In links, two interesting things are going on: object is always a reference to whatever was passed to the constructor of this serializer (see the first let: block in the spec).  routes is literally the Rails routing engine, which ```Xing::Serializers::Base``` makes available to you so you can use it to generate hypermedia links within your resource. 

When you run this spec, it probably still won't quite work. You may see an error like this one:

```
backend$ rspec spec/serializers/project_serializer_spec

Failures:

  1) ProjectSerializer as_json should have the correct structure and content
     Failure/Error: expect(json).to be_json_string("15000.00")               .at_path("data/goal")
       Expected equivalent JSON at path "data/goal"
       Diff:
       @@ -1,2 +1,2 @@
       -"15000.00"
       +"15000.0"
       
     # ./spec/serializers/project_serializer_spec.rb:29:in `block (3 levels) in <top (required)>'

```

What's going on here?  The problem is that we have a decimal number (stored in decimal datatype in our SQL database and represented as a BigDecimal in Ruby. Unfortunately JSON doesn't have a decimal data type, only Number, which is represented by a floating point and so [isn't suitable for representing monetary amounts like our funding goal](http://spin.atomicobject.com/2014/08/14/currency-rounding-errors/).  So the default serializer attempts to represent our decimal number as a string. However, it can't know exactly how many decimal points we care about for this particular number, and is defaulting to just one decimal point: 15000.0 isn't quite the same as the 15000.00 that we expected.

The solution is to give our serializer more information on how to serialize this particular attribute.  If you create a method with the same name as your attribute, your serializer will output the result of that method instead of the raw attribute. So let's change our serializer to say this:

```ruby
class ProjectSerializer < Xing::Serializers::Base
  attributes :name, :description, :deadline, :goal

  def goal
    sprintf("%.2f", object.goal)
  end

  def links
    {
      self: routes.project_path(object)
    }
  end
end
```

Now we've used the ```sprintf``` method to force Ruby to format our fundraising goal using two decimal points, and our both our serializer and request specs should pass:

```
backend$ rspec spec/serializers/ spec/requests/

Rails API server started on 127.0.0.1:54179
Setting up static app:
  serving files from ../frontend/bin
  using http://localhost:54179/ for API
  logging to /Users/evan/Development/Xing/apps/crowdfundr/backend/log/test_static.log
....

Finished in 0.33558 seconds (files took 1.8 seconds to load)
4 examples, 0 failures
```

Attentive readers have noticed that we didn't run the entire spec suite: just serializers and requests.  That's because end-to-end (E2E) tests also live in backend/ for technical reasons, and they can't execute successfully yet since we haven't built an AngularJS frontend.  If you try to run them, you'll get failures, but that's entirely expected at this point.
