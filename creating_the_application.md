# Creating The Application

## Installing the Xing Framework rubygem

We'll start by installing xing-framework. Make sure you're using a compatible version of Ruby (we prefer >= 2.2).  If you're using a Ruby manager with gemsets (like RVM), you probably want to [create a gemset now](https://rvm.io/gemsets/basics) and switch to it. If you're not using gemsets, don't worry about it.

## Generating the application

    $ gem install xing-framework
    Fetching: xing-framework-0.2.0.gem (100%)
    Welcome to the exciting new world of Xing web development!
    Successfully installed xing-framework-0.2.0
    1 gem installed
    
Let's create a new Xing project! Be aware that `xing new` will automatically generate directories and bundle a lot of gems.

    $ xing new crowdfundr


