---
title: 'Creating Python Packages'
teaching: 20
exercises: 4
---

:::::::::::::::::::::::::::::::::::::: questions 

- Where do I start if I want to make a Python package?
- What will I need / want in my package?
- What's considered good practice with packaging?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Create and build a basic example Python package
- Understand all the parts and decisions in making the package

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

This episode will see us creating our own Python project from scratch and installing it ready for use.
Feel free if you're feeling adventurous to create your own package content or follow along with this example of a Fibonacci counter.

## Python Package Structure

The barebones directory structure of a Python package is as follows:

```
📦 my-package/
├── 📂 src/
│   └── 📂 my_package/
│       └── 📄 my_code.py
│       └── 📄 __init__.py
└── 📄 pyproject.toml
```

where

- 📦 `my-package/` is the root directory of the project.
- 📂 `my_package/` is the package directory containing the source code.
- 📄 `pyproject.toml` is a configuration file for setting up the package, containing basic metadata.

Tools such as `uv` and `pip` use the `pyproject.toml` file to configure how the package is built, distributed, and installed.

::: callout

**NB: the difference in punctuation between `my-package` and `my_package`**

The top level folder name is the project name as it would be listed on PyPI and cannot contain underscores (but hyphens are valid), while `src/my_package` is the name of an importable package in Python and cannot contain hyphens (but underscores are valid).
This oddity is sometimes observed in the wild; for example the SciKit Learn package is installed with `pip install scikit-learn` but the actual imports are to `import sklearn`.

A simple solution to the awkwardness of having two subtly different names is to only use alphanumeric characters and is the common approach in Python.

:::

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: What is `__init__.py`?

At this point, it's worth discussing the use of the `__init__.py` file.
The `__init__.py` script is used to mark a directory as a Python package, allowing the contained modules to be imported (note; the use of double underscores in Python, often abbreviated to `dunder` lines, signal that this script should be "hidden" from users, helping distinguish this script from others).
It also contains any initialisation code for the package.

For instance, consider the times you have imported a package, such as [numpy](www.numpy.org).
The ability to write `import numpy` is enabled by the modular structuring of the numpy package, including the `__init__.py` file.
The complete `import numpy` statement then means Python searches for the `numpy` package  in its search path (`sys.path`) and loads its contents into the namespace under the name `numpy`.
Packages that follow the folder structure above are often referred to as **regular packages**.

