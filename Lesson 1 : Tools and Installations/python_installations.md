# Tools, Environment & Dependencies

## Install Python

Using Virtual Environment

### Why using virtual environment?

A virtual environment is a named, isolated, working copy of Python that that maintains its own files, directories, and paths so that you can work with specific versions of libraries or Python itself without affecting other Python projects. Virtual environments make it easy to cleanly separate different projects and avoid problems with different dependencies and version requirements across components.

### Using conda

Conda is an open source package manager tool and environment management system. It is mostly used for created isolated Python environments but it can package and distribute software for any language.

#### Create environment

```
conda create -n myenv
conda create -n myenv python=3.6  # with a specific python version
```

List environments

```
conda env list
```

Activate environment

```
conda activate myenv
```

Deactivate environment

```
conda deactivate
```

View a list of the packages in an environment

```
conda list
conda list -n myenv
```

Remove an environment

```
conda env remove --name myenv
```

Check conda version

```
conda info
```

Update conda

```
conda update conda
```

### Using virtualenv

virtualenv is a tool to create isolated Python environments.

The full documentation is available at http://pypi.python.org/pypi/virtualenv, from which parts of this text has been taken/modified.

#### Installation

These instructions might vary depending on your operating system. The next lines are the instructions for Ubuntu.

Install setuptools:

```
$ sudo apt-get install python-setuptools
```

Install virtualenv and pip (actually, pip should be installed automatically as a dependency of recent virtualenv versions):

```
$ sudo easy_install virtualenv pip
```

Creating a Python sandbox
Create a virtual environment

```
$ virtualenv yourenv
```

or

```
$ virtualenv --no-site-packages yourenv
```

If you build with the optional --no-site-packages switch, your virtual environment will not inherit any packages from your “global” site-packages directory. This can be used if you don’t have control over site-packages and don’t want to depend on the packages there, or you just want more isolation from the global system.

Activate the virtual environment

```
$ source yourenv/bin/activate
```

This will change your $PATH to point to the virtualenv bin/ directory, and update your prompt. This is all it does. If you use the complete path like /path/to/yourenv/bin/python myscript.py, you do not need to activate the environment first. You must use source because it changes the environment in-place.

Adding custom Python modules to the virtual environment

```
(yourenv)$ pip install 3rdPartylibrary
```

If you have activated the virtual environment, running pip within your environment will install 3rdPartylibrary only to this virtual environment.

Deactivate the virtual environment
After activating an environment you can use the function deactivate to undo the changes.

```
(yourenv)$ deactivate
```
