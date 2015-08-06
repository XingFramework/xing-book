# The Xing Work Flow - Frontend

This is a general guide to creating a feature in Xing in the frontend.

In this example, we will be creating feature that allows a user to read or write Dogs to correspond with the API resource for Dogs we built in "The Xing Work Flow - Backend".

Some things to consider:
* The frontend is its own MVC situation. We will have to set up a frontend resource to consume the backend API.
* We have a divergence of code bases for Xing between different projects. For documentation purposes, we will be describing the most recent syntax and structure.

## Setting up the Frontend

1. [Feature Spec](#feature_spec)
2. [Create a Directory](#directory)
3. [Create a Module](#module)
5. [State](#state)
6. [Controller](#controller)
7. [Template](#template)
8. [Resource](#resource)

### <a name="feature_spec"></a>Feature Spec

We all do TDD, right? This is obviously where you should be starting.

Just as the request spec is the integration test for the Rails backend API, the feature spec is the integration test for the frontend. Yes, it lives in the backend with the backend specs. Get over it.

###### backend/spec/features/user_can_see_dog_info.rb
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

Oh my god, where IS everything? 

Things are organized differently in the Xing frontend directory than in Rails. Here, files are grouped by feature or component. In our example, all the Dog files will be in the same directory.

*Remember that in the frontend, the file and variable naming convention is lowerCamelCase rather than snake_case.*

###### frontend/src/app

```javascript
frontend/src/app/dogs
```

Let's also make the test directory.

```javascript
frontent/test/dogs
```

###<a name="module"></a>Create a Module

We'll need to create a Dog module. It will include all the files needed for the Dog component of our application. For the moment, we will be setting up the empty module and exporting it.

###### frontend/src/app/dogs/dogs.js
```javascript
import {Module} from "a1atscript";

var dogs = new Module('dog', []);

export default dogs;
```

###<a name="state"></a>State

States are mildly analogous to routes in the backend. It's in the States that we make resources available to 

Firstly, we're going to need tests!

###### frontend/test/dogs/dogsStates.js
```javascript
import {DogState} from '../../src/app/dogs/dogsStates.js';

```

###### frontend/src/app/dogs/dogsStates.js
```javascript
import {State, Resolve} from 'stateInjector';

@State('root.inner.dogs')
export class DogsState {
  constructor() {
    super();
    this.url = "^/dogs";
    this.template = "<ui-view lrd-state-attrs></ui-view>";
    this.abstract = true;
  }
}

@State('root.inner.dogs.show')
export class DogsShowState {
  constructor() {
    super();
    this.url = "/:id"
    this.controller = "DogShowCtrl";
    this.controllerAs = "dogShow";
    this.templateUrl = "dogs/dogs-show.tpl.html";
  }
  
  @Resolve('resources', '$stateParams')
  dog(resources, $stateParams) {
    return resources.dogs({id: $stateParams.id}).load();
  }
}
```





