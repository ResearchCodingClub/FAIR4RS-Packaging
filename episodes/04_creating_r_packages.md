---
title: 'Creating R Packages'
teaching: 10
exercises: 0
---


:::::::::::::::::::::::::::::::::::::: questions 

- Where do I start if I want to make an R package?
- What will I need / want in my package?
- What's considered good practice with packaging?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Create and build a basic example R package
- Understand all the parts and decisions in making the package

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

This episode will see us creating our own R package from scratch.
Feel free if you're feeling adventurous to create your own package content or follow along with this example of a Fibonacci counter.

::: callout

We will be using a couple of R packages to assist with boiler-plate code generation so ensure that these are installed:

  - `devtools`
  - `usethis`
  
:::

## R Package Structure

The most basic directory structure of an R package is as follows:

```
📦 myproject/
├── 📂 R/
│   └── 📄 my_code.R
├── 📄 DESCRIPTION
└── 📄 NAMESPACE
```

where

- 📦 `myproject/` is the root directory of the package.
- 📂 `R/` contains the source code (`.R` files).
- 📄 `DESCRIPTION` contains basic metadata describing the package
- 📄 `NAMESPACE` is an automatically generated file that details imports (functions from other packages used in your package) and exports (functions your package makes available to other users). It should never be manually edited.

::: challenge

### What other files and content go into a package?
Think back to the earlier episodes and try to recall all the things that can go into a package.

::: solution

- Other metadata files - e.g. LICENCE, README.md, citation.cff
- `tests` - A directory full of test (unit, integration, etc...)
- Documentation, both docstrings and long-form (termed 'vignettees' in R)
- Example data or other resources

:::

:::

Package names must contain just letters, numbers and '.' - hyphens and underscores are not permitted.
Choosing a name that succinctly describes your package in a catchy way is quite a challenge!
In this episode we'll be making a package to generate Fibonacci sequences, so we'll use the package name `fibonacci`.

To create this package skeleton we'll use the `usethis` package, which in addition to creating the folder structure will also populate the `DESCRIPTION` with some basic metadata.
To do this, run the command below from within RStudio:

This will do several things:

  - Creates directory `fibonacci` at location `/path/to`, so be sure to set the path to somewhere sensible such as your Documents or Home folder
  - Creates folder for R scripts
  - Creates DESCRIPTION and populates it
  - Creates an R project
  - Opens the project in a new RStudio window

```
usethis::create_package('/path/to/fibonacci')
```

Once you are working in the new project take a look at the Files tab to see what has been created:

```
📦 fibonacci/
├── 📂 R/
│   └── 📄 my_code.R
├── 📄 .gitignore
├── 📄 .Rbuildignore
├── 📄 fibonacci.Rproj
├── 📄 DESCRIPTION
└── 📄 NAMESPACE
```

The additional files beyond the bare minimum detailed before are:

