# Styling the projects list

Let's make some improvements to the appears of the Projects list.

Xing gives you some basic tools to make styling your applications easier. You'll write your styles using [SASS](http://sass-lang.com/) and we provide some default mixins to get you started. We don't include a CSS Framework by default, other than Compass. 

Styles for a Xing application live in ``frontend/src/styles``. The styles directory looks like this:

    styles
        partials
        states
        _constants.sass
        main.sass

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