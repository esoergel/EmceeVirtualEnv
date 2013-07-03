# EmceeVirtualEnv

-----

EmceeVirtualEnv adds some shortcut utils for developing django projects in a virtualenv (as you should).  In particular: easy assignment of environment variables, shortcut aliases to run django commands from anywhere (not just in the directory with manage.py).

To start off, take full advantage of virtualenvwrapper.  Install it if you haven't already.

    $ pip install virtualenvwrapper

Open ~/.bashrc with a text editor and add the following to the bottom:

    export WORKON_HOME=~/virtualenvs/
    alias setvenv='source $WORKON_HOME/setvenv'
    source /usr/local/bin/virtualenvwrapper.sh

Close .bashrc and either restart your terminal session or run `$ source .bashrc` to activate the changes.
`$WORKON_HOME` is the directory where all your virtualenvs and activation scripts will be stored.  If you've been using virtualenv wrapper and didn't set this manually before, this stuff is probably stored somewhere like `~/.virtualenvs/`


### Adding EmceeVirtualEnv Customizations

Copy this directory into your `$WORKON_HOME` to overwrite the default postactivate and postdeactivate.  Do this manually or navigate to the directory where this README is stored and run the following:

    $ cp * $WORKON_HOME

(warning: this will overwrite postactivate and postdeactivate, so copy the commands over manually if you've made customizations you want to keep)


### Setting up a new project
Go to the project's root and run

    $ mkvirtualenv myproject
    $ setvenv

this second command mimics the behavior of virtualenvwrapper's setvirtualenvproject command; it sets the current directory as the root of the project associated with the virtualenv.  It also creates a venv file.  More on that later.


### Commands
#### VirtualEnvWrapper
VirtualEnvWrapper gives you several useful commands (some are only available if you've run `setvenv` or `setvirtualenvproject`):

* `$ workon myproject` - activates the virtualenv and puts you in the project root.
* `$ cdproject` - puts you in the project root
* `$ cdvirtualenv` - puts you in the root of your virtualenv directory
* `$ cdsitepackages` - puts you in the directory where all your virtualenv's libraries are stored
* `$ lssitepackages` - lists libraries in the virtualenv
* `$ add2virtualenv directory1` - adds directory1 to your pythonpath
* `$ deactivate` - leave the virtualenv

These commands are all vanilla virtualenvwrapper.  For more, [read the docs](http://virtualenvwrapper.readthedocs.org/en/latest/)

#### Aliases
EmceeVirtualEnv adds some shortcut aliases (defined in postactivate) so they're accessible from all your environments.  To add or edit these global aliases, put them in $WORKON_HOME/postactivate, or if you only want them in a specific virtualenv, directly in your venv file.
* `dj` is an alias for `django-admin.py` (works the same as ./manage.py, but from anywhere)  
* `djrun` is an alias for `django-admin.py runserver`


## The `venv` file
This file sits in the root of your project and stores environment-specific stuff.  It sets a few django-specific variables, but they're generated, so it's worth making sure they came out correctly.

* DJANGO\_PROJECT\_ROOT should be the root of your django project - the location where manage.py goes.
* DJANGO\_SETTINGS\_MODULE should point to the settings module you want to use for this project, this is used by django-admin.py for the shortcut aliases.

Open up the file and see the comments for more specifics.

You should really use environment variables to store stuff like your SECRET_KEY so it doesn't go in source control.  So as not to defeat this purpose, venv should also be excluded from source control.  Add 'venv' to your .gitignore file or lookup using a global .gitignore if you can't/don't want to do that.

To set an environment variable everytime you enter your virtualenvironment, add another export statement to your venv file (copy the format of the ones already in there if you're unfamiliar).

----

Check out the ["more information"](more_information.md) file for implementation details.

Should I make a setup.sh script to automate the inital setup?


Thanks to [Richard Laskey](https://github.com/rlaskey) for BASH help.

