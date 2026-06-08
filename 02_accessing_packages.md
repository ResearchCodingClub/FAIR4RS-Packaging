---
title: 'Accessing Packages'
teaching: 30
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I access my own package?
- What are the different ways of downloading Python packages?
- What are the different ways of installing R packages?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Install packages from source in both R and Python
- Install packages using `pip`
- Install packages in R

::::::::::::::::::::::::::::::::::::::::::::::::


## Introduction

Due to the proliferation of software into many areas of academic research, it is quite likely that you won't be the first person to set off on solving any particular task.
Many others have worked on common problems and then shared their solution in the form of a package, which you can conveniently integrate into your own code and use!

::: callout

### Popular Packages

Some of the most popular packages you may have heard of are:

- Numpy (Python)
- Pandas (Python)
- tidyverse (R)
- ggplot2 (R)

:::

## Python

To use a package that is installed you use the key word `import` in Python.

```python
# This imports the pandas package and gives it a new name 'pd'.
import pandas as pd 

# Use the package to read a file
pd.read_csv("my_data.csv") 
```

### Python Package Index (PyPI)

The Python Package Index or PyPI is an online repository of Python packages hosting over 500,000 packages! While there are alternatives such as [conda-forge](https://conda-forge.org), PyPI is by far the most commonly used and likely to have all you need.

::: challenge

### Exercise 1: Explore PyPI

Explore [PyPI](https://pypi.org) to get familiar with it, try searching for packages that are relevant to your research domain / role!

:::

::: callout
### pip

pip (package installer for Python) is the standard tool for installing packages from PyPI. 
You can think of PyPI being the supermarket full of packages and pip being the delivery van bringing it to you.

:::


### Using pip

pip itself is a Python package that can be found on [PyPI](https://pypi.org). It however comes pre-installed with most Python installations, for example [python.org](https://python.org) and inside virtual environments.

The most common way to use pip is from the command line. At the top of a package page on PyPI will be the example line you need to install the package

```
python -m pip install numpy
```

The above will install [numpy](https://pypi.org/project/numpy/) from PyPI, a popular scientific computing package enabling a wide range of mathematical and scientific functions. 

::: callout
You may notice a wheel file download during the pip install, for example `Downloading numpy-2.0.0-cp312-cp312-win_amd64.whl`. A wheel in Python is a pre-built package format that allows for quicker and more efficient installation, so when it is downloaded your local computer doesn't need to do any building. The alternative is source files which often take the form `.zip` or `.tar.gz`, which when downloaded will then need to be built then installed, which is often far slower.

:::

::: challenge
### Exercise 2: Create venv and install Numpy

Step 1: Create a venv in the .venv directory using the command `python -m venv .venv` and activate it with

::: tab

### Windows 

`.\.venv\Scripts\activate`



### macOS / Linux

`source .venv/bin/activate`


:::

When activated you should see the name of your environment in brackets at the start of your terminal line

Step 2: Install Numpy into your new environment

Step 3: Check your results with `python -m pip list`

Step 4: Deactivate your environment with `deactivate`

:::

::: spoiler

### Virtual Environments

Check out [this documentation](https://docs.python.org/3/library/venv.html) or the FAIR4RS course on virtual environments to learn more!

:::

#### Installing from source 

pip can also be used to install packages from source. This means that the package file structure (source) is on your local computer and pip installs it using the instructions from the `setup.py` or `pyproject.toml` file. This is especially handy for packages either not on PyPI, like ones downloaded from github, or for your own packages you're developing.

::: tab

### Windows 

`python -m pip install .`



### macOS / Linux

`python3 -m pip install .`


:::

::: instructor

It may be worth checking with the class at this point that they are all familiar with the `-m` notation, and what the above command does exactly

:::

Here the `.` means to install your current directory as a Python package. For this to work the directory your command line interface is currently in needs to have a packaging file, i.e. `setup.py` or `pyproject.toml`. 


## R

To load a package in R the `library` function is used, which will automatically make all the exported functions from that package available in the R environment.
Contrast this behaviour with Python where you still need to write `pd.read_csv` rather than `read_csv`.

```r
# Loads the readr package and places all its exported functions into the global namespace
library(readr)

# Use the package to read a file
read_csv("my_data.csv")
```

Less commonly you can access a single function from a package using the `::` syntax, this is useful for situations where you might not want to populate your environment with all the exported functions from that library, e.g. if you know there will be a conflict.

```r
readr::read_csv("my_data.csv")
```

### The Comprehensive R Archive Network (CRAN)

R's primary package repository is called [CRAN](https://cran.r-project.org/).
It currently (at the time of writing) hosts 23,667 packages, significantly fewer than PyPI, but there are several substantial differences between these two repositories.
PyPI functions as more of a warehouse with a very low barrier to entry, hence the vast number of packages.
CRAN by contrast operates as a curated library with very strict criteria for incoming packages (as well as for any updates!).
This stark difference in philosophy results in a very different user experience.

::: challenge

### Exercise 3: Explore CRAN

Explore [CRAN](https://cran.r-project.org/) to get familiar with it, try searching for packages that are relevant to your research domain / role!
The [task views](https://cran.r-project.org/web/views/) are particularly useful for scoping out the range of existing packages in your field.
Look at the page for a package (e.g. [deSolve](https://cran.r-project.org/web/packages/deSolve/index.html)) and identify the different types of documentation available.

:::

### `install.packages`

R does not have a separate package manager like `pip` that can be run as an external tool.
The primary way of installing packages is through the `install.packages` command which is run from within R.
This function by default installs from CRAN, although it can be used to install from other repositories setup in the same fashion as CRAN.
The example below shows how to use it to install packages (NB: it will ask for a mirror if one isn't setup, the `0-Cloud` option is recommended).

```r
install.packages(c("dplyr", "ggplot2"))
```

::: callout

### Binary vs source packages in R

Like with Python, packages will be downloaded as pre-built binaries but only for Windows (`.zip`) and Mac (`.tgz`) users - on Linux machines the source package (`.tar.gz`) will be downloaded and built directly on the user's machine.
This is because CRAN does not offer pre-built Linux packages owing to the vast heterogeneity in Linux toolchains.
Python accomplishes this via the `Manylinux` standard.

:::

### Other package sources

Because the barrier to publishing a package on CRAN is so high, there exist several alternatives that have built up popularity in different fields.

#### GitHub

A far more streamlined alternative to hosting your package on CRAN is to maintain it on GitHub where users can install it from.
This fits in neatly into a version-controlled workflow (see our course on [version control](https://docs.google.com/presentation/d/1ifKyvCnR-ZcJokvKrrfUbf9n7lmIrP8XKRz9Uqyu538)) and for many use cases will be sufficient.

To install a package from GitHub the `pak` package is required which can install from GitHub using the `pkg_install` function.
This example will install the current development version of `dplyr` straight from GitHub, which will be newer than the stable release on CRAN.

```r
pak::pkg_install("tidyverse/dplyr")
```

::: callout

#### `devtools` vs `remotes` vs `pak`

You might come across documentation recommending to use `devtools` or `remotes` to install GitHub-hosted packages instead of `pak`.
`devtools::install_github` was once the recommended method, then this functionality was spun-out into the more minimal `remotes` package to keep `devtools` more streamlined on package development.
Now as of the time of writing `pak` is the current one-stop-shop for package management in R and is faster and more robust than `devtools`/`remotes` and can handle more backends than `install.packages`.

:::

#### Bioconductor

Before GitHub became so widespread as a package repository, the Bioinformatics community created their own alternative to CRAN: [Bioconductor](https://www.bioconductor.org).
This is accessed via the R package `BiocManager` or via `pak` again by using the syntax `pak::pkg_install("bioc::<package>")`.

#### Local

`pak` can also be used to install local packages from source by passing the `pkg_install` function a path to a package stored on the local filesystem.

```r
pak::pkg_install("path/to/source")
```



::: keypoints
- `pip` is the most common tool used to download and access Python packages from PyPI.
- PyPI is an online package repository which users can choose to upload their packages to for others to use.
- CRAN is R's repository which has a far higher barrier to entry than PyPI.
- `install.packages` is the gold standard way of installing packages from CRAN.
- `pak` is the modern fast package manager in R and can install from a variety of sources.
- Both `pip` and `pak` can also be used to install packages on your local system (installing from source).
:::






