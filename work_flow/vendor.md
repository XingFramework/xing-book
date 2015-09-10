# The Xing Work Flow - Adding Vendor Files
So you want to install an npm package? If you've checked with your project lead and got the green light, this is the general list of steps to take when installing an npm package in a Xing project.

## Steps
While in the frontend dir, run the following commands:
* `npm prune`
* `npm cache clean`
* `npm install <package name> --save-dev`

Then do these:
* Add module to app.js
* Add file path to build.config.js (for both js and css files as needed)


Run `npm shrinkwrap --dev` in the frontend dir, and `rake develop` in the root dir. (In that order)

You may or may not have to do a forced reload `cmd+shift+r`. I've certainly had to, for reasons I don't totally understand.

## Example (for angular modules)
Let's say you're trying to install the imaginary npm package lrdAwesomeFortOffice. You go to npmjs.com and see that the registered name is lrd-awesome-fort-office. 

This example is for installing an angular module kind of node package. If you're installing a different sort of node package, please add another example and expand our docs!

In the console:

```
npm prune
npm cache clean
npm lrd-awesome-fort-office --save-dev
```

Go check frontend/package.json, you should see the package listed in the devDependencies. 

Go to the frontend/node_modules dir and find the lrd-awesome-fort-office dir. There should be a file named lrd-awesome-fort-office.js where you can find the module name. Something like:

```
angular
  .module('lrdAwesomeFortOffice', [])
```

Go to frontend/app.js. You will want to include the lrdAwesomeFortOffice module in the application. You'll need to put it in quotes. 

```
var app = new Module(appName, [
'lrdAwesomeFortOffice'
]
```

Next, go to build.config.js. In the vendor files section, add the js, css, and asset files you need. 

```
vendor_files: {
    js: [
      'node_modules/lrd-awesome-fort-office/lrd-awesome-fort-office.js'
    ],
    css: [
      'node_modules/lrd-awesome-fort-office/lrd-awesome-fort-office.css'
    ],
    assets: []
  },
};
```

Go back to the console. In frontend dir, run:
```
npm shrinkwrap --dev
```
When that's done, in root dir run:
```
rake develop
```
Bam.
