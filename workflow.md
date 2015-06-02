# The Xing Work Flow

This is a general guide to creating a feature in Xing from the Backend to the Frontend. 

In this example, we will be creating feature that allows a user to read or write Dogs.

## Setting up the Backend resource

#### Outgoing
* API Doc
* Request Spec
* Routing Spec
* Routes
* Controller Spec
* Controller
* Model Spec
* Model
* Factory
* Serializer Spec
* Serializer


#### Incoming
* API Doc
* Request Spec
* Model Spec
* Model
* Factory
* Routing Spec
* routes.rb
* Controller Spec
* Controller
* Serializer Spec
* Serializer
* Mapper Spec
* Mapper

#### API Doc
###### API_DOC/dog

Let's first consider the resource that you are trying to build for the Frontend to consume. For a show resource, it would look something like this:

```
{
  links: {
    self: "/dogs/1"
  },
  data: {
    name: "Buddy",
    breed: "Some kind of large rat"
  }
}
```

#### Request Spec 
###### backend/spec/requests/dog_show_spec.rb

Now that we know what we'd like to receive when we're trying to get information on a specific dog, we should write a request spec. The request spec tests that when we hit "/dogs/:id" we will get a response that matches the show resource we imagined above.


```
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
      expect(response.body).to have_json_path('data/breed')
    end
  end
end
```

#### Routing Spec
###### backend/spec/routing/dogs_routing_spec.rb

```
require 'spec_helper'

describe DogsController do
  describe "routing" do
    it "recognizes and generates #show" do
      expect({ :get => "/dogs/1" }).to route_to(:controller => "dogs", :action => "show", :id => "1")
    end
end

```

#### Routes
###### backend/config/routes.rb

Add the following line to your routes file:
```
resources :dogs, :only => [:show]
```

#### Controller Specs
###### backend/spec/controllers/dogs_controller.rb

```
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

  let :mock_errors do
    { data: { stuff: "Is required" }}
  end


  ########################################################################################
  #                                      GET SHOW
  ########################################################################################
  describe "responding to GET show" do

    it "should find the dog and pass it to a serializer" do
      Dog.should_receive(:find).with(dog.id).and_return(dog)
      DogSerializer.should_receive(:new).with(dog).and_return(serializer)
      controller.should_receive(:render).
        with(:json => serializer).
        and_call_original
      get :show, :id => 123
    end
  end
end

```

#### Controller
Note that our controller descends from JsonController. 

For outgoing resources, the controller will render JSON that is output by its serializer (DogSerializer). We'll be looking at those shortly.
```
class DogsController < JsonController

  # GET /dogs/:id
  def show
    dog = Dog.find(params[:id])
    render :json => DogSerializer.new(dog)
  end
```

You will find that all the controllers follow almost exactly the same pattern. If you find yourself straying from this pattern or adding methods here, you should consult your team leader.

#### Model Specs/Model/Factory