- 📄 `fibonacci.Rproj` defines the folder as an R package, used by RStudio to set working directories and allow for easier switching between projects
- 📄 `.gitignore` removes the `.Rproj.user` file from git (NB: a git repository hasn't been initialised)
- 📄 `.Rbuildignore` removes the `.Rproj` and `.Rproj.user` files from the R package build process (more details later)


::::::::::::::::::::::::::::::::::::: spoiler

### Optional: Using `usethis`

`usethis` does nothing that you cannot do by hand - it simply creates folders and files in the right place.
It's particularly useful for generating skeleton package directories which can be tedious otherwise, and ensuring that everything is laid out correctly.

While it's a very useful tool it's still important to understand what it is doing - fortunately `usethis` is very verbose and explains every step it has taken.

::::::::::::::::::::::::::::::::::::::::::::::::

### `DESCRIPTION`

We'll next turn our attention to the `DESCRIPTION` metadata file.
This should look like the output below.
It contains fields for basic information such as the name, a description, version number (more on versioning in a later episode), a longer description, and a licence.
You don't need to worry about the last three fields - they are related to how the documentation is generated and the defaults will suffice.

```
Package: fibonacci
Title: What the Package Does (One Line, Title Case)
Version: 0.0.0.9000
Authors@R: 
    person("First", "Last", , "first.last@example.com", role = c("aut", "cre"))
Description: What the package does (one paragraph).
License: `use_mit_license()`, `use_gpl3_license()` or friends to pick a
    license
Encoding: UTF-8
Roxygen: list(markdown = TRUE)
RoxygenNote: 8.0.0
```

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: Choosing a licence

There are a large number of different software licences, differentiated by how you want others to be able to use and extend your code.

The most permissive licence is the MIT licence and is a sensible default if you don't want to place any restrictions on the use of your software.
[choosealicense.com](https://choosealicense.com/) can help identify a more suitable licence if you don't want it fully permissive.

Once you have chosen a licence, you can add it to the package with one of the `usethis` helpers; e.g. `usethis::use_mit_license()`

NB: in British English the more common spelling is 'licence' with a second 'c', but in US English it is spelled 'license' and is the more common spelling found in documentation.

::::::::::::::::::::::::::::::::::::::::::::::::

::: challenge

### Populate `DESCRIPTION`

Enter appropriate values for the Title, Description, and Author.

:::


## Adding code

The example application for this episode is the same as in the Python episode: generating values from a Fibonacci sequence of a specified length.
An R implementation of the iterative function is shown below.

```r
compute <- function(n_terms) {
    current_num <- 0
    next_num <- 1

    for (i in seq(n_terms)) {
        print(current_num)
        prev_num <- current_num
        current_num <- next_num
        next_num <- prev_num + current_num
    }
}
```

To add this this to the project we can either manually save it into a file in the `R/` directory or run `usethis::use_r("sequence")` which will create the file `R/sequence.R` and open it in RStudio, saving you a few clicks.

To load the latest development version of the package into the environment we run `devtools::load_all()`.
This doesn't install the package into your main R installation - instead it just makes the functions available in your current session.
It needs to be run each time a file in `R/` is modified.

Then can run from REPL with just `compute(5)` and verify works as expected

::: challenge

### Confirm that the code runs

Ensure that the `compute` function can be run and gives the expected output.

::: solution

```r
> devtools::load_all()
> compute(5)
[1] 0
[1] 1
[1] 1
[1] 2
[1] 3
```

:::

:::

## Documenting functions

As discussed in a [previous lesson](https://researchcodingclub.github.io/documentation/docstrings.html), docstrings are a way of providing a summary of a function to users.
They detail the purpose of the function, describe every parameter and the return value, show example usage, and provide any other information that will help users.
In R the docstrings can be viewed using the `?` operator, e.g. `?lm` will show the documentation page for the `lm` standard library function.

In this section we will annotate our Fibonacci function to generate a similar help page.
To do so, in RStudio ensure that `sequence.R` is open in the editor and place the cursor in the `compute` function.
Then select Code -> Insert Roxygen Skeleton from the top bar.
This will insert formatted comments above the function's code.


::: challenge

### Validating the package

Running `devtools::check()` provides a way of ensuring that our package is correctly formatted and adheres to the standards to be uploaded onto [CRAN](https://cran.r-project.org/).
Try running it now and see if your package passes the tests.
You should have at least 1 warning resulting from the unpopulated docstrings.
Try to get to 0 errors, 0 warnings, and 0 notes.

NB: you can safely can ignore warnings about `unable to verify current time`, these are resulting from the check getting the current time from an website that occasionally isn't reachable.

::: solution

You might need to add a licence if you haven't already with `usethis::use_mit_licence()` or similar.

Here is an example of the populated docstrings, which can be viewed in the Help pane with `?compute`.

```r
#' Generates values from the Fibonacci sequence.
#'
#' @param n_terms The number of terms to generate.
#'
#' @returns Doesn't return anything but prints the values instead.
#' @export
#'
#' @examples
#' compute(5)
```

:::

:::


## Adding dependencies

Most packages depend on other R packages rather than being entirely self-contained.
This section will demonstrate how to bring in existing R packages and use their functions.

In the Python example we used `numpy` to turn the iterative sequence generation into a vectorized version.
However, most mathematical operators and functions in base R support vectorized functionality so there is no need to use an external package.
Instead, we'll bring in the `dplyr` library to incorporate a `tidyverse`-style approach.

The function below shows the same implementation of [Binet's Formula](https://en.wikipedia.org/wiki/Fibonacci_sequence#Binet's_formula) but this time within the framework of a `tidyverse` workflow using a `tibble` rather than a base R `data.frame` and the `mutate` function to add new columns.

The way in which we'd normally access the `tibble` and `mutate` functions in a data analysis script is to run `library(dplyr)`, but `library` calls are not used in package code as they modify the user's environment.
Instead, we must explicitly specify the package source using the `::` notation, i.e. `dplyr::mutate`.

```r
compute_vectorized <- function(n_terms) {
    # Create an array of indices from 0 to n_terms-1
    df <- dplyr::tibble(
        n = seq(0, n_terms-1)
    )

    # Define the Golden Ratio components
    phi <- (1 + sqrt(5)) / 2
    psi <- (1 - sqrt(5)) / 2

    # Apply Binet's Formula across the entire array at once
    # F(n) = (phi^n - psi^n) / sqrt(5)
    df <- df |>
        dplyr::mutate(
            sequence_raw = (phi**n - psi**n) / sqrt(5),
            sequence_int = as.integer(sequence_raw)
        )
    df
}
```

But `devtools::check()` should show a warning - despite the source code stating that these functions are imported from `dplyr`, the package itself needs to declare `dplyr` as a dependency.

```r
❯ checking dependencies in R code ... WARNING
  '::' or ':::' import not declared from: ‘dplyr’

❯ checking R code for possible problems ... NOTE
  compute_vectorized: no visible binding for global variable ‘n’
  compute_vectorized: no visible binding for global variable
    ‘sequence_raw’
  Undefined global functions or variables:
    n sequence_raw
```


::: challenge

### Add `dplyr` as a dependency

The `DESCRIPTION` file needs to be updated to declare `dplyr` as a package.
Unlike Python's `pyproject.toml`, which `uv` initiates with an empty dependency list, there is no obvious placeholder for where to do this.
Either research it, or have a look at the available `usethis` functions as one of them will come in handy here (type `usethis::` then press TAB in the R console to view all the functions in a package.)


::: solution

An `Imports` section needs to be added to `DESCRIPTION` as follows.
This can either be modified by hand or automatically using `usethis::use_package("dplyr")`.

```
Imports: 
    dplyr
```

:::

:::

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: Fixing no visible binding notes

The `tidyverse` functions heavily rely on so-called Non-Standard Evaluation.
This means referring to columns directly without quotation marks or explicitly referencing it as a column.
Compare these 3 ways to make a column called 'foo'.
The first 2 use standard evaluation - the R interpreter knows that `foo` is either a column because it is directly referenced as such using the `$` syntax, or it is written as a string.
However, in the third example the R interpreter just sees `foo = 1:10` and thinks that `foo` is a global variable that hasn't been declared before, hence the note.

```r
df$foo <- 1:10
df['foo'] <- 1:10
df |> mutate(foo = 1:10)
```

In practice, these notes can be safely ignored if you do not intend for your package to be uploaded to CRAN.
However, if you do intend to store it in CRAN then the check must be completely clean.
The simplest solution is to simply not use non-standard evaluation.
Non-standard evaluation makes interactive data analysis code cleaner, but it does not offer much benefit for library code which is not written very frequently.

::::::::::::::::::::::::::::::::::::::::::::::::

The `compute_vectorized` function should now be able to be loaded and run from the R console!
Verify that the results are the same as `compute` and make any suitable modifications - including adding docstrings.

::::::::::::::::::::::::::::::::::::: spoiler

### Optional: Types of dependencies

There are 3 types of dependencies in R packages.

  - Imports: Must be installed for your package to work. This is what `usethis::use_package()` defaults to and is generally the right choice
  - Suggests: Not required for the core package functionality but might be needed for development, i.e. running tests or building vignettes
  - Depends: Used to pin to a specific R version and packages. But it also loads all packages into the namespace so do not use unless absolutely necessary
  
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints 

- A package can be built with as little as 3 files: `DESCRIPTION`, `NAMESPACE`, and a source file.
- `usethis` helps generate package skeletons, add dependencies, and add source code files
- `devtools::load_all()` loads the current package allowing for quick testing without needing to install it
- `devtools::check()` validates the package structure and contents

::::::::::::::::::::::::::::::::::::::::::::::::
