# The Xing Frontend

Xing is built on AngularJS, the well-supported SPA framework by Google.  We incorporated a ES6 build process, as well as ES-future decorators to speed the development process and improve code structure.

## ES6 / ESNext

Xing uses current generation JavaScript, and includes an integrated build environment with [Traceur](https://github.com/google/traceur-compiler) to provide transpilation.

## UI-Router

Xing uses ui-router, the best currently available router for AngularJS 1.x. 

## Modularization & File Architecture

Xing organizes frontend javascript files into subdirectorie by feature, rather than by class type.  For example, in a ToDo app with 'items', you would put the module, state, controller, and templates together in a single directory, as follows:

* /frontend/src/app/items/items.js   # module
* /frontend/src/app/items/itemsControllers.js
* /frontend/src/app/items/itemsStates.js
* /frontend/src/app/items/items-show.tpl.html
* /frontend/src/app/items/items-edit.tpl.html

## A1Ascript

Todo...
