# Styling the projects list

Let's improve the appearance of the Projects list.

Xing gives you some basic tools to make styling your applications easier. You'll write your styles using [SASS](http://sass-lang.com/) and we provide some default mixins to get you started. We don't include a CSS Framework by default, other than Compass. 

Styles for a Xing application live in ``frontend/src/styles``.

## Changing Style Constants

First, let's get rid of the blue background in the application. Xing provides several default constants for things like colors. You can override them with your own by putting them in `frontend/src/styles/_constants.sass`, which looks like this:

```scss
// These are default constants for Xing
// Uncomment to override any of these

// Breakpoints
// $small: 500px
// $medium: 768px
// $desktop: 960px

// General content & nav layout
// @include breakpoint-set('to ems', true)
// $content_width: 960px

// $main_background_color: #67c

// $navbar_link_h_padding: 10px
// $navbar_link_v_padding: 10px
// $navbar_responsive_background_color: #ddd

// Success color for forms, flashes etc.
// $success_color: #0C0
// $error_color: #C00

// Admin menu & callout color
// $admin_menu_height: 22px
// $admin_background_color: #777755

// $link_color: #00e
// $call_to_action_color: #0e0

// $text_highlight_color: #777755

```

Uncomment the line for `$main_background_color` and change it to white, so it reads:

```scss
$main_background_color: white
```

Change other constants to see what kind of results you get.

## Modifying Styles For A State

In the `frontend/src/styles/` directory, you'll see a `states/` folder. Files in this folder describe styles that apply to a specific state or states. Open `_root_homepage.sass` in this folder. The contents should look like this:

`frontend/src/styles/states/_root_homepage.sass`

```scss

#root_homepage
  h1
    border-bottom: 1px black solid

// Sub-state styles can be put in the same file, if they aren't long enough
// to deserve a separate file.
#root_homepage_show

```

You'll see that styles in this file are nestend under css selectors with an id of the state name. Xing adds some extra magic to UI-router such that when a state is active, a css selector with the id of the state name is present on the page. That way, you can right styles for when this id is present, and your styles will only get applied inside that states template.

Let's go ahead and add some styling to our list:

```scss
#root_homepage_show
  line-height: 2em
  #projects
    border: 1px solid #ccc
    border-collapse: collapse
    th, td
      padding-left: 5px
      padding-right: 5px
      border: 1px solid #ccc
    th
      font-weight: bold
    tr:nth-child(even)
      background-color: #f2f2f2
```

There that looks better!

### Aside: how organizing your css by router state works

UI-Router works by rendering one template for each level of your state tree.  Each template that's not the leaf of the branch should have a `<ui-view>` tag somewhere in it.  When the next level is rendered, the next template "down" in the tree will get plugged in as the contents of the `<ui-view>` tag from the level above.  

If you poke through the files in this project inside `frontend/src/app`, you'll find `root.tpl.html`, `homepage/homepage.tpl.html` and `homepage/homepage-show.tpl.html`.  These correspond to the three levels of the route `root.homepage.show`, and the first two have `<ui-view>` tags, where the next one in the chain gets rendered.

As a convenience, Xing provides the Angular directive `xng-state-attrs`.  If this directive is used as an attribute on each `<ui-view>`, that `ui-view` will be decorated with an id and a class indicating which part of the state tree is being rendered. If we ignore the other components of the page, the rendered homepage's HTML looks like this:

```html
<ui-view class='root' id='root'>
  <ui-view class='homepage' id='root_homepage'>
    <ui-view class='show' id='root_homepage_show'>
      Visible homepage contents here
    </ui-view>
  </ui-view>
</ui-view>
```

These classes and IDs turn out to be a powerful way to organize your style rules.  By convention, Xing projects put all the rules for a state in their own file.  But if you were to put them all in one file, you can see how grouping by the state tree can be really useful.  For example, the login page is root.inner.sessions,  the password reset request page is root.inner.passwordRequest, and the password reset page is root.inner.passwords. These three interfaces likely have similar styling, so you could group most of the styles under #root_inner, and specific rules under the sub-states:

```scss
#root_inner
  // CSS rules common to all three pages, including form formatting
  
#root_inner_sessions, #root_inner_passwords
  // CSS rules common to just these two interfaces
```

