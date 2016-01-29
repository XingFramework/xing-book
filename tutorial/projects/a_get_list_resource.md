# A GET list resource

In addition to an individual project, the frontend needs to be able to know a list of all the projects available, so we need to create a list resource.  Here we'll be building the backend API support for the [GET /projects resource described at the beginning of this chapter.](your_first_resource_projects.md)

## Tests

The test for our list resource looks much like the one for our singular resource, though of course we will factory multiple Projects and confirm that all of them appear in the correct location in the returned JSON.

##### ```backend/spec/requests/projects_get_spec.rb```

    TODO once tested

Note that we're explicitly testing the fact that project descriptions, deadlines, and goals don't appear in the JSON of the list resource.  For now, our list resource will give only minimal information about each project. 

As before, running this spec will give us a failure because the route doesn't exist.

    backend$ rspec spec/requests/projects_get_spec.rb 

    1) GET /project returned JSON has the correct contents and format
     Failure/Error: json_get resource_url
     ActionController::RoutingError:
       No route matches [GET] "/projects"

## Route and Controller

We already have a Project model, so we won't need to do any more work in the model layer.  But we do need to route the /projects endpoint and write a controller action.  

Fortunately these are both just as simple as they were in the singular project resource.

Change one line in routes.rb:

##### backend/config/routes.rb

    resources :projects, :only => [:show, :index]

And add this method to the controller, to handle the list resource.

##### backend/app/controllers/projects_controller.rb

    # GET /projects
    def index
      projects =  projects = Project.all.order('created_at ASC')
      render :json => ProjectListSerializer.new(projects)
    end

**IMPORTANT NOTE on coding conventions:** Rails conventions encourage us to think of 'index' and 'show' as two aspects of a single resource. For example, Rails routing helpers generate both with a single 'resources' call, and we put both actions in the same controller. Because those conventions are well established, we'll tend to structure our Rails code that way.  However, in Xing we tend to look at things from an HTTP perspective: ```/projects/1``` and ```/projects``` are two different resources. They have different URLs, different data formats, different contents - one is the data about a project, where the other is a list of available projects. Each of these two resources can (in principle) respect multiple HTTP verbs like GET, PUT, DELETE and so forth. So in most of Xing we will treat the two as different resources: they'll have different descriptions in ```API_DOC/```, each will get its own entry and file in the frontend, etc. If Xing were starting over with a "replacement" for Rails, we would probably have separate ```project_controller.rb``` and ```projects_list_controller.rb```, each one handling the multiple HTTP verbs for its particular resource.  However, because Rails' existing conventions and helpers provide so much convenience, we will stick with those conventions in ```backend/``` to a large extent.

Astute readers will notice that we specified an order for the projects list, even though our API_DOC entry says that no particular order is guaranteed. In this case, since our API makes no claims and promises to the frontend as to the order of the projects, it's okay for the backend to pick whatever order suits it. If we specified no order, the projects would be retrieved in whatever order the database provided them, which would make them hard to test. So we've specified that they will be returned in the order they were created, which makes the structure of the JSON predictable and easy to test. 

Now when we run our integration spec, as before we'll fail because we haven't written the serializer yet.

    $ rspec spec/requests/projects_get_spec.rb 
    
      1) GET /project returned JSON has the correct contents and format
         Failure/Error: json_get resource_url
         NameError:
           uninitialized constant ProjectsController::ProjectListSerializer
         # ./app/controllers/projects_controller.rb:12:in `index'

## Serializer

### Serializer Spec

As usual, we'll start with a spec.  What output do we want from this list resource?

