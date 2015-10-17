# Debugging tricky JS:

Here's the problem: you're trying to debug something in Javascript that fails as soon as you open the page. Worse: whatever the failure is, it's so bad that it locks up Chrome and bogs down your machine. Bad times.

How to go about attacking this kind of problem.

First of all, make sure the debugging browser is disposable. Do this:

```
open -a "Google Chrome" --args --user-data-dir=/tmp/fake-chrome
```

or

```
chrome --user-data-dir=/tmp/fake-chrome
```
if you're on Linux.

This'll start up a completely isolated instance of chrome - no plugins, and no association with your normal browser.

Start devtools - I usually right-click and choose "inspect element" but Ctrl-Shift-I will open them, too.

Next, go to the Sources tab and look in the upper right-hand corner. You should see a little stop sign with a pause button in it. Click that to toggle it on.  It's "break on exceptions" - so when something goes wrong in the code, you'll stop right there.

*Now* open up the problem page in the disposable browser. With luck, you'll stop at the source of an exception right away. If you go into a hard lock, though, you can kill the browser and start afresh.

The next good trick is to use the JavaScript `debugger` keyword. Chrome respects this one, and it'll stop as long as devtools it open. Put it early in the code that's causing problems, and set more specific breakpoints from there.