However, in Python versions >= 3.3, the concept of **implicit namespace packages** (see [PEP 420](https://peps.Python.org/pep-0420/)) was introduced.
Namespace packages are commonly used to split a regular Python package (as described above) across multiple directories, which ultimately means the `__init__.py` file is technically not required to create any Python package.
For the purposes of this course, we will use an `__init__.py` to keep with convention and avoid complications with namespace packages.

::::::::::::::::::::::::::::::::::::::::::::::::

::: challenge

### What other files and content go into a package?
Think back to the earlier episodes and try to recall all the things that can go into a package.

::: solution

- Other metadata files - e.g. LICENCE, README.md, citation.cff
- `tests` - A directory full of test (unit, integration, etc...)
- Extended documentation
- Example data or other resources

:::

:::

In this episode we will only be creating a minimal example so many of the files you have thought of won't be included.
Next we will be creating our directory structure.

In either your `documents` folder if you are on Windows or your `home` directory if you are on macOS or Linux, create a folder called `fibonnaci-uoy-<name>` where `<name>` is either your University username or a random string if you don't want your username to be displayed publicly on the web (when we publish our packages to Test PyPi at the end of the session).
In the reminder of this episode the placeholder `abc123` will be used to represent your username/random string.
Populate the newly created directory with the following sub-folders and empty files.

```
📦 fibonacci-uoy-abc123/
├── 📂 src/
│   └── 📂 fibonacci_uoy_abc123/
│       └── 📄 sequence.py
│       └── 📄 __init__.py
├── 📄 pyproject.toml
└── 📄 README.md
```
    
::::::::::::::::::::::::::::::::::::: spoiler

### Optional: Using `uv` to create package skeleton

The [Reproducible Computational Environments](https://researchcodingclub.github.io/course/#reproducible-computational-environments) introduced the [uv](https://docs.astral.sh/uv/) package and project manager.
One of its many features is the ability to create a package skeleton with the command below, which will create a directory called `fibonacci-uoy-abc123` in the current working directory.
In addition to creating the required file structure, it will also populate the `pyproject.toml` with basic metadata.

```
uv init --package fibonacci-uoy-abc123
```

:::::::::::::::::::::::::::::::::::::::::::::

## Configuration File

The first thing we will do in this project is look at the metadata, stored in `pyproject.toml`.
`.toml` files have sections (termed 'tables') denoted by `[<title>]` lines.
In a `pyproject.toml` file there are 2 tables required at minimum: `[build-system]` and `[project]`.
Take a look at the minimum example `pyproject.toml` below (this is what is populated by `uv`, along with a `project.scripts` table not shown here).

```toml
[project]
name = "fibonacci-uoy-abc123"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "YOUR NAME", email = "YOUR_EMAIL@DOMAIN }
]
dependencies = []

[build-system]
requires = ["TODO"]
build-backend = "TODO"
```

### [project]
The `[project]` table is where your package's core metadata is declared.
If you used `uv` to create your skeleton then the author information may have been automatically populated from your `git` settings.
Any dependencies used by your package must be declared in the `dependencies` list, for example `numpy` or `pandas`.


### [build-system]
The `[build-system]` table specifies information required to build your project directory into a package, both the name of the build tool (`requires`) and the command it needs to run (`build-backend`).
There are multiple popular [build tools](https://packaging.python.org/en/latest/guides/tool-recommendations/#build-backends) that can be used to build your project, in this tutorial we will use `uv`, as it is simple and very popular and fits in neatly to a `uv` managed project.

::: callout
### pyproject.toml documentation

The full list of accepted keys can be found [here](https://packaging.python.org/en/latest/specifications/pyproject-toml/) in the documentation
:::

::: challenge
### Create your configuration file

Populate your `pyproject.toml` file with the two required tables


::: solution

```toml
[project]
name = "fibonacci-uoy-abc123"
version = "0.1.0"
description = "A package which can produce the Fibonacci sequence"
readme = "README.md"
authors = [
    { name = "Your Name", email = "youremail@email.com" }
]
dependencies = []

[build-system]
requires = ["uv_build"]
build-backend = "uv_build"
```
:::
:::

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: What does 'building' a Python package mean?

Building a Python package means converting the raw source code into a wheel (`.whl`) that is ready to be installed.
If any extensions are present (such as C++) they will be compiled as well and bundled into the wheel.
However, even for pure Python packages there are still several differences between the raw source code and the wheel: namely that the wheel is stripped of all metadata beyond that needed to install the package and that the code is reorganized into a standardized folder structure that can be placed directly into a user's library.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: What is `project.scripts`?

If you used `uv` to create the package skeleton you might have noticed the `[project.scripts]` table in `pyproject.toml` as follows.
This section is used to create new entry-points into your package so that certain functions can be run from the command line easily.
The example below means that running `fibonacci-uoy-abc123` on the command line will run the `main` function from this package, rather than having to type `python -c "import fibonacci_uoy_abc123; fibonacci_uoy_abc123.main()"`.
This is particularly useful when your package provides a tool for others to use, instead of (or in addition to) library functions to be imported.

```toml
[project.scripts]
fibonacci-uoy-abc123 = "fibonacci_uoy_abc123:main"
```


::::::::::::::::::::::::::::::::::::::::::::::::

## Creating Python modules

The next step in this episode is to finally write some Python code!
`.py` files are termed 'modules' in the context of a package and they are stored in `src/<package>`.

This example package will allow a user to find any value from the Fibonacci sequence.
The Fibonacci sequence is a series of whole numbers where each number is the sum of the two previous numbers.
The first 8 numbers of the sequence are `0, 1, 1, 2, 3, 5, 8, 13`.

A Python implementation of an algorithm to return the Fibonacci sequence for a specified number of terms is shown below.
Add this into the `sequence.py` module that you created earlier.

```python
def compute(n_terms):
  current_num = 0
  next_num = 1

  for i in range(n_terms):
    print(current_num)
    prev_num = current_num
    current_num = next_num
    next_num = prev_num + current_num
```

::: callout
### Reinventing the wheel
It is good to ask yourself if the package or features you are designing have been done before. 
Obviously we have chosen a simple function as the focus of this episode is on packaging code rather than developing novel code.

:::


::: challenge
### Using your Python module

Create a script in your project directory that imports and uses your sequence script. This will serve as a good quick test that it works.

::: solution
1. Create the file in the project folder `fibonacci-uoy-abc123`, for example `use_fibonacci.py`.
2. Import and run the compute function:
```python
from fibonacci_uoy_abc123.sequence import compute

compute(5)
```
:::
:::

If you try running `python use_fibonacci.py` at first it will fail with `ModuleNotFoundError: No module named 'fibonacci_uoy_abc123'` - this is because it isn't installed!
Running `python -m pip install .` from the same working directory as the `pyproject.toml` will install your package, thereby fixing this error.
Alternatively, if you're using `uv` you can simply run `uv run python use_fibonacci.py`, which will automatically install the current working version of the package into a virtual environment before running the script.

::: callout
### Editable Install
When installing your own package locally with `pip`, there is an option called editable or `-e` for short.
`python -m pip install -e .`

With a default installation (without `-e`), any changes to your source package will only appear in your Python environment when your package is rebuilt and reinstalled.
The editable option allows for quick development of a package by removing that need to be reinstalled, for this reason it is sometimes called development mode!

:::

## Adding dependencies

So far our package is entirely self-contained and doesn't require any other libraries.
However, this isn't a very realistic scenario as very few packages are written entirely from scratch without building on other libraries.

We'll now look at an example of how dependencies are added during the package development process.
To do so, we'll extend the iterative Fibonacci generation function by using [Binet's Formula](https://en.wikipedia.org/wiki/Fibonacci_sequence#Binet's_formula) to vectorize the process.
Vectorization means mathematical operations are performed on an entire list of numbers at once in a single step (by delegating to fast numerical libraries written in C/C++) rather than looping through each iteration.
As well as being faster than iterative approaches, vectorization methods also scale well with the number of iterations.
Python doesn't support vectorized functions on its in-built list data structure, so instead we'll use `numpy` and its array datatype, which does support vectorized operations.

The Python function below will calculate Fibonacci's sequence using Binet's formula.

```python
import numpy as np

def compute_numpy(n_terms):
    # Create an array of indices from 0 to n_terms-1
    n = np.arange(n_terms)
    
    # Define the Golden Ratio components
    phi = (1 + np.sqrt(5)) / 2
    psi = (1 - np.sqrt(5)) / 2
    
    # Apply Binet's Formula across the entire array at once
    # F(n) = (phi^n - psi^n) / sqrt(5)
    fib_sequence = (phi**n - psi**n) / np.sqrt(5)
    
    # Round to the nearest integer and convert to int
    fib_sequence = np.rint(fib_sequence).astype(np.int32)
    
    # Print the results
    for x in fib_sequence:
        print(x)
```

::: challenge
### Add the vectorized function to the package

Try adding the `compute_numpy` function to the package and ensure that you can run it.
Think about what additions to the modules you'll need to make, as well as the package metadata.

::: solution
1. Add the `compute_numpy` function to `sequence.py`
2. Import it and run it in `use_fibonacci.py`
3. Add `numpy` to the `dependencies` list in `pyproject.toml`
4. Reinstall the package in editable mode

```toml
dependencies = [
    "numpy>=2.4.6",
]
```
:::
:::

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: adding dependencies with `uv`

If you're using `uv` to manage your package you can simply run `uv add numpy` which will add `numpy` to `dependencies` in `pyproject.toml` and then install it into the project's virtual environment.
Subsequent `uv run python use_fibonacci.py` calls will correctly import `numpy`.

::::::::::::::::::::::::::::::::::::::::::::::::

## What Python packaging file formats and tools exist?

While reading about Python packaging, you will likely stumble across a massive alphabet soup of tools, file formats, and historical terms:

- Ancient History: `distutils`, `setup.py`, `eggs`
- The Transition Era: `setuptools`, `requirements.txt`, `setup.cfg`
- Modern Standards: wheels, `pyproject.toml`, Poetry, `uv`

Fortunately, the Python community has largely settled on `pyproject.toml` as the modern, unified standard.
You don't need to master all of these historical tools to build a package today.
However, understanding how we got here will make the current ecosystem make a lot more sense!

### distutils, setup.py, and setuptools

The first standard tool for installing packages was `distutils` (short for distribution utilities), which debuted in the late 1990s.
It relied on a `setup.py` script to configure and install packages.

However, storing a package's configuration inside an executable Python file presented some serious problems:

- Security Risks: Because `setup.py` is actual Python code, running `pip install` meant executing arbitrary code on your machine. A malicious package could easily hide malware inside its setup script.
- Boilerplate & Clutter: Every package required writing repetitive, messy code just to define basic things like the package name and version.
- The "Chicken-and-Egg" Problem: To read a `setup.py` file, you need to run Python. But if that setup.py file required a specific helper library to run, you couldn't install the helper library without running the file first.

Because `distutils` was very basic and slow to evolve, a third-party project called `setuptools` was created to supersede it.
`setuptools` added massive improvements, such as the ability to automatically find packages inside your code, declare dependencies, and introduced the first true Python package format: Eggs.

### eggs and wheels

Before you can upload your code to the Python Package Index (PyPI) for others to use, it needs to be bundled into a single file.
Eggs (`.egg`) were introduced by `setuptools` as Python's first standard package format.
While a massive leap forward at the time, they had severe limitations:

- No Standardized Metadata: Eggs didn't have a universally agreed-upon internal structure, making it hard for other tools to interact with them.
- Installation Quirks: They were often treated as zipped files added directly to your Python path, which caused bizarre import bugs and made uninstalling them incredibly messy.
- Platform Issues: They didn't handle compiled code (like C extensions) cleanly across different operating systems.

To fix this, the community created the Wheel (`.whl`) format in 2012.
Wheels completely superseded Eggs.
A Wheel is essentially a highly standardized, pre-compiled ZIP file.
Because all the heavy lifting is done before you download it, `pip` can install a Wheel almost instantly by simply unzipping it directly into your environment.

### requirements.txt

A `requirements.txt` is a text file where each line represents a package or library that your project depends on.
A package managing tool like `pip` can use this file to install all the necessary dependencies. 

```
requests==2.26.0
numpy>=1.21.0
matplotlib<4.0
```

While requirements.txt is incredibly common, it is not a packaging tool. 
`requirements.txt` is meant for **deployments** (e.g., telling other researchers exactly what specific versions of packages to download so the application runs identically on their machine).
Packaging tools (like `pyproject.toml`) are meant for **distribution** (e.g., telling the world what abstract dependencies your library needs so it can be safely installed alongside other software).

### Third-party tools

Over the years, a plethora of third-party tools emerged to plug the gaps left by Python's built-in utilities.
Managing a project required juggling separate tools for dependency resolution, virtual environments, and publishing.

- pyvenv & virtualenv: Early tools dedicated entirely to creating isolated environments so different projects wouldn't break each other's dependencies. (`pyvenv` was later deprecated in favor of Python's built-in `venv` module).
- Poetry: One of the most successful all-in-one modern tools. Poetry revolutionized Python by combining dependency management, environment isolation, and package building into a single tool using a `pyproject.toml` file.

Today, while Poetry remains highly popular, the ecosystem is shifting toward ultra-fast, next-generation tools like `uv`, which handles environments, syncing, and building at lightning speeds while strictly respecting modern packaging standards.

### Pyproject.toml

Introduced in [PEP517](https://peps.python.org/pep-0517/), the latest file for packaging a python project is the `pyproject.toml` file.
Like a `.cfg` file, a `toml` file is designed to be easy to read and declarative.
It is the current recommended way to package your Python 

::: callout
TOML stands for Tom's Obvious Minimal Language!
:::

When originally introduced, `pyproject.toml` was only designed to solve the "chicken-and-egg" problem by declaring exactly which build system `pip` should download to build your package.
A bare minimum `pyproject.toml` looked like this.

```toml
[build-system]
# Minimum requirements for the build system to execute.
requires = ["setuptools", "wheel"]
```

At first, your project's actual metadata (like its name, version, and author) still had to live in a secondary file like setup.cfg or setup.py.

With the introduction of [PEP621](https://peps.python.org/pep-0621/) in 2020, project metadata could also be stored in the `pyproject.toml` files, meaning you only now need the single file to specify all the build requirements and metadata required for your package!
This is still the preferred way in the community.

By moving to `pyproject.toml`, Python packaging has finally aligned with other modern languages (like Rust's `Cargo.toml` or Node's `package.json`), giving beginners a safe, clean, and unified way to manage their code.


::::::::::::::::::::::::::::::::::::: keypoints 

- A package can be built with as little as 3 files: a metadata file, a Python script, and an `__init__.py` file
- pyproject.toml files have 2 key tables, [build-system] and [project]
- Editable installs allow for quick and easy package development
- There are multiple standards out there for Python packaging, but pyproject.toml is the current recommended way.
- `uv` streamlines the package development process over using inbuilt Python tooling

::::::::::::::::::::::::::::::::::::::::::::::::

