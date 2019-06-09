---
layout: post
title:  "Python: Getting Started with Python Virtual Environments and pip"
author: sma
categories: [ Python ]
image: assets/images/posts/fabian-grohs-423591-unsplash.jpg
description: "Python: Getting Started with Python Virtual Environments and pip"
tags: [featured]
---

## What Is Python Virtual Environment and why we need it?

*Virtual environment* helps to create a *sandbox* by isolating dependencies required by a specific *python* project. As a result each python project can have an isolated environment / sandbox with it's own dependencies, regardless of the decencies a-viable elsewhere on the same system.

## Why Use Virtual Environment?



In this section I will list some of reasons and benefits of creating and managing multiple virtual environments for *Python* development.

1. Major operating systems like *macOS* come with pre-installed version of *Python*, however the version installed with OS might be an older one and in some cases upgrading this default installation 
my compromise the proper working on system components. 
2. With default global installation using *pip*, it's not possible to install different packages with 
3. *Virtual envionrmet* enables you to use different version of a same package for different projects.
4. By default *pip* installs the *python packages* globally, this may result in conflicts.
same name. However *virtual environment* makes it easy to install packages within the project sandbox.

## Installing Python

I prefer *Python's Anaconda distribution*, you can follow [official documentation](https://docs.anaconda.com/anaconda/install/){:target="_blank"} to install it on different platforms. AAlternatively you can get [latest official python distribution](https://www.python.org/downloads/release/python-373/){:target="_blank"} for different platforms. 

## Installing virtualenv

*virtualenv* can be installed using *pip* python package manager as following.

```
pip install virtualenv
```
Official installation documentation of *virtualenv* can be found [here](https://virtualenv.pypa.io/en/latest/installation/){:target="_blank"}.


## Creating a Virtual Environment using virtualenv

1. Create your project directory.
    ```
    mkdir myproject
    cd myproject
    ```
2. Create virtual environment using virtualenv.
    ```
    virtualenv -p python3 venv
    ```
### Notes
- `-p python3` specifies the version of python to be used for newly created virtual environment, you can skip `-p python3` option to use the default version of python installed on your system.
- `venv` is the name of directory for virtual environment.

## Activating Virtual Environment

We can use `source` command to activate a virtual environment of systems like *Linux and Mac*.

```
source venv/bin/activate
```
Use `activate.bat` to activate virtual environment on Windows OS.

```
venv\Scripts\activate.bat
```

## Installing Packages using pip

*pip* is the default package manager for python, we can install a package named *camelcase* using *pip* as following.

```
pip install camelcase
```

You can install a specific version of package as following.

```
pip install camelcase==0.2
```

## Installing multiple packages using requirements.txt

You can store list of all packages required by your project in a file named `requirements.txt` and then *pip* command can be used to install all these packages at once. Let's do it.

1. At the root of your project directory, create a new file named `requirements.txt` with following contents.
    ```
    camelcase==0.2
    decorator==4.3.0
    ```
2. Install the `requirements.txt` using *pip* command as following.
    ```
    pip install -r requirements.txt
    ```
Above command will install all two packages listed in our `requirements.txt`.


## Uninstalling Packages using pip

You can uninstall a package as following.

```
pip uninstall camelcase
```


## Searching Packages using pip

You can find python packages using *pip* as following.

```
pip search math
```

Above command will return available packages relating to keyword *math* as following.

```
math-addition (3.0)                  - Math Addition
some-math (0.0.3)                    - some math routines
animals-math (0.0.7)                 - A package for animals and their math
math-fold (0.1.5)                    - back math notaion in CLI
micropython-math (0.0.0)             - Dummy math module for MicroPython
blockdiagcontrib-math (0.9.0)        - LaTeX math plugin for blockdiag
mo-math (2.40.19027)                 - More Math! Many of the aggregates you are familiar with, but they ignore Nones
scry-math (0.5)                      - A simple SCRY service to extend SPARQL with basic math procedures
python-markdown-math (0.6)           - Math extension for Python-Markdown
django-math-captcha (0.1)            - Simple, secure math captcha for django forms
ntcir10-math-converter (0.2.2)       -  The NTCIR-10 Math Converter package converts NTCIR-10 Math XHTML dataset and relevance
                                       judgements to the NTCIR-11 Math-2, and NTCIR-12 MathIR XHTML5 format.
pelican-render-math (0.3.0)          - Pelican math rendering plugin modified to work with nice-blog theme
ntcir-math-density (0.2.1)           -  The NTCIR Math Density Estimator package uses datasets, and judgements in the NTCIR-11
                                       Math-2, and NTCIR-12 MathIR XHTML5 format to compute density, and probability estimates.
wagtail-simple-math-captcha (0.1.2)  - A simple math captcha field for Wagtail Form Pages based on Django Simple Math Captcha.
django-simple-math-captcha (1.0.8)   - An easy-to-use math field/widget captcha for Django forms.

```



## References
1. [Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)


## Programming with Python Series

1. [Understanding Python Programming Language]({{ site.baseurl }}{% post_url 2019-06-09-understanding-python-programming-language %})


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
