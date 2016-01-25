# Tools We Use

Xing is the glue between front-end and back-end applications. It's nothing to trifle with, but it relies on other projects and frameworks to provide the actual frontend and backend.

## Backend: Ruby on Rails

Xing uses the venerable [Ruby on Rails](http://rubyonrails.org/) HTTP framework as its default backend target. Documentation and guides for Rails are freely available on the internet. We recommend:

* [Rails Guides](http://guides.rubyonrails.org/) is the official documentation.
* [Rails Tutorial](https://www.railstutorial.org/) has served many aspiring Rails programmers well over the years.

Xing adds a number of components to the Rails backend. Mappers and Serializers almost entirely replace traditional Rails views, and Xing Controllers are much simpler and less variable than traditional controllers are. These concepts are explained in greater detail later in this book.

## Frontend: AngularJS

The frontend lacks a unified solution that compares to Rails for the backend. Therefore, Xing makes a number of selections from the technology field for its frontend.

* [AngularJS 1.4](https://docs.angularjs.org/guide) is the foundation for our tools. AngularJS is well supported, with good tools and documentation.
* [UI Router](https://github.com/angular-ui/ui-router/wiki) is used for application state management. The [API reference](http://angular-ui.github.io/ui-router/site/#/api/ui.router) is reasonably light and complete. There are some best practices that bear documentation, however.
* [A1Atscript](https://github.com/hannahhoward/a1atscript) reduces the amount of boilerplate Javascript and will ease the transition to AngularJS 2.
* [Compass](http://compass-style.org/) to compile [Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html) for Xing stylesheets. Granted, this adds a Ruby dependency to the frontend, and we're evaluating alternatives.

## Other components

* Build system: [Grunt](http://gruntjs.com/)
* Transpiler: [Traceur](https://github.com/google/traceur-compiler)
* Backend Testing: [RSpec](http://rspec.info/), [json_spec](https://github.com/collectiveidea/json_spec)
* Frontend Testing: [Jasmine](http://jasmine.github.io/)
* E2E Testing: [Capybara](https://github.com/jnicklas/capybara), [Selenium](http://www.seleniumhq.org/), [rspec-steps](https://github.com/LRDesign/rspec-steps)
* Backend Components: [ActiveModel::Serializers](https://github.com/rails-api/active_model_serializers)
