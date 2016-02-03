# Where to Report Issues

Xing is composed of many submodules, both NPM modules and Ruby gems.

We are working on a permanent solution and architecture for problem reporting and communication.  For now, all issues with Xing (including bugs, suggestions, etc.) should be reported to one of two places:

* If the issue affects the overall framework or integration of multiple components, [The GitHub issues page for the xing-framework rubygem](https://github.com/XingFramework/xing-framework/issues).
* If the issue clearly affects just one gem or module, The GitHub issues page for that repository. (See the [XingFramework GitHub organization](https://github.com/XingFramework)).  Specific options include:
  * [xing-application-base issues](https://github.com/XingFramework/xing-application-base/issues) (Template for a new Xing application)
  * [xing-frontend-utils issues](https://github.com/XingFramework/frontend-utils/issues) JavaScript modules used by all Iing frontend applications.
  * [xing-backend issues](https://github.com/XingFramework/xing-backend/issues) Rails engine to create a Xing backend API.
  * [xing-framework-sass](https://github.com/XingFramework/xing-framework-sass/issues) Sass stylesheet build system for Xing frontend.
  * Many others, see the GitHub org.

At present, the latter gem is primarily just the new application generator, but because of the name we will use it as the clearinghouse for framework issues.