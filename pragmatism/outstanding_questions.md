# Outstanding Questions

Both in the sense of "these questions have yet to be answered by the documentation" and "gee, that's a *really* good question.

Maybe some of these become part of a FAQ? They'll certainly get links to the appropriate place in this book once they're answered.



* `on-` and `bind-`  properties.  What they do, how to use them w/ usage
  examples, and where they come from.  (Are they part of A1atscript? something
  in `src/framework`?)
* When making a `@Component`, `@Controller` etc. - I’m not sure what goes in
  the annotation, and what goes in the object you declare outside the
  annotation.
* When things need to be part of a `@Module` and when they can just be declared
  with other annotations.  (e.g. I notice that `@Components` often don’t have
  an `@Module,` but still get imported elsewhere as if they are a module.  But
  I see other `@Components` that do have an `@module.`  example:
   `src/common/components/adminNav.js`)
* Evan and Patricia discussed that xing-book should have a general “how to
  construct a `@Component`”) flow.
* `@View` (most Yoric components) vs `@Template` (much of
  `src/common/components` ).  Which to use and why?
* `@Module` vs `@ModuleAs`
* ( `@View({ template: ‘thing’ })` -- why does this require a separate annotation
  instead of something else?
* `@DirectiveObject` vs `@component.` latter is necessary for
  recursive directives?  Examples:  `NavBar` and `LocationList`
* In general, a list of all a1atscript annotations and their functionality,
  particularly as they relate to each other.
