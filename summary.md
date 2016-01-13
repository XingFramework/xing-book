# Summary

The Xing Framework is a combined web development environment for building both a server backend (in [Ruby on Rails](http://rubyonrails.org/)) and a single-page browser frontend (in [AngularJS](https://angularjs.org/)). 

The Xing Framework provides integration and coordination of these two frameworks.  Specifically:

## Features: Why you should use Xing for your next web application
* **Development Support**. Tooling for building and launching the entire stack in development. With a single command, you can compile and boot your whole frontend and backend stack at once and open your app in a browser.
* **API Language**.  A pre-defined, consistent, and hypermedia-enabled JSON API language.
* **API Support**. Base classes in both AngularJS and Rails for consuming and producing hypermedia JSON resources in the Xing format.
* **ES6 / ESNext**. Xing includes a complete build and transpilation environment so you can safely work in up-to-date versions of JavaScript. 
* **Testing Support**.  Pre-structured testing environments for frontend unit tests (in [Jasmine](http://jasmine.github.io/)), backend unit tests (in [rSpec](http://rspec.info/)), and true end-to-end tests of the entire stack (in rSpec with [Capybara](https://github.com/jnicklas/capybara)).
* Predefined, sensible default **file hierarchy**.
* Predefined, sensible default **build architecture** (using [Grunt](http://gruntjs.com/)).
* Predefined, sensible default **deployment architecture** (using Capistrano).
* **Angular2 Ready**. The Xing frontend style uses AngularJS-2-like TypeScript annotations via  [A1Atscript](https://github.com/hannahhoward/a1atscript), allowing for easier conversion of your app to A2 down the road..
* **Secure Authentication** with token auth.

## Future Features

* **Mobile Development**.  Xing supports multiple front-ends. In future versions of Xing, we hope to build complete support simultaneous building of browser, iOS, and Android apps via Cordova while allowing reuse of nearly all the AngularJS codebase across all three platforms.
* **Angular2**. The Xing Framework was built
* **CMS Tools**. An upcoming version of Xing will include modules for a set of default content management classes and resources, usable for managing editable content in most web projects.