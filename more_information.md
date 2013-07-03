## About VirtualEnvWrapper Hooks

Virtualenvwrapper provides a series of files in your $WORKON_HOME (mine is ~/virtualenvs/) that are run at various stages.  For example, postactivate is run immediately after activating a virtualenv with `workon myvirtualenv`.  Also in $WORKON_HOME, you'll see a directory for each of your virtualenvs.  This is where the actual packages are stored.  In each virtualenv directory (`$ cdvirtualenv`) you'll see a /bin/ directory that contains a second set of hooks.

The postactivate in $WORKON_HOME is run for all your virtualenvs, so that's where you put global config options.  The postactivate in $WORKON_HOME/myvirtualenv/bin/ is run only for the virtualenv "myvirtualenv", so it stores stuff relevant only to that project.

This is a somewhat inconvenient location, so EmceeVirtualEnv uses the global postactivate hook to look for a file called "venv" in the current directory (which will be the project's root as determined by `setvirtualenvproject`).  To change this directory, edit $WORKON_HOME/myvirtualenv/.project or hardcode the location of your venv file in the specific virtualenv's postactivate.

To create this venv file, EmceeVirtualEnv runs a BASH script in postmkproject.  If you already have a project associated with the virtualenv, you can run `$ $WORKON_HOME/postmkproject` to manually call that file and create the venv.


## Customizing EmceeVirtualEnv

There is a file called `EmceeVirtualEnv_config` in your $WORKON_HOME that sets some configuration options.  The customizations are pretty simple, so you can also edit the files directly.

MCV sets aliases in $WORKON_HOME/postactivate to be used only in virtualenvs, but you can add them to your .bashrc if you want them to be accessible everywhere.

If you would like to add the venv file automatically whenever you run mkvirtualenv, add $WORKON_HOME/setvenv to your global postmkvirtualenv file ($WORKON_HOME/postmkvirtualenv).  This is not done by default because setvenv assumes you are in the project root.  If you automate the running of the file, be sure to run mkvirtualenv from the project root when creating a new environment.

EmceeVirtualEnv unsets the DJANGO_SETTINGS_MODULE variable in $WORKON_HOME/postdeactivate so django-admin.py doesn't attempt to use that settings module outside of the environment.


To use a global .gitignore, use this command or look it up:

    git config --global core.excludesfile '~/.gitignore'
