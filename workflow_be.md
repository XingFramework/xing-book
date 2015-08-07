# The Xing Work Flow

This is a general guide to creating a feature in Xing from the Backend to the Frontend (coming soon). 

In this example, we will be creating feature that allows a user to read or write Dogs.

## Setting up the Backend resource

There are basically two types of resource:
1. [OUTGOING:](#outgoing) Resources that are being consumed by the Frontend
2. [INCOMING:](#incoming) Resources that are being received from the Frontend

---
### <a name="outgoing"></a>OUTGOING

Outlined here are the files that you would need to touch to produce an outgoing Backend Resource. We generally tend to write files in this order.

* [API Doc](#oapidoc)
* [Request Spec](#orequestspec)
* [Routing Spec](#oroutingspec)
* [Routes](#oroutes)
* [Controller Spec](#ocontrollerspec)
* [Controller](#ocontroller)
* [Model Spec](#omodelspec)
* [Model](#omodel)
* [Factory](#ofactory)
* [Serializer Spec](#oserializerspec)
* [Serializer](#oserializer)

#### <a name="oapidoc"></a>API Doc
###### API_DOC/dog

Let's first consider the resource that you are trying to build for the Frontend to consume. For a show resource, it would look something like this:

```ruby
{
  links: {
    self: "/dogs/1"
  },
  data: {
    name: "Buddy",
    age: 3,
    breed: "Some kind of large rat"
  }
}
```

#### <a name="orequestspec"></a> Request Spec 
###### backend/spec/requests/dog_show_spec.rb

Now that we know what we'd like to receive when we're trying to get information on a specific dog, we should write a request spec. The request spec tests that when we hit "/dogs/:id" we will get a response that matches the show resource we imagined above.


```ruby
require 'spec_helper'

describe "dogs#show", :type => :request do
  let :dog do
    FactoryGirl.create(:dog)
  end

  describe "GET dogs/:id" do
    it "shows page as json" do
      json_get "dogs/#{dog.id}"

      expect(response).to be_success
      expect(response.body).to have_json_path('links/self')
      expect(response.body).to have_json_path('data/name')
      expect(response.body).to have_json_path('data/age')
      expect(response.body).to have_json_path('data/breed')
    end
  end
end
```

#### <a name="oroutingspec"></a>Routing Spec
###### backend/spec/routing/dogs_routing_spec.rb

```ruby
require 'spec_helper'

describe DogsController do
  describe "routing" do
    it "recognizes and generates #show" do
      expect({ :get => "/dogs/1" }).to route_to(:controller => "dogs", :action => "show", :id => "1")
    end
end

```

#### <a name="oroutes"></a>Routes
###### backend/config/routes.rb

Add the following line to your routes file:
```ruby 
resources :dogs, :only => [:show]
```

#### <a name="ocontrollerspec"></a>Controller Spec
###### backend/spec/controllers/dogs_controller.rb

```ruby
require 'spec_helper'

describe DogsController do

  let :dog do
    double Dog, :id => "123"
  end

  let :serializer do
    double(DogSerializer)
  end

  ########################################################################################
  #                                      GET SHOW
  ########################################################################################
  describe "responding to GET show" do

    it "should find the dog and pass it to a serializer" do
      allow(Dog).to receive(:find).with(dog.id).and_return(dog)
      allow(DogSerializer).to receive(:new).with(dog).and_return(serializer)
      expect(controller).to receive(:render).
        with(:json => serializer).
        and_call_original
      get :show, :id => 123
    end
  end
end

```

#### <a name="ocontroller"></a>Controller
Note that our controller descends from JsonController. 

For outgoing resources, the controller will render JSON that is output by its serializer (DogSerializer). We'll be looking at those shortly.
```ruby
class DogsController < JsonController

  # GET /dogs/:id
  def show
    dog = Dog.find(params[:id])
    render :json => DogSerializer.new(dog)
  end
```

You will find that all the controllers follow almost exactly the same pattern. If you find yourself straying from this pattern or adding methods here, you should consult your team leader.

#### <a name="omodelspec, omodel,ofactory "></a>Model Specs/Model/Factory

You'll need to create these as well. This part is pretty much the same as you've done in Rails.

#### <a name="oserializerspec"></a>Serializer Spec
###### backend/spec/serializers/dog_serializer_spec.rb

Serializers are where we construct our outgoing resources. It will take AR models and build JSON according to the instructions we write!

```ruby
require 'spec_helper'

describe DogSerializer, :type => :serializer do
  let :dog do
    FactoryGirl.create(:dog, :age => 1
    )
  end

  describe 'as_json' do
    let :json do
      DogSerializer.new(dog).to_json
    end

    it "should have the correct structure" do
      expect(json).to have_json_path('links/self')
      expect(json).to have_json_path('data/name')
      expect(json).to have_json_path('data/age')
      expect(json).to have_json_path('data/breed')
    end
  end
end
```

#### <a name="oserializer"></a>Serializer
###### backend/serializers/dog_serializer.rb

Note that the serializer descends from BaseSerializer. 
```ruby
class DogSerializer < BaseSerializer
  attributes :name, :age, :breed

  def links
    { :self => routes.dog_path(object) }
  end
end
```

High Five! We should now be able to hit the /dogs/:id resource and GET correctly formatted JSON.

---
### <a name="incoming"></a>INCOMING

Outlined here are the files that you would need to touch to **receive** JSON and write that data to the DB. We generally tend to write files in this order.

* [API Doc](#iapidoc)
* [Request Spec](#irequestspec)
* [Routing Spec](#iroutingspec)
* [Routes](#iroutes)
* [Controller Spec](#icontrollerspec)
* [Controller](#icontroller)
* [Model Spec](#imodelspec)
* [Model](#imodel)
* [Factory](#ifactory)
* [Serializer Spec](#iserializerspec)
* [Serializer](#iserializer)
* [Mapper Spec](#imapperspec)
* [Mapper](#imapper)

#### <a name="iapidoc"></a>API Doc
###### API_DOC/dog

Create/Update resources will have a similar structure to the show:

```ruby
# GET /dogs/:id
{
  links: {
    self: "/dogs/1"
  },
  data: {
    name: "Buddy",
    age: 3,
    breed: "Some kind of large rat"
  }
}

# POST /dogs
{
  links: {
  },
  data: {
    name: "Pooh Bear",
    age: 3,
    breed: "Snorting Spinner"
  }
}

# PUT /dogs/:id
{
  links: {
    self: "/dogs/1"
  },
  data: {
    name: "Buddy McLovin",
    age: 3,
    breed: "Some kind of large rat"
  }
}
```
Note that the POST resource does not require a self link, but a PUT does require a self link to know which record to update.

For the rest of the example, we will be working with a PUT resource.

#### <a name="irequestspec"></a> Request Spec 
###### backend/spec/requests/dog_update_spec.rb

Now that we know what we are receiving from the Frontend, we should write a request spec. The request spec tests that when we hit "/dogs/:id" with a JSON body, we will either successfully update a record or process a failed update correctly.


```ruby
require 'spec_helper'

describe "dogs#update", :type => :request do
  let! :dog do
    FactoryGirl.create(:dog
    )
  end

  let :json_body do
    {
      links: {
        self: "/dogs/1"
      },
      data: {
        name: "Buddy McLovin"
      }
    }.to_json
  end

  describe "Successful update"do
    describe "PUT dogs/:id" do
      it "is a 200 success including the serialized object" do

        json_put "dogs/#{dog.id}", json_body

        expect(response).to be_success
        expect(response.body).to have_json_path('links/self')
        expect(response.body).to have_json_path('data/name')
        expect(response.body).to have_json_path('data/age')
        expect(response.body).to have_json_path('data/breed')

        expect(response.body).to be_json_eql("\"#{routes.dog_path(dog)}\"").at_path('links/self')
        expect(response.body).to be_json_eql("\"Buddy McLovin\"").at_path("data/name")

      end
    end
  end

  describe "failing update" do
    describe 'required information omitted' do
      let :invalid_json do
        {
          links: {
            self: "/dogs/1"
          },
          data: {
            name: nil
          }
        }.to_json
      end

      describe "PUT organizations/:id" do
        it "is a 422 with an error in response body" do
          authenticated_json_put admin, "organizations/#{organization.id}", invalid_json

          expect(response.status).to be(422)
          expect(response.body).to be_json_eql("\"can't be blank\"").at_path("data/name/message")
        end
      end
    end
  end
end
```

#### <a name="iroutingspec"></a>Routing Spec
###### backend/spec/routing/dogs_routing_spec.rb

Add the update route to the Routing Spec.

```ruby
require 'spec_helper'

describe DogsController do
  describe "routing" do
    it "recognizes and generates #show" do
      expect({ :get => "/dogs/1" }).to route_to(:controller => "dogs", :action => "show", :id => "1")
    end
    
    it "recognizes and generates #update" do
      expect({ :put => "/dogs/1" }).to route_to(:controller => "dogs", :action => "update", :id => "1")
    end
end

```

#### <a name="iroutes"></a>Routes
###### backend/config/routes.rb

Add update to the dogs route:
```ruby
resources :dogs, :only => [:show, :update]
```

#### <a name="icontrollerspec"></a>Controller Spec
###### backend/spec/controllers/dogs_controller.rb

Test the update method in your controller spec. Notice that we use plenty of mocks, and that we now require a Mapper!

```ruby
require 'spec_helper'

describe DogsController do

  let :dog do
    double Dog, :id => "123"
  end

  let :serializer do
    double(DogSerializer)
  end

  let :json do
    { stuff: "like this", more: "like that" }.to_json
  end
  
  let :mock_mapper do 
    double(DogMapper)
  end

  let :mock_errors do
    { data: { stuff: "Is required" }}
  end


  ########################################################################################
  #                                      GET SHOW
  ########################################################################################
  describe "responding to GET show" do

    it "should find the dog and pass it to a serializer" do
      expect(Dog).to receive(:find).with(dog.id).and_return(dog)
      expect(DogSerializer).to receive(:new).with(dog).and_return(serializer)
      expect(controller).to receive(:render).
        with(:json => serializer).
        and_call_original
      get :show, :id => 123
    end
  end
end

  ########################################################################################
  #                                      PUT UPDATE
  ########################################################################################
  describe "responding to PUT update" do

    it "should update with dog mapper and pass the JSON to it" do
      expect(DogMapper).to receive(:new).with(json, dog.id).and_return(mock_mapper)
      expect(mock_mapper).to receive(:save).and_return(true)
      expect(mock_mapper).to receive(:dog).and_return(dog)
      expect(DogSerializer).to receive(:new).with(dog).and_return(serializer)

      controller.should_receive(:render).
        with(:json => serializer).
        and_call_original

      put :update, json, { :id => 123 }

    end

    it "should render status 422 if not updated" do
      expect(DogMapper).to receive(:new).with(json, dog.id).and_return(mock_mapper)
      expect(mock_mapper).to receive(:dog).and_return(dog)
      expect(mock_mapper).to receive(:save).and_return(false)
      expect(mock_mapper).to receive(:errors).and_return(mock_errors)
      expect(controller).to receive(:failed_to_process).with(mock_errors).and_call_original

      post :update, json, { :id => 123 }

      expect(response).to reject_as_unprocessable
```

#### <a name="icontroller"></a>Controller
An update controller is probably about as complicated of a controller as you will write. 

It uses Mapper to process the JSON and params to update the record. If it is a successful update, it then uses a Serializer to respond with the updated record in JSON format.
```ruby
class DogsController < JsonController

  # GET /dogs/:id
  def show
    dog = Dog.find(params[:id])
    render :json => DogSerializer.new(dog)
  end
  
  #PUT /dogs/:id
  def update
    mapper = DogMapper.new(json_body, params[:id])
    
    if mapper.save
      render :json => DogSerializer.new(mapper.dog)
    else
      failed_to_process(mapper.errors)
    end
  end
  
end
```

Again, you will find that all the controllers follow almost exactly the same pattern. If you find yourself straying from this pattern or adding methods here, you should consult your team leader.

#### <a name="imodelspec, imodel, ifactory, iserializer, iserializerspec "></a>Model Spec/Model/Factory/Serializer Spec/Serializer

If you had already created a show resource, You have all these things already! If not, see the code above regarding [Serializers](#oserializers) and [Serializer Specs](#oserializerspec).

#### <a name="imapperspec"></a>Mapper Spec
###### /backend/spec/mappers/dog_mapper_spec.rb

Here we test that our Mapper does what it is meant to do, which is to: 
* take JSON
* break it down
* map it to the corresponding Active Record Model(s)
* check for errors
* save the record(s)

or
* send meaningful error messages to the Frontend

OK! 

```ruby
require 'spec_helper'

describe DogMapper, :type => :mapper do

  let! :dog do
    FactoryGirl.create(:dog, :name => "Fluffy FooFoo")
  end

  let :mapper do
    DogMapper.new(json, dog.id)
  end

  let :valid_data do
    {
      data: {
        name: "Buddy McLovin"
      }
    }
  end

  let :invalid_data do
    { 
      data: {
        name: nil
      }
    }
  end

  describe 'updating content' do

    describe "valid data" do

      let :json do
        valid_data
      end

      it "should save the dog" do
        expect do
          mapper.save
        end.to change{ dog.reload.name }.to("Buddy McLovin")
      end

      it "should be able to return dog" do
        mapper.save
        expect(mapper.dog).to be_a(Dog)
        expect(mapper.dog).to be_persisted
      end
    end

    describe "invalid data" do
      let :json do
        invalid_data.to_json
      end

      it "should insert an error into the errors hash without saving anything" do
        expect do
          mapper.save
        end.not_to change{ dog.reload.name }
        expect(mapper.errors).to eq(
          {:data=>{:name=>{:type=>"required", :message=>"can't be blank"}}}
        )
      end
    end
  end

end
```

#### <a name="imapper"></a>Mapper
###### /backend/mappers/dog_mapper.rb

Note that the Mapper descends from HypermediaJSONMapper. HJM has quite a few methods in it and it would be beneficial to give it some study to figure out how it works. 

The following example is a deceptively simple case. The Mapper is often the most difficult part of a Backend Resource, especially if the resource has nested models and complicated validations. Don't be afraid to ask for help here!
```ruby
class DogMapper < HypermediaJSONMapper
  alias dog record
  alias dog= record=

  def record_class
    Dog
  end

  def assign_values(data_hash)
    @dog_data = data_hash

    super
  end

  def update_record
    dog.assign_attributes(@dog_data)
  end

end
```

(∩`-´)⊃━☆ﾟ.*･｡ﾟ DONE!