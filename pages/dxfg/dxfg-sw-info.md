---
title: Open-source software tools
last_updated: May 31, 2024
summary: "Information about open source software used in connection with DXFG Resources"
permalink: dxfg-sw-info.html
toc: true
mathjax: true
layout: page
---
This page provides information about open-source software referred to elsewhere on this website. 

We offer some guidance about installing the software.
However, all of these tools have extensive user communities, who provide a wealth of supporting information.
We urge readers to search for more information online. 

The information below assumes that your operating system is Windows 11, or Windows 10 with May 2020 Update (version 2004) or later. 

## LaTeX

[LaTeX](https://www.latex-project.org/) is a free high-quality typesetting system; it includes features designed for the production of technical and scientific documentation. LaTeX is the de facto standard for production and publication of scientific documents. 

LaTeX runs on top of a typesetting system called TeX. [TeX distributions](https://www.latex-project.org/get/#tex-distributions) usually bundle the various parts needed for a working TeX system and include both configuration and maintenance utilities. 

To use the features of LaTeX developed for tagged data (in PDF files), it is important to keep your LaTeX distribution up to date.

People often use an special editor tool to work with LaTeX. However, we will give command line instructions for processing where necessary. These commands should work correctly with all LaTeX distributions. The examples presented in these resources pages were developed using [MiTeX](https://miktex.org/) for Windows.  



## Installing open-source software tools  
We use `WinGet`, the [Windows Package Manager](https://learn.microsoft.com/en-us/windows/package-manager/).
If `WinGet` is not installed, refer to [install WinGet](https://learn.microsoft.com/en-us/windows/package-manager/winget/#install-winget), or install the open-source software in another way.

The examples below show commands typed into an open terminal (i.e., PowerShell or Command Prompt).
When finished, this terminal should be closed. 
The newly installed programs will be accessible the next time a terminal is opened.

### Python 
Home page: [Python](https://www.python.org/)

To install Python (version 3.12 is shown)
```console
> winget install --exact --id Python.Python.3.12
```

#### 3rd party Python packages
Python is shipped with many packages but there are also external packages available. 
For example, we refer to the `pypdf` and `pikepdf` packages, which must be installed in addition to Python itself. 

To install these packages (for example) in a Python [virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments)
```console
> py -m venv .venv
> .venv\Scripts\activate
> pip install pypdf pikepdf
```

### LaTeX 
Home page: [MiKTeX](https://miktex.org/)

To install MiKTeX
```console
> winget install --exact --id MiKTeX.MiKTeX
```

### Git 
Home page: [Git](https://git-scm.com/)

To install Git
```console
> winget install --exact --id Git.Git
```

#### Cloning a git repository
The DXFG has a github repository in which some resources are available.

Files related to examples of producing PDF/A documents may be downloaded (this is called 'cloning' the git repository).
In the local directory where the files are to be cloned, open a terminal and type
```console
> git clone https://github.com/apmp-dxfg/pdfa3-documents.git
```


{% include links.html %}
