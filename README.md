# What's this?

A starter repo for students in University of Montana's CS 136.

Students can clone this repo and write programs as needed for homework.

## Directory structure

- `src`: Put your work here (homework, etc).
- `booksite/examples`: Examples provided with the textbook
- `booksite/stdlib`: Library code provided with the textbook. It should not be called "stdlib" as that is a title generally reserved for the library that is included with the language itself, but that's what the book calls it and it seemed even more confusing to have a different name than what the book uses.
- `lecture-notebooks`: Jupyter notebooks with the examples created during the lectures.

# Setup

## Python
Install Python 3.7. 

- If you’re using a popular Linux distro (Ubuntu, etc) or macOS, install python 3.7 from the package manager.
    - Ubuntu: `sudo apt-get install python3.7 python3-pip python3-tk`
        - Other linux distros should have a similar equivalent.
    - macOS [MacPorts](https://www.macports.org/): `sudo port install python37 py37-pip py37-tkinter`
    - Otherwise, download [Miniconda](https://conda.io/miniconda.html) 
- Linux or macOS are suggested. For what we'll be doing, Windows is more difficult to set up as a development environment. Windows is not commonly used for modern development outside of a few specific areas (e.g. games), so users typically have issues with instructions that don't work on Windows, code that expects Unix or Linux behavior, etc. You will probably want to dual-boot into Linux (Ubuntu is a good default choice) or use a VM like the one provided here (see below) or even try using WSL if you're brave. It's almost certain that regardless of how you developed it, the server running your code in production will be Linux, so the sooner you get used to Linux the better off you'll be...

## Pipenv

See detailed instructions below under "Installing pipenv".

## Clone the repo (command line)

If you're comfortable using the command line, clone the repository with `git`:

```
git clone https://bitbucket.org/marshallpierce/cs136-template.git
```

This will create a directory called `cs136-template`, but that's just `git` defaulting to using the name of the repository. You can rename the directory to something else if you like.

Once inside the resulting directory, run:

```
pipenv sync
```

Then, open the project in PyCharm and it should automatically use the python version specified via pipenv.

## Clone the repo (PyCharm)

Clone the repo inside PyCharm (url: `https://bitbucket.org/marshallpierce/cs136-template.git`) and it should invoke pipenv to set up the virtualenv and python version for you.

## Configure PyCharm

Open PyCharm's Preferences window and go to Project Structure. For the `src`, `booksite/examples`, and `booksite/stdlib` directories, select the directory and click the "Mark As: Sources" button (in blue) so that PyCharm knows that's where source code resides.

# Running scripts on the command line

Use `pipenv run` to run one-off commands, like this:

```
pipenv run python --version
```

Or, if you'll be running multiple commands, it's probably easier to have a shell with the correct python, etc, already set up:

```
pipenv shell
```

This will drop you into a subshell, at which point you could run `python --version` to see that you have the right python version, etc.

## Module structure

PyCharm should be set up (as per above) with the right source roots, but when running `python` from the command line, it does not have that info, so you will need to provide it. A reasonable way to do this that doesn't require any permanent configuration changes (which are best avoided as they might conflict with other python projects you have) is to set the `PYTHONPATH` environment variable. This is an environment variable that `python` reads when it starts up to know where to import modules from.

You can set any environment variable for one shell command by prefixing the command with `VARNAME=value`.

As an example, the `date` command will honor the `TZ` variable (for "time zone"), so you could see the date in `America/Los_Angeles` with

```
TZ=America/Los_Angeles date
```

or in UTC with

```
TZ=UTC date
```

We can use this mechanism to set `PYTHONPATH` for a `python` command. The directories we want to include are `src` and `booksite/stdlib`, so an example python invocation would be:

```
PYTHONPATH=src:booksite/stdlib python booksite/examples/harmonicf.py 1 2 3 4
```

The paths (separated with a colon `:`) in `PYTHONPATH` are relative paths, so you will need to adjust them accordingly if you don't run `python` from the root directory of this repo.

Also note that we run `python` directly, which implies that we're already in the subshell created with `pipenv shell` that sets up the correct Python version, etc. If not in the subshell, use `pipenv run python` instead of just `python`.

# Textbook code

The example programs and the stdlib are already included in this repo; the example data files are not since it's a bit large (>300MiB). You can download the data from the ["booksite"](https://introcs.cs.princeton.edu/python/code/index.php).

## Graphics demos and Tkinter

For various boring reasons, code that tries to display graphics (using Python's Tk integration "Tkinter") is unlikely to work unless running on Linux. If you want to see demos that use graphics and it's not working for you, try setting up the provided VM (see below) and running them from there. 

Because doing graphics with Tk is generally painful, we will avoid graphics as much as practical in this class.

# Installing pipenv

If you're on macOS, you can use [MacPorts](https://www.macports.org/) (`sudo port install pipenv`) or [Homebrew](https://brew.sh/) (`brew install pipenv`) to install pipenv. Some Linux distros (Fedora, and perhaps others) may also have packages available for pipenv, but most do not. See the [official docs](https://pipenv.readthedocs.io/en/latest/#install-pipenv-today).

If you're comfortable with virtualenvs, use virtualenvwrapper to make a virtualenv dedicated to pipenv. Otherwise, read on to see how to use pip to install pipenv.

## Installing on Ubuntu via pip
 
If `pip` isn't installed yet, install it:

```
sudo apt-get install python3-pip
```

Install pipenv for your user:

```
pip3 install --user pipenv
```

pipenv is now installed in your user's homedir, but we need to add it to `PATH` to make it usable from the shell. This is done in your per-user shell config file. If you're using bash, `~/.bashrc` should work, or with zsh, use `~/.zshrc`. Either way, put this at the bottom of the file (it's ok to create the file if it doesn't exist). On Ubuntu, user-installed python packages are in `~/.local/bin`; other systems may vary (try `python3.7 -m site --user-base` to see where it is on your system -- append `/bin` to the output)

```
export PATH=$PATH:$HOME/.local/bin
```

Once you've done that, open a new terminal to use your newly updated shell config.

`cd` to this project's directory and run pipenv:

```
pipenv sync
```

# VM

In the `/vm` directory, there's a Vagrantfile that will set up an Ubuntu VM. It's not pleasant to work inside a VM, but if all else fails, install [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) or another virtualization system and run `vagrant up && vagrant reload` to bring up a VM. It will take some time as it downloads a lot of packages.

# Jupyter

Jupyter notebooks should only be used for little snippets of code for quick demos, not for more substantial development.

Jupyter is a dependency in the `Pipfile`, and PyCharm knows how to run the Jupyter server, so open a `*.ipynb` file and run a cell. It will ask for an external Jupyter server URL. Click "Cancel", then click "Run Jupyter Notebook" in the warning message that pops up after that to have PyCharm run the server for you. (It will create a run configuration for you also.)

If you want to run the serer manually:

```
PYTHONPATH=src:booksite/stdlib pipenv run jupyter notebook
```

Note the URL with the token that it outputs on startup and provide that to PyCharm when trying to run a notebook so that it can authenticate with the Jupyter server.
