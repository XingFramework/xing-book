# A-1 AtScript

Angular 2 makes use of a language construct called an 'annotation' - essentially, a quick way to describe some re-used behavior. In order to streamline the transition between Angular 1 and 2, Xing uses a library called A1AtScript to provide annotatin support. It also provides a set of annotations to make Angular 1 code look much more like Angular 2 code.

## General Use

### Angular Type Annotations

##### src/app/ExampleStuff.js
```javascript
import {Controller, Service} from 'a1atscript';

@Controller('ExampleController', ['$scope', 'SomeService'])
export class ExampleController {
  constructor($scope, SomeService) {
  }
}

@Service('ExampleService', ['SomeService'])
export class ExampleService {
	constructor(SomeService) {
	}
}
```

In general, in Xing projects, we prefer the `@Controller` syntax over the equivalent `angular.module('named').controller` syntax. You'll note, however, that there's no reference to the module. Surely it isn't magic, right?

### Setting up modules

```javascript
import {Module} from 'a1atscript';
import {
  ExampleController,
  ExampleService
} from './ExampleStuff.js';

import { AnotherModule } from './anotherA1AtScriptModule.js';

export var MyModule = new Module('MyModule', [
	AnotherModule,
	ExampleController,
	ExampleService,
	'aRegularAngularModule'
]);
```

Note you can mix other modules, controllers, and services all together in the list -- A1AtScript will figure out how to sort out the module definition.

You can include regular angular modules by just referencing them as strings.

### Compile your main app module

###### src/app/app.js
```javascript
import {bootstrap, Module} from 'a1atscript';
import { MyModule } from './myModule'

var AppModule = Module('AppModule', [
  MyModule
]);

bootstrap(AppModule, "myAppPrefix");
```

This is the part where it all comes together. The `bootstrap` call starts from a root module object (defined in a1atscript) and traverses all of it's dependencies, and all of theirs and so on. Where it finds `@Controllers` _et al_, it adds them as items the module provides via Angular DI. Where it finds other modules, it sets the dependencies for Angular and continues to process.

The key thing to know is that, in order for your code to be made available to Angular, it has to be in the dependency list of a module, and that module has to be reachable from your app's root module. It's not difficult, but it can sometimes be surprising when new features don't appear because they haven't been added.

## Useful Extras

### Shortform notation

If you want to quickly define a module with only one component... just use two annotations

```javascript
@AsModule('ServiceModule')
@Service('ExampleService')
class ExampleService {
  constructor() {
    this.value = 'Test Value';
  }
}
```

### Get ready for Angular 2!

Angular 2 introduces an entirely new syntax for working with directives. The most common type of directive is a Component. The good news is with A1AtScript you can write components right now, using a syntax remarkably similar to Angular 2.

```javascript
@Component({
  selector: "awesome",
  properties: [ "apple" ],
  services: ["ExampleService"]
})
@View({
  templateUrl: "awesome.tpl.html"
})
class AwesomeComponent {
  constructor(exampleService) {
    this.exampleService = exampleService;
    this.test = "test";
  }
  setValue() {
    this.value = this.exampleService.value;
  }
}
```

There are two annotations in use there: `@Component` and `@View`, which is in alignment with the Angular 2 design.

The `@Component` takes a description object that can have the following fields:

| Field name     | Purpose
| ---:           | ----
| **selector**   | This is like a CSS selector, but the syntax is a very limited subset. Use `awesome` to describe an element, `.awesome` to describe a class-selected component, or `[awesome]` (equivalent to `restrict: 'E','C','A' in an Angular 1 directive)
| **properties** | A list of attributes which will be bound to the component controller. The difference between a simple string and a JS value is at use time - in the above example `bind-apple='2+2'` would set the value `4`, where `apple='2+2'` would set `"2+2"`.
| **services**   | Named items to be injected into the component object when it's created

The component object will be made available in the template based on its selector - in essence, the CSS punctuation gets stripped, and Angular normalization is applied to the resulting text. So for instance, all of `.example-thing`, `[example-thing]` and `example-thing` will all get access to the component object as `exampleThing`, as if `controllerAs: exampleThing` had been used in an old-style directive.

Use your component like this:
```html
<awesome apple="stringLiteral"></awesome> <!-- amesome.apple == 'stringLiteral' -->
<awesome bind-apple="'stringLiteral'.length"></awesome> <!-- awesome.apple == 13 -->
```

The `@View` annotation also takes a description object, which is much simpler:

| Field name | Purpose
| ---:       | ----
| **url**    | The url to fetch the template for this component from
| **inline** | The literal text of the template for this component

Use one or the other of `url` or `inline` in the `@View`.


If you're more familar with Angular 1.3 syntax, the above `@Component` example is the equivalent of this:
```javascript
angular.directive('awesome', function() {
  return {
  	restrict: 'E',
  	bindToController: {
  	  apple: "@apple"
  	  // a setter is created automatically on your
  	  // controller so that your controller can access this.apple
  	  ___bindable___apple: "=?bindApple"
  	},
  	controller, ['ExampleService', AwesomeComponent]
  	controllerAs: 'awesome',
  	scope: {},
  	templateUrl: "awesome.tpl.html",
  }
  function AwesomeComponent(exampleService) {
    this.exampleService = exampleService;
  	this.test = "test";
  }
  AwesomeComponent.prototype.setValue = function() {
    this.value = this.exampleService.value
  }
});
```