##### ```backend/spec/serializers/project_list_serializer_spec.r```

    require 'spec_helper'
    
    describe ProjectSerializer, :type => :serializer do
    
      let! :project_1 do
        FactoryGirl.create(:project,
                           :name => "The Xing Framework",
                           :description => "Cool new web framework!"
                          )
      end
    
      let! :project_2 do
        FactoryGirl.create(:project,
                           :name => "The Xing Book",
                           :description => "A book!"
                          )
      end
    
      let! :project_3 do
        FactoryGirl.create(:project,
                           :name => "The Xing Screencasts",
                           :description => "Screencasts!"
                          )
      end
    
      let :serializer do
        ProjectListSerializer.new([project_1, project_2, project_3])
      end
    
      describe "as_json" do
        let :json do serializer.to_json end
    
        it "should have the correct links" do
          expect(json).to be_json_string("/projects").at_path("links/self")
        end
    
        it "should have the correct structure and content" do
          expect(json).to be_json_string("/projects/#{project_1.id}").at_path("data/0/links/self")
          expect(json).to be_json_string("The Xing Framework")       .at_path("data/0/data/name")
          expect(json).not_to have_json_path("data/0/data/description")
          expect(json).not_to have_json_path("data/0/data/deadline")
          expect(json).not_to have_json_path("data/0/data/goal")
    
          expect(json).to be_json_string("/projects/#{project_2.id}").at_path("data/1/links/self")
          expect(json).to be_json_string("The Xing Book")            .at_path("data/1/data/name")
          expect(json).not_to have_json_path("data/1/data/description")
          expect(json).not_to have_json_path("data/1/data/deadline")
          expect(json).not_to have_json_path("data/1/data/goal")
    
          expect(json).to be_json_string("/projects/#{project_3.id}").at_path("data/2/links/self")
          expect(json).to be_json_string("The Xing Screencasts")     .at_path("data/2/data/name")
          expect(json).not_to have_json_path("data/2/data/description")
          expect(json).not_to have_json_path("data/2/data/deadline")
          expect(json).not_to have_json_path("data/2/data/goal")
    
          # the project descriptions do not appear anywhere in this resource
          expect(json).not_to be_json_string("Cool new web framework!")
          expect(json).not_to be_json_string("A Book!")
          expect(json).not_to be_json_string("Screencasts")
        end
      end
    end

We're being very thorough about this test: confirming not only that the correct data exists, but that the data we don't want (other parts of the project resource) also aren't present. For those, we are checking both that the path where they might exist isn't present (e.g. "data/1/data/description") and for some also that the text doesn't exist anywhere in the JSON (e.g. ```expect(json).not_to be_json_string("A Book!")```, specified without ```.at_path()```). This is probably overkill in practice, it's included here to demonstrate available practices.

### The Serializer Itself

In Xing, a resource list serializer is slightly more complex than an individual resource serializer. It creates an array, and then delegates the individual items in that array to a second serializer. Sometimes you'll need to write that second serializer, sometimes you can use one you've already written. 

Let's start with a simple version of the list serializer.

##### ```backend/app/serializers/project_list_serializer.rb```

    class ProjectListSerializer < Xing::Serializers::List
    
      def links
        {
          self:     routes.projects_path,
          template: routes.project_path_rfc6570
        }
      end
    
      def item_serializer_class
        ProjectSerializer
      end
    
    end

There are three new things worth noting here.  First, we subclass ```Xing::Serializers::List``` instead of ```Xing::Serializers::Base```.  This version of the serializer class expects to be initialized with an Array (or other Enumerable) of items, and to serialize each of those with a secondary delegated serializer.  To specify which class is used to serialize the individual items in the array, define the method * ```#item_serializer_class``` and have it return said class. Here we simply delegate to the ```ProjectSerializer``` we already created. 

The third item is this mysterious new link called template, with the value ```routes.project_path_rfc6570```. Xing frontend applications need to know the generalized format of URLs for items in a list. (We'll discuss why when we get to coding the frontend.) We'll specify that in the RFC 6570 format mentioned earlier. In this case, that will look like ```/projects/{id}```.  That string has the same meaning as the Rails routing template ```projects/:id```.  The  [rails-rfc6570](https://rubygems.org/gems/rails-rfc6570) rubygem can produce these templates for us by extracting routes from the Rails routing table and formatting them in  RFC 6570 style.

With the serializer above, our spec will run but still fail:

    $ rspec spec/serializers/project_list_serializer_spec.rb 
    .F
    
    Failures:
    
      1) ProjectSerializer as_json should have the correct structure and content
         Failure/Error: expect(json).not_to have_json_path("data/0/data/description")
           Expected no JSON path "data/0/data/description"
         # ./spec/serializers/project_list_serializer_spec.rb:40:in `block (3 levels) in <top (required)>'

The failure is because our delegated item serializer in ```ProjectSerializer``` is emitting the entire structure of the project, but we specified that only the name and self link should be included. 

### Filtering The Item Serializer

Fortunately there's an easy solution: any subclass of ```ActiveModel::Serializers``` (including all Xing serializers) can accept an options Hash that filters which attributes are emitted.  You can specify either ```{:only => [:list, :of: attributes]}``` or ```{:except => [:list, :of, :attributes]}``` when you instantiate the serializer.

In a Xing list serializer, if you define a method ```item_serializer_options```, it will get passed to the serializer for each individual item in the list.  So, limiting the fields for the items in /projects is simple.  Add these three lines to your list serializer:


##### ```backend/app/serializers/project_list_serializer.rb```  

    def item_serializer_options
      { only: :name }
    end
    
With that method added, our specs will pass.

    backend$ rspec spec/requests/ spec/serializers/
    .......
    
    Finished in 0.38943 seconds (files took 1.6 seconds to load)
    7 examples, 0 failures


