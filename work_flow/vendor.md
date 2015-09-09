# The Xing Work Flow - Adding Vendor Files
So you want to install an npm package? If you've checked with your project lead and got the green light, this is the general list of steps to take when installing an npm package in a Xing project.

## Steps
While in the frontend dir, run the following commands:
* `npm prune`
* `npm cache clean`
* `npm install <package name>`

Then do these:
* Add file path to build.config.js (for both js and css files as needed)
* Add module to app.js
* Add to frontend/package.json

Run `rake develop` in the root dir, and `npm shrinkwrap --save-dev` in the frontend dir.

You may or may not have to do a forced reload `cmd+shift+r`. I've certainly had to, for reasons I don't totally understand.


