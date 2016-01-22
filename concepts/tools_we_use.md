# Tools We Use

Xing is the glue between frontend and backend applications. It's nothing to trifle with, but it relies on other projects and frameworks to provide the actual frontend and backend.

## Backend: Ruby on Rails

Xing uses the venerable [Ruby on Rails](http://rubyonrails.org/) HTTP framework as its default backend target. There's a huge amount of documentation and guides about Rails freely available on the internet. We recommend:

* [Rails Guides](http://guides.rubyonrails.org/) is the official documentation.
* [Rails Tutorial](http://guides.rubyonrails.org/) has served many aspiring
  Rails programmers well over the years.

Be aware that Xing adds a number of components to the Rails backend.  Specifically, Mappers and Serializers almost entirely replace traditional Rails views, and Xing Controllers are much simpler and less variable than traditional controllers are.

## Frontend: AngularJS

At the time of writing, there's much less of a unified solution to building front end browser applications than the Rails approach to the backend. Xing, therefore, makes a number of selections from the technology field for its frontend.

First of all, we use [Angular 1.4](https://docs.angularjs.org/guide) as the foundation for our tools. Angular is well supported and there are many tools and documentation around it.

Xing asumes the use of [UI Router](https://github.com/angular-ui/ui-router/wiki) for application state management. The [API reference](http://angular-ui.github.io/ui-router/site/#/api/ui.router) is reasonably light and complete. There are some best practices that bear documentation, however.

To reduce the amount of boilerplate Javascript related to the above, Xing uses [A1Atscript](https://github.com/hannahhoward/a1atscript), a contender in the overall attempt to address the transition to Angular 2.

We use [Compass](http://compass-style.org/) to compile [Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html) for stylesheets. Granted, this adds a Ruby dependency to the frontend. We're currently evaluating other options related to this issue.

## Other components

A number of other modules and tools out there are key to the function of Xing projects.  Here are some of the most important:

* Build system: [Grunt](http://gruntjs.com/)
* Transpiler: [Traceur](https://github.com/google/traceur-compiler)
* Backend Testing: [RSpec](http://rspec.info/), [json_spec](https://github.com/collectiveidea/json_spec)
* Frontend Testing: [Jasmine](http://jasmine.github.io/)
* E2E Testing Tools: [Capybara](https://github.com/jnicklas/capybara), [Selenium](http://www.seleniumhq.org/), [rspec-steps](https://github.com/LRDesign/rspec-steps)
* Backend Components: [ActiveModel::Serializers](https://github.com/rails-api/active_model_serializers)
