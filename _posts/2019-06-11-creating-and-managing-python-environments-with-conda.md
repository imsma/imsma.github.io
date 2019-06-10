---
layout: post
title:  "Creating and Managing Python environments with Conda"
author: sma
categories: [ Python ]
image: assets/images/posts/fotis-fotopoulos-790373-unsplash.jpg
description: "Creating and Managing Python environments with Conda"
tags: [featured]
---


[Conda](https://docs.conda.io/en/latest/){:target="_blank"} is the package manager for *Anaconda* Python distribution.In this article we will explore following topics. 

- What *Python environments* are.
- Why and when we need *Python environments*.
- How to create and manage *Python environments* using *conda*.

Please follow the official documentation  to [install conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html#regular-installation){:target="_blank"}.

## What Is Python Virtual Environment and why we need it?

*Virtual environment* helps to create a *sandbox* by isolating dependencies required by a specific *python* project. As a result each python project can have an isolated environment / sandbox with it's own dependencies, regardless of the decencies a-viable elsewhere on the same system.

## Why Use Virtual Environment?

In this section I will list some of reasons and benefits of creating and managing multiple virtual environments for *Python* development.

1. Major operating systems like *macOS* come with pre-installed version of *Python*, however the version installed with OS might be an older one and in some cases upgrading this default installation 
my compromise the proper working on system components. 
2. With default global installation using *pip*, it's not possible to install different packages with 
3. *Virtual environment* enables you to use different version of a same package for different projects.
4. By default *pip* installs the *python packages* globally, this may result in conflicts.
same name. However *virtual environment* makes it easy to install packages within the project sandbox.


## Creating a virtual environment with conda

Open a terminal / command prompt and run following command to create a new virtual environment.

```
conda create --name newenv
```

On execution of above command, conda will ask you to proceed, type `y` to proceed with creation of virtual environment.

```
proceed ([y]/n)?
```

### Creating a virtual environment with Python package

You can also specify a Python package during the creation of a virtual environment using following command.

```
conda create -n newenv numpy
```

Above command will install `numpy` package in `newenv` virtual environment, in other words the package `numpy` installed through above command will only be available within the sandbox of `newenv` virtual environment.

You can install a **specific version** of a python package as following

```
conda install --name newenv numpy=1.16.4
```

Use following command to install specific versions of multiple packages with creation of a new virtual environment.

```
conda create -n newenv urlopen=1.0.0 numpy=1.16.4 PyFEBOL=0.3.3
```

### Creating a virtual environment with Python version

During creation of a virtual environment, we can also specify a particular version of Python as following.

```
conda create -n newenv python=3.4
```

Use following command to create a new virtual environment with a specific python version and multiple packages.

```
conda create -n newenv python=3.4 urlopen=1.0.0 numpy=1.16.4 PyFEBOL=0.3.3
```

## Activating a virtual environment

Following tasks are performed when we activate a virtual environment.

1. Environment specific entries are added in `PATH`.
2. Any activation scripts specified for this environment are executed.

Run following command to activate a specific environment, please replace the `newenv` with your environment name.

```
conda activate newenv
```

## Deactivating a virtual environment

Run following command to *deactivate* currently active virtual environment, in context of this article following command will *deactivate* environment named `newenv`.

```
conda deactivate
```

## Listing available virtual environments

You can execute following command to list available virtual environments on your system.

```
conda env list
```

Output of `conda env list` command.

```
# conda environments:
#
untitled                 /Users/sma/.conda/envs/untitled
newenv                *  /Users/sma/.conda/envs/newenv
base                     /anaconda3
```

The symbol `*` indicates the currently active environment.

Alternatively you can run `conda info` command as following to print a list of available virtual environments on your system.

```
conda info --envs
```

Output of `conda info --envs` command.

```
# conda environments:
#
untitled                 /Users/sma/.conda/envs/untitled
newenv                *  /Users/sma/.conda/envs/newenv
base                     /anaconda3
```

## Using pip in a conda virtual environment

You can easily use `pip` package manager from within the virtual environment created using `conda`, let's explore the details.

1. Before you can use `pip`, you need to install it within  your virtual environment using `conda install` command. Following command will create a virtual environment named `newenv` and install `pip` in it.

    ```
    conda install -n newenv pip
    ```
2. Activate the newly created environment.

    ```
    conda activate newenv
    ```

3. Now you are ready to use `pip` in your conda virtual environment, in following example replace the `pip_command` with proper pip subcommand.

    ```
    pip pip_command
    ```

## Removing a conda virtual environment

You can use following command to remove a conda virtual environment from your system, please replace the name `newenv` with the name of environment that you want to remove from your system.

```
conda remove --name newenv
```

Run the `conda env list` command to verify the removal of virtual environment named `newenv`.

```
conda env list
```

Following output confirms that the environment named`newenv` is no longer available on our system.

```
# conda environments:
#
untitled                 /Users/sma/.conda/envs/untitled
base                   * /anaconda3
```

## Creating a virtual environment with environment.yml file

You can create conda virtual environment YAML file (environment.yml) instead of terminal commands, in this section we explore the process of virtual environment ceration using `environment.yml` file.

Following listing show basic structure of a `environment.yml` file.

```
name: name_of_virtual_enviornment
channels:
  - ...
dependencies:
  - ...
```

Following is a sample `environment.yml` file.

```
name: testenv
channels:
  - javascript
dependencies:
  - python=3.4
  - numpy=1.16.4
  - urlopen=1.0.0
  - PyFEBOL=0.3.3
  - flask
  - pip:
    - Flask-Testing
```

Save the above file as `environment.yml` in your project directory, and fun following command to create a new virtual environment using this file.

```
conda env create -f environment.yml
```

One of the core advantages of using `environment.yml` file is that you can easily share this environment with your team members and friends. 

---

*This article is part of **Python Programming, IoT, Big Data, Data Science, AI and Machine Learning Tutorials Series**, please [click here]({{ site.baseurl }}{% post_url 2019-06-02-python-programming-iot-big-data-data-science-ai-and-machine-learning-tutorials-series %}){:target="_blank"} to visit the complete list of articles and tutorials in this series.*

---

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
