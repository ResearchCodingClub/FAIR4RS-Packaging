---
title: Setup
---
## Summary

Packaging your software is one of the most important steps in a software project to make it both findable and accessible.
This course will provide you with an understanding of why and when packaging is useful, how to structure packages, and take you through each step of the packaging process.
Examples will be provided in both Python and R.

## Software Setup

### Python

Python can be downloaded from the [official website](https://www.python.org/downloads/), versions 3.11 or newer are recommended. 
`uv` will be used to manage dependencies and create the package skeleton as it consolidates a lot of functionality that is distributed amongst multiple first party packages.
Installation instructions are available on the [website](https://docs.astral.sh/uv/getting-started/installation/).

### R

RStudio can be installed from [Posit's website](https://posit.co/download/rstudio-desktop), which also has links to install R.
Some additional tools will be needed for Windows and Mac users to build packages: Windows users will need to install [Rtools for Windows users](https://cran.r-project.org/bin/windows/Rtools/), while Mac users will need to setup [Xcode](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) then run installed the Command Line Tools via Preferences->Downloads.

### Terminal

A terminal interface will also be required, listed below are the recommended ones for each OS. 
Although terminal commands aren't an explicit part of the lesson if you are unfamiliar with them guidance will be available during the session. 
This [carpentries lesson](https://swcarpentry.github.io/shell-novice/) is also a good starting place to learn.

:::::::::::::::: spoiler
### Windows
Use Command Prompt or PowerShell.

- Located in Start Menu, or by typing "cmd" or "powershell" in the search bar.
- Basic commands: `dir` (list files), `cd` (change directory), `cls` (clear screen).
- To confirm if Python is available, type `python --version` in Command Prompt or PowerShell. 
- If Python is correctly installed, you should see the correct version number printed in your terminal.
::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

Use Terminal.app.

- Located in Applications → Utilities → Terminal, or open using `Cmd+Space` and type "Terminal".
- Basic commands: `ls` (list files), `cd` (change directory), `clear` (clear screen).
- To confirm if Python is available, type `python3 --version` in your terminal.
- On some macOS installations `python` may refer to Python 2, so you can use `python3` to be specific. Commands throughout the course will use `python`, but please use `python3` if you need to. If it's correctly installed, you should see the correct version number printed in your terminal.

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

Use Terminal

- Open with `Ctrl+Alt+T` or find it in application menu (often called "Terminal", or GNOME Terminal").
- Basic commands: `ls` (list files), `cd` (change directory), `clear` (clear screen).
- To confirm if Python is available, type `python3 --version` in your terminal.
- On some distributions, `python` may refer to Python 2, so you can use `python3` to be specific. Commands throughout the course will use `python`, but please use `python3` if you need to. If it's correctly installed, you should see the correct version number printed in your terminal.

::::::::::::::::::::::::

