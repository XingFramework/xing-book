# The Xing Work Flow - Frontend

This is a general guide to creating a feature in Xing in the frontend.

In this example, we will be creating feature that allows a user to read or write Dogs to correspond with the API resource for Dogs we built in "The Xing Work Flow - Backend".

Some things to consider:
* The frontend is its own MVC situation. We will have to set up a frontend resource to consume the backend API.
* We have a divergence of code bases for Xing between MindSwarms and Yoric. So we will be describing the old style (MSW) and new style (Yoric) for Xing.

## Setting up the Frontend

1. [Feature Spec](#feature_spec)
2. [Create a Directory](#directory)
3. Module
5. Controller
6. State
7. Resource

### <a name="feature_spec"></a>Feature Spec
###### backend/spec/features/user_can_see_dog_info.rb

We all do TDD, right? This is obviously where you should be starting.

Just as the request spec is the integration test for the Rails backend API, the feature spec is the integration test for the frontend. Yes, it lives in the backend with the backend specs. Get over it.

```ruby
require 'spec_helper'

# We are using `steps` rather than `describe` for feature specs.
steps "User sees Dog show page", :js => true, :size => :desktop, :type => :feature do

  before :all do
    # Remember to use an instance variable, not a let block for a feature spec
    @user = FactoryGirl.create(:user)
    @dog = FactoryGirl.create(:dog)
  end
  
  # From spec/support/session_helpers.rb. It signs in @user.
  perform_steps "sign in with"
  
  it "has the dog's information" do 
    expect(page).to have_content(@dog.name)
    expect(page).to have_content(@dog.breed)
    expect(page).to have_content(@dog.age)
    expect(page).to have_content(@dog.favorite_toy)
  end
end
```

### <a name="directory"></a>Create a Directory
###### frontend/src/app/

Oh my god, where IS everything? 

If you haven't noticed yet, things are organized much differently in the frontend than you might be used to in Rails. 
