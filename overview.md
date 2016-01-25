# Summary

The Xing Framework is a web development environment for building both a server backend in [Ruby on Rails](http://rubyonrails.org/) and a single-page browser frontend in [AngularJS](https://angularjs.org/). The Xing Framework provides integration and coordination of these two frameworks.

## Features:
* **Development Support**. Tooling for building and launching the entire stack in development. A single command compiles and boots your frontend and backend, then opens your app in a browser.
* **API Protocol**.  A predefined, consistent, and hypermedia-enabled JSON-based protocol.
* **API Support**. Base classes in AngularJS and Rails for consuming and producing hypermedia JSON resources in the Xing format.
* **ES6/ESNext**. A complete build and transpilation environment so you can safely work in up-to-date versions of JavaScript.
* **AngularJS2 Ready**. Uses AngularJS-2-like TypeScript annotations via  [A1Atscript](https://github.com/hannahhoward/a1atscript), easing future conversion.
* **Testing Support**.  Prestructured testing environments for front-end unit tests ([Jasmine](http://jasmine.github.io/)), back-end unit tests ([rSpec](http://rspec.info/)), and true end-to-end tests of the entire stack (rSpec with [Capybara](https://github.com/jnicklas/capybara)).
* Predefined, sensible default **file hierarchy**.
* Predefined, sensible default **build architecture** ([Grunt](http://gruntjs.com/)).
* Predefined, sensible default **deployment architecture** ([Capistrano](http://capistranorb.com/)).
* **Secure Authentication** with token auth.

## Future Features:

* **CMS Tools.** Modules for default content management classes and resources, usable for managing editable content in most web projects.
* **Code generation.** Cross-environment, cross-language code generation tools.
* **Mobile Development.**  Xing supports multiple frontends. Future versions may support simultaneous building of browser, iOS, and Android apps via Cordova and enable reuse of the AngularJS codebase across all three platforms. 
* **Built Futureproof.** New SPA frameworks will be integrated as they become prominent, including AngularJS2.