---
title: "Software Packaging Overview"
teaching: 10
exercises: 2
editor_options: 
  markdown: 
    wrap: 100
---


:::::::::::::::::::::::::::::::::::::: questions

- What is software packaging?
- How is packaging related to reproducibility and the FAIR4RS principles?
- What content compromises a software package?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Recognise the importance of software packaging to ensure reproducibility.
- Understand what the basic building blocks of a package are in general

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

One of the most challenging aspects of research is reproducibility.
This necessitates the need to ensure that both research data and research software adhere to a set of guidelines that better enable open research practices across all disciplines.
The recent adaptation of the original **FAIR** principles (Findable, Accessible, Interoperable, Reusable) means that research software can now also benefit from the same general framework as research data, whilst accounting for their inherent differences, including software versioning, dependency management, writing documentation, and choosing an appropriate license.
This is commonly referred to as the FAIR4RS (FAIR for **R**esearch **S**oftware) principles.



::::::::::::::::::::::::::::::::::::: discussion

Can you recall a time when you have used someone else's software but encountered difficulties in reproducing their results? What challenges did you face and how did you overcome them?

::::::::::::::::::::::::::::::::::::::::::::::::

Software packaging is one of the core elements of reproducible research software. 
In general, it encompasses the process of collecting and configuring software components into a format that can be easily deployed on different computing environments.

<figure style="text-align: center;">
    <img src="fig/package.png" alt="alt text for accessibility purposes" width="300"/>
    <figcaption><em>Figure 1: A software package is like a box containing all the items you need for a particular activity, neatly packed together to transport to someone else</em>.</figcaption>
</figure>

::::::::::::::::::::::::::::::::::::: callout

Think about what a `package` is in general; you typically have a box of items that you want to post to someone else in the world. But before you post it for others to use, you need to make sure the package has things like: an address label, an instruction manual, and protective material.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 1: Packaging Analogy

Using the analogy in the callout above, provide an example for each package attribute in terms of the software attribute.


:::::::::::::::::::::::: hint

## Hint

The above callout should help you think about a few different possible analogies.

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution

1. Box of items: The software itself (source code and additional resources e.g. data, images).

2. Address label: Computer readable instructions to reproduce an environment with the dependencies setup

3. Instruction manual: User documentation explaining how to use the software effectively.

4. Protective materials: Error handling routines to safeguard the software from misuse or unexpected situations, and tests to verify the code functions under a variety of conditions

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::



## Why Should You Package Your Software?

As we've touched on above, there are several benefits to packaging your software:

- **Ease of installation**: Instead of manually copying individual files and setting configurations, users can automatically install the package using a package manager.

- **Dependency management**: Automates the process of installing the dependencies to prevent version conflicts and ensures the software environment is consistently correct

- **Accessibility**: Storing completed packages on central repositories ensures they are easier to access for other researchers and contributes to the software ecosystem

- **Standardisation**: Packaging enforces a standard structure and format for software, making it easier for users and developers to understand.

- **Reproducibility**: A packaged piece of software is a snapshot of the code and its dependencies at a specific point in time, making it easier to reproduce results

- **Research impact and collaboration**: From a research impact perspective, software packaging ensures reproducibility, accessibility, and ease of dissemination to the wider research community.

If you decide that packaging your software is right for your project, there are some important questions that you, as the developer, should consider before getting started:

- **Target Users**: Who are you building this package for? Beginners, experienced users, or a specific domain? This will influence the licence you choose as well as the level of detail needed in the documentation and the complexity of dependencies you may need to include. ([Software lifecycle course materials](https://docs.google.com/presentation/d/1pAwfg1H5Ny957NRrUlNhCRizVs4AkuBooYWcL7zu6i0/edit?slide=id.g3a48315a259_0_1869#slide=id.g3a48315a259_0_1869))

- **Generalization**: Is your code generalized to the extent it can be used by others, or does it rely upon your specific machine's file paths or local input data? Will it handle data in different formats? (Software Design Course coming soon!)

- **Dependencies**: What other libraries does your package rely on to function? What about hardware dependencies? Have these been documented in a standard format? ([Reproducible environments course materials](https://docs.google.com/presentation/d/144TqQoYIj1OgbIpQ4JDZmLnx4FtMsg3WL7cvf6N914A/view))

- **Testability**: How will users test your package? You should consider including unit tests and examples to demonstrate usage and ensure your code functions as expected. ([Testing and CI course materials](https://researchcodingclub.github.io/python-testing-for-research/))

- **Documentation**: Is there a `README` detailing installation instructions and basic usage as well as internal documentation in the form of docstrings? ([Documentation course materials](https://researchcodingclub.github.io/documentation/))

- **Scalability**: As your project and software grows in size and complexity, how can you effectively handle the increased modules, dependencies and distribution requirements? 

Of course, this is not an exhaustive list, however, once you have thought about candidate solutions for these questions, you're in a good position to start packaging your software.

### Anatomy of a Software Package

The most basic directory structure of a software package looks something like:

```
📦 my_project/
├── 📂 <source>/
│   └── 📄 <code>
└── 📄 README.md
└── 📄 <metadata>
```

where

- 📄 `README.md` provides the essential information about your package including how to install and use it
- 📄 `<metadata>` is a configuration file detailing the most important metadata: package name, version, authors, and dependencies amongst others. Python's metadata file is `pyproject.toml` while R's is `DESCRIPTION`. The following sections will show examples of both of these.
- 📂 `<source>` contains the source code. The structure and naming conventions of this folder will differ between programming languages

::::::::::::::::::::::::::::::::::::: challenge

## Challenge 2: Improving your project's packaging

The directory structure of a basic package shown above is a good starting point, but it can be improved. From what you have learned so far, what other files and folders could you include in your package to provide better organisation, readability, and compatibility?


:::::::::::::::::::::::: hint

## Hint

Use the emoji folder structure above to get you started. 

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: solution

## Solution

A possible improvement could be to include the following to your package:

```
📦 my_project/
├── 📂 <source>/
│   └── 📄 <code>
├── 📂 tests/
│   └── 📄 <tests>
├── 📂 docs/
│   └── 📄 example_usage.md
├── 📂 resources/
│   └── 📄 example_data.csv
└── 📄 LICENSE.md
└── 📄 CODE_OF_CONDUCT.md
└── 📄 README.md
└── 📄 <metadata>
```

Where:

- 📄 `LICENSE.md` explicitly details licensing terms and conditions under which the package's code and associated assets are made available to others for use, modification, and distribution.
- 📄 `docs` contains additional long form documentation, either hand-written and/or generated automatically from the docstrings. Often hosted as a separate website.
- 📄 `resources` contains any additional resources, such as example data or images
- 📄 `CODE_OF_CONDUCT.md` outlines community guidelines for behaviour and standards for contributors

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

Although we have touched on the core concepts of packaging, we still need to learn about how to write the metadata and logic for building a package in both Python and R.
The next episode of this course will show example minimal package structures for both of these languages.

::::::::::::::::::::::::::::::::::::: keypoints

- Reproducibility is an integral concept in the FAIR4RS principles. Appropriate software packaging is one way to account for reproducible research software, which involves collecting and configuring software components into a format deployable across different computer systems.

- Software packaging is akin to the packaging a box for shipment. Attributes such as the software source code, installation instructions, user documentation, and test scripts all support to ensure reproducibility.

- The purpose of a software package is to install source code for execution on various systems, with considerations including target users, dependencies, testability and scalability.

::::::::::::::::::::::::::::::::::::::::::::::::

