# The Xing Work Flow - Frontend

This is a general guide to creating a feature in Xing in the frontend.

In this example, we will be creating a feature that allows a user to read or write Dogs to correspond with the API resource for Dogs we built in "The Xing Work Flow - Backend".

Some things to consider:
* The frontend is its own MVC situation. We will have to set up a frontend resource to consume the backend API.
* We have a divergence of code bases for Xing between different projects. For documentation purposes, we will be describing the most recent syntax and structure.

## Building a Frontend Feature

1. [Feature Spec](#feature_spec)
2. [Resource](#resource)
3. [Create a Directory](#directory)
4. [Create a Module](#module)
5. [App.js](#app)
6. [States](#states)
7. [Controllers](#controllers)
8. [Template](#template)
9. [Assets](#assets)
10. [Styling](#styling)


### <a name="feature_spec"></a>Feature Spec

We all do TDD, right? This is obviously where you should be starting.

Just as the request spec is the integration test for the Rails backend API, the feature spec is the integration test for the frontend. Yes, it lives in the backend with the backend specs. Get over it.

###### backend/spec/features/user_can_see_dog_info_spec.rb
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

### <a name="resource"></a>Create a Resource
We will be looking at how to create a resource using Relayer [https://github.com/LRDesign/relayer](https://github.com/LRDesign/relayer). This is the frontend resource object that talks to our backend API. 
###### frontend/common/resources/Dog.js
```javascript
import RL from "relayer";
import {Module, Config} from 'a1atscript';

class Dog extends RL.Resource {
}

RL.Describe(Dog, (desc) => {
  desc.property("name", "");
  desc.property("breed", "");
  desc.property("age", "");
  // remember the lowerCamelCase!
  desc.property("favoriteToy", "");
});

class Resources extends RL.Resource {

}

RL.Describe(Resources, (desc) => {
  var dogs = desc.hasList("dogs", Dog, []);
  dogs.linkTemplate = "dogs";
  dogs.canCreate = true;
})

@Config('relayerProvider')
function setupResources(relayerProvider) {
  relayerProvider.createApi("resources", Resources, "http://www.example.com/resources")
}

// setup an a1atscript module
var DogAppResourceLayer = new Module('DogAppRL', [RL, setupResources])

export default DogAppResourceLayer;
```

You then add this file to your [app.js](#app) file. Chances are, it's already been done for you, you lucky dog, you.

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
// a1atscript is a polyfill that Hannah wrote so that 
// we can use more ES6/Angular2.-like syntax. It's awesome.
import {Module} from "a1atscript";

var Dogs = new Module('dog', []);

export default Dogs;
```
Checkout a1atscript [https://github.com/hannahhoward/a1atscript](https://github.com/hannahhoward/a1atscript).

###<a name="app"></a>App.js
You will need to add your new module to App.js for it to register in the application.

###### frontend/src/app/app.js
```javascript
import {appName} from 'config';\
import DogAppRL from 'common/resources/Dog.js';
...
// add your import to the end of the list of existing imports
import Dogs from './dogs/dogs.js';

var app = new Module(appName, [
  ...
  DogAppRL,
  // add your module name here
  Dogs
])
```

###<a name="states"></a>States

States are mildly analogous to routes in the backend. According to ui-router  [https://github.com/angular-ui/ui-router/wiki](https://github.com/angular-ui/ui-router/wiki):
* A state corresponds to a "place" in the application in terms of the overall UI and navigation.
* A state describes (via the controller / template / view properties) what the UI looks like and does at that place.
* States often have things in common, and the primary way of factoring out these commonalities in this model is via the state hierarchy, i.e. parent/child states aka nested states.

It's in the States that we set up our user urls and make resources available to the component.

Firstly, we're going to need tests! You can learn more about testing in Jasmine [http://jasmine.github.io/2.0/introduction.html](http://jasmine.github.io/2.0/introduction.html).

###### frontend/test/dogs/dogsStates.js
```javascript
import {DogsState} from '../../src/app/dogs/dogsStates.js';

describe("DogsState", function() {
 var dogsState, mockResources, mockDog, mockStateParams, resolvedDog;

  beforeEach(function() {
    mockDog = "Buddy";
    mockResources = {
      dogs(params) {
        return {
          load() {
            if (params.id == 2) {
              return Promise.resolve(mockDog);
            } else {
              return Promise.reject("mistake");
            }
          }
        };
      }
    }
    mockStateParams = {
      id: 2
    }

    dogsState = new DogsState();
  });

  describe("state config", function() {
    it("should have a url of /dogs/:id", function() {
      expect(dogsState.url).toEqual("^/dogs/:id");
    });

    it("should have an empty template with a ui-view", function () {
      expect(dogsState.template).toEqual("<ui-view></ui-view>");
    });

    it("should be an abstract state", function() {
      expect(dogsState.abstract).toEqual(true);
    });

  });


  describe("dog", function() {
    beforeEach(function(done) {
      dogsState.dog(mockResources, mockStateParams).then((dog) => {
        resolvedDog = dog;
        done();
      })
    });

    it("should resolve dog from the resources and stateParams", function () {
      expect(resolvedDog).toEqual(mockDog);
    });
  });
  
  describe("failed dog", function() {
    beforeEach(function(done) {
      badStateParams = {
        id: undefined
      }
      dogsState.dog(mockResources, badStateParams).then((dog) => {
        resolvedDog = dog;
        done();
      })
    });

    it("should resolve dog from the resources and stateParams", function () {
      expect(resolvedDog).toEqual("mistake");
    });
  });

});

```
You are seriously going to need to read the ui-router wiki [https://github.com/angular-ui/ui-router/wiki](https://github.com/angular-ui/ui-router/wiki). It will explain the constructor properties and resolves used below. Our syntax is different (and better) than the syntax you will see in the ui-router wiki, but you will be able to draw the correct similitudes.
###### frontend/src/app/dogs/dogsStates.js
```javascript
import {State, Resolve} from 'stateInjector';

@State('root.inner.dogs')
export class DogsState {
  constructor() {
    this.url = "^/dogs";
    // lrd-state-attrs is a Xing directive that will help you with styling. 
    this.template = "<ui-view lrd-state-attrs></ui-view>"; 
    this.abstract = true;
  }
}

@State('root.inner.dogs.show')
export class DogsShowState {
  constructor() {
    // watch out for url collisions
    this.url = "/:id"
    this.controller = "DogCtrl";
    // this replaces $scope
    this.controllerAs = "dogShow";
    this.templateUrl = "dogs/dogs-show.tpl.html";
  }
  
  @Resolve('resources', '$stateParams')
  dog(resources, $stateParams) {
    return resources.dogs({id: $stateParams.id}).load();
  }
}
```

Add the new states to the module file! 

###### frontend/src/app/dogs/dogs.js
```javascript
import {Module} from "a1atscript";
import * as States from './dogsStates.js';

var Dogs = new Module('dog', [
  States
]);

export default Dogs;
```

Using `import * as States` brings in all the classes exported by `dogsStates.js`.


### <a name="controllers"></a>Controllers
Controllers contain business logic behind the application to decorate the controllerAs object with functions and values.

TDD!

###### frontend/test/dogs/dogsControllers.js
```javascript
// beware of case sensitivity in the file path!!!! 
// yes, you need to add .js to the file name
import {DogCtrl} from "../../src/app/dogs/dogsControllers.js"

describe("DogCtrl", function() {
  var dogCtrl, mockState, mockDog
  
  beforeEach(function() {
    mockState = jasmine.createSpyObj('mockState', ["go"]);
    mockDog = "mockBuddy";
    
    dogCtrl = new DogCtrl(mockState, mockDog);
  });
  
  it("should assign dog on the scope", function() {
    expect(dogCtrl.dog).toEqual("mockBuddy");
  });
  
  it("should go to dogs edit state", function() {
    dogCtrl.edit();
    expect(mockState).toHaveBeenCalledWith('root.inner.dogs.edit');
  });
});
```
###### frontend/src/app/dogs/dogsControllers.js
```javascript
@Controller("DogCtrl", ['$state', 'dog'])
export class DogCtrl {
  constructor($state, dog) {
    this.$state = $state;
    this.dog = dog;
  }
  
  // what happens when this function gets called?
  edit() {
    this.$state.go('root.inner.dogs.edit');
  }
}
```
### <a name="template"></a>Template
Templates handle all normal Angular directives, as well as any custom directives and components you make. Please, for the love of ʕ •ᴥ•ʔ, format your crazy Angular html so that it's readable. 

Note that you will need to use `ui-sref` rather than `href` to create links to a state within the application.
###### frontend/src/app/dogs/dogs-show.tpl.html
```javascript
seriously. not going to include a template example.
```
### <a name="assets"></a>Assets
Assets go in this following dir. Please try and keep these organized.
###### frontend/src/assets

### <a name="styling"></a>Styling
Your .sass files are stored here. Please, for more love of ʕ •ᴥ•ʔ, keep these sense-making. There are framework, partial, and state dirs. We will eventually complete a separate doc on how we should be structuring our CSS so they are less of a pain to work with.

Also, Don't touch framework. Just. don't.
###### frontend/src/styles