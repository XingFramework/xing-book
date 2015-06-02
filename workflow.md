# The Xing Work Flow

This is a general guide to creating a feature in Xing from the Backend to the Frontend. 

In this example, we will be creating feature that allows a user to read or write Dogs. YAY, PUPS!

## Setting up the Backend resource

#### Outgoing
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

#### Incoming
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

Now that we know what we'd like to receive when we're trying to get information on a particular dog, we should write a request spec where we can test that when we hit "/dogs/:id" we will get a response that matches the show resource we imagined above.
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

