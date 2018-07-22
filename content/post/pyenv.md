+++
title = "Pyenv"
date = 2018-07-22T14:01:43+02:00
description = "A brief overview of a good Python package manager"
draft = false
toc = false
categories = ["programming"]
tags = ["pyenv", "python", "dev", "tool", "package", "manager"]
+++

I write Python code each day, for personal and professional projects.  
As I am working on multiple Python projects, old and fresh ones, I have to use different Python versions for those projects, from `2.7.2` to `3.7.0`.  
Also, I want to switch the Python version of my projects very easily and quickly, in case of we have to upgrade the Python version of the software.

I am a macOS user, and I use *a lot* [HomeBrew](https://brew.sh/).  
Unfortunately, HomeBrew is not a great solution to keep a trace of multiple versions of a given build (especially the interpreters)...  
I can take a look at each source code in [python.org](https://python.org), download the one a want, build it, and set manually all my paths in order to retrieve the Python version I want for a given project.  
But, we are in 2018, and I figured that someone already automatized this thing.

I was right.

This tool is called `pyenv`, and is a very simple Python package manager for *UNIX-like systems*.

## What is `pyenv`

`pyenv` is a Python package manager, focused on the interpreter and famous Python associated tools (`easy_install`, `pip`, etc...).  
`pyenv` was originally forked from `rbenv` and `ruby-build` for Ruby, and modified for Python.  
The project is composed, principaly, by shell scripts, and totalized nearly 12,000 stars on Github on 22th of July, 2018.

## Installation of `pyenv`

For macOS, using [HomeBrew](https://brew.sh/), the installation of `pyenv` is very simple: `brew update && brew install pyenv`.  
That's it.

If you are not on macOS, or if you don't want to use HomeBrew for that, you can check [the automatic installer](https://github.com/pyenv/pyenv-installer) on Github.

`pyenv` uses an internal system calls `shims` to enable the search of the Python interpreters (and associated tools), and enable autocompletion.  
In order to use those binaries, you just have to enable shims and autocompletion using `eval "$(pyenv init -)"` (remember to add this line on your `.bashrc` or `.zshrc`).

Ok - now, `pyenv` is installed on the system...but there is no interpreter on your system yet.

## Promises

### Install a toolchain

First, you have to install a given Python ecosystem version (interpreter, and associated tools) on your system.  
If you check first the versions you have installed, using `pyenv versions`, the output is empty.

_**NOTE**_: Along the `versions` option, there exists the `version` option. `versions` returns the toolchains you installed on the system, but `version` returns the toolchain(s) you are *using* on the current session.

First, let's try to install the version `2.7.15` of Python:
```
~ pyenv install 2.7.15
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-2.7.15.tar.xz...
-> https://www.python.org/ftp/python/2.7.15/Python-2.7.15.tar.xz
Installing Python-2.7.15...
python-build: use readline from homebrew
Installed Python-2.7.15 to /Users/acarette/.pyenv/versions/2.7.15
```

As you've seen, `pyenv` will download the `2.7.15` version of Python directly from the official Python servers, and will compile and install the binary directly *for* your system, on your system.  
If you are wondering where is the build, you can check the folder `.pyenv/shims` in your `$HOME`.

Now, let's install the `3.4` version:
```
~ pyenv install 3.4
python-build: definition not found: 3.4

The following versions contain `3.4' in the name:
  3.3.4
  3.4.0
  3.4-dev
  3.4.1
  3.4.2
  3.4.3
  3.4.4
  3.4.5
  3.4.6
  3.4.7
  3.4.8
  miniconda-3.4.2
  miniconda3-3.4.2
  stackless-3.4-dev
  stackless-3.4.1
  stackless-3.4.2
  stackless-3.4.7
```

Oups - what the problem here?  
The problem is we were not specific at all, when we asked to install the `3.4` toolchain.  
Indeed, the `3.4` version is a major version of Python 3, and there exists multiple minor versions of `3.4`.
So, `pyenv` was asking to specify the minor version of the major version we asked for: `pyenv install 3.4.8`.

_**Note**_: If you look more closely at the error message, you can see that you can install multiple versions of `anaconda` via `pyenv` - so, *the tool is not exclusive to Python builds but the Python ecosystem*.

### Global usage of a toolchain

If you have more than 50% of your Python projects that are using the same specific version of Python, maybe you want to use this version as a global one on your system.  
`pyenv` can override the *default* Python version of your projects using the `global` option.

So, if you want to use the `3.4.8` version of Python as the default one, ask to `pyenv` to override it: `pyenv global 3.4.8`.

Now that Python 3 is different than Python 2 (actually, it may be a new programming language by itself...), maybe you want to override a default toolchain for Python 2 and for Python 3 too.  
You can do that using `pyenv global 2.7.15 3.4.8`.

Using this feature, when you want to use `python2`, it will reach Python `2.7.15`.  
Also, if you want to use `python3`, it will reach Python `3.4.8`.

You can see this behaviour on typing `pyenv version` (*no `s` at the end of `version`!):
```
~ pyenv version
2.7.15 (set by /Users/acarette/.pyenv/version)
3.4.8 (set by /Users/acarette/.pyenv/version)
```

Now, what happen when we want to use `python`?

When we asked to override the default toolchains, we gave to `pyenv` an order: `2.7.15` *before* `3.4.8`.  
So, using this configuration, it will reach Python `2.7.15` first.  
If you want to override this behaviour, in using Python 3 instead of Python 2, you can do that switching the previous versions: `pyenv global 3.4.8 2.7.15`.  
Using this configuration, if you launch `python`, it will be the `3.4.8` toolchain:
```
~ pyenv version
3.4.8 (set by /Users/acarette/.pyenv/version)
2.7.15 (set by /Users/acarette/.pyenv/version)
```

### Local usage of a toolchain

The previous section explains how to define *defaults* toolchains in the entire system.  
Now, what if you want to work using different toolchains, on different projects?

`pyenv` can override the default Python toolchain for a given path (or project), creating a `.python-version` file at the root of your Python project.  
To do that, you need the `local` option.

So, using the `local` command, on a local project, each time I will want to use `python`, the `shims` will check if an overrided Python version has been done *for this project*.  
If a `.python-version` file exists in the current project, `pyenv` will launch the toolchain described in the file.
Otherwise, `pyenv` will execute the first default toolchain.

Ok, let's try:
```
~ mkdir -p Code/pyenv_test
~ cd Code/pyenv_test
~ pyenv version
3.4.8 (set by /Users/acarette/.pyenv/version)
2.7.15 (set by /Users/acarette/.pyenv/version)
~ pyenv local 3.6.2
~ pyenv version
3.6.2 (set by /Users/acarette/Code/pyenv_test/.python-version)
```

_**NOTE**_: like the `global` option, multiple Python versions: `pyenv local 2.7.15 3.4.8`, with the same priorities than before.

### Interactive usage of a toolchain

Python is a dynamic and interactive programming language and, sometimes, you just want to check _something_ in the standard library, or just try to hack _something_ on the interpreter.

Just imagine you are working on a given project, using the latest version of Python, when a colleague ask you to verify a very small piece of code using the version `3.6.2` of Python - maybe because something is struggling with the library standard of this specific version.  
The code to test is so small that it would be overkill to create a virtual environment to test it, and to put in on a `totest.py` file.

No, you want to test the code *in a Python interpreter*.

Currently, using `global` and `local`, you can't do that.  
So, `pyenv` contributors added the `shell` option, in order to launch a specific version of Python *in your current shell session*.

```
~ pyenv version
3.4.8 (set by /Users/acarette/.pyenv/version)
2.7.15 (set by /Users/acarette/.pyenv/version)
~ python -V
Python 3.4.8
~ pyenv shell 3.6.2
~ python -V
Python 3.6.2
```

Pretty convenient, isn't it?? :)

## Conclusion

As you noticed in the *Promises* section, `pyenv` offers great features to work easily with different Python toolchains, without scratching your head on how you can install different versions of Python in your system, for a given project.  
Of course, `pyenv` is not perfect - I still wait to install a toolchain *only* for a specific project, and let the tool to drop out the version if I delete the project - but it's a very good beginning in managing your Python toolchains.
