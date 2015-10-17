# Outstanding Questions

Both in the sense of "these questions have yet to be answered by the documentation" and "gee, that's a *really* good question.

Maybe some of these become part of a FAQ? They'll certainly get links to the appropriate place in this book once they're answered.



* `on-` and `bind-`  properties.  What they do, how to use them w/ usage examples, and where they come from.  (Are they part of A1atscript? something in `src/framework`?)
* When making a `@Component`, `@Controller` etc. - I’m not sure what goes in the annotation, and what goes in the object you declare outside the annotation.
* When things need to be part of a `@Module` and when they can just be declared with other annotations.  (e.g. I notice that `@Components` often don’t have an `@Module,` but still get imported elsewhere as if they are a module.  But I see other `@Components` that do have an `@module.`  example:  `src/common/components/adminNav.js`)
* Evan and Patricia discussed that xing-book should have a general “how to construct a `@Component`”) flow.
* `@View` (most Yoric components) vs `@Template` (much of `src/common/components` ).  Which to use and why?
* `@Module` vs `@ModuleAs`
* ( `@View({ template: ‘thing’ })` -- why does this require a separate annotation instead of something else?
* `@DirectiveObject` vs `@component.` latter is necessary for recursive directives?  Examples:  `NavBar` and `LocationList`
* In general, a list of all a1atscript annotations and their functionality, particularly as they relate to each other.
* What rules do we use to arrange the frontend files? How does that interact with the module loading configuration? Why must we repeat naming in the frontend? e.g. Why is there src/app/consumers/signup/consumerSignupStates.js as opposed to src/app/consumers/signup/states.js ?

* Need docco on how to test things that depend on promises (e.g. save actions)
* Wasn’t able to find docco on how to update a changed resource with Relayer. (README examples show only read actions for Relayer, not destructive actions).
* Wasn’t able to find docco on how to tell if a resource has been changed.  
* In general, it's hard to find example code for the current form for basic CRUD actions. Because:
  * MSW uses an old format and no Relayer
  * Xing CMS techniques are out-of-date
  * Yoric is a special case where everything is a component, so states/state controllers are rare.  I couldn’t, for example, find any files associated with updating changed resources. (Doesn't mean they aren't there' but while 'create' and 'show' appear frequently in filenames, 'edit' and 'update' do not so i wasn't sure where to look.)


## Tooling Feature Requests

* **Ability to turn off the auto-launch of browser by rake develop. ** I have many windows open with multiple profiles and it almost always picks the wrong one to attach a new tab. ```XING_AUTOLAUNCH_BROWSER=false``` would do it.
* **Build of the front-end before running feature specs**.  When working on a single spec, the following sequence is very slow - something like 5 minute cycle to test a very tiny change.
  1. ```backend $> rspec spec/features/thing_spec.rb``` #fail
  1. … small edit to template… 
  1. ```backend $> rspec spec/features/thing_spec.rb``` # fail, oops
  1. ```backend $> cd ..; rake develop```
  1. ...Wait for load...
  1. C-b C-x
  1. ```app-root $> cd backend```
  1. ```backend $> rspec spec/features/thing_spec.rb``` # pass

## General frustrations

* Code coupling:  Validation ENUMs (for ```<select>``` options, e.g.) must be repeated in FE and BE, and must be kept in sync.
* Code coupling: Strings in UX must be repeated in FE (templates) and feature specs.  Minor changes to labels, buttons, links etc. will break feature specs.
* Karma attached browser frequently disconnects, requiring reload to get tests/builds working again. Also, tests often stall if the browser window is in the background so must frequently be manually focussed.
