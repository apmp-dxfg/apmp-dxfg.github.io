---
title: PDF/A-3 Documents
last_updated: May 18, 2024
summary: "The First Report Page"
permalink: dxfg-pdfa-tut-title-style.html
toc: true
mathjax: true
layout: page
---
{% include important.html content="This page is currently being written and has not yet been reviewed" %}

## Title Page Style 
### Artwork
The first page of calibration reports usually contains logo images and often too some text that never changes. 
The design of this cover artwork is often handled by someone with computer graphics skills. 
Details about the report itself (title, date, etc.) are printed on the first page with this artwork in the background. 

For this example, we created artwork for a title page using [LibreOffice Writer](https://www.libreoffice.org/). 
Our background <a href="supplied\LMI_cover.pdf" target="_blank">artwork (opens a new tab)</a> was exported from LibreWriter in PDF/A format.  

### Page Style
A LaTeX file that places details about a calibration report over background artwork is shown below. 

{% highlight latex %}
{% raw %}
\DocumentMetadata{
    pdfversion=1.7,
    pdfstandard=A-3b,
}
\documentclass[11pt,a4paper]{article}

% Set the paper size to A4 with 25 cm margins on all sides
\usepackage[
    a4paper,
    headheight=30pt,
    margin=25mm]{geometry}

%% These two packages allow the PDF artwork to be loaded
%% in the background for the title page
\usepackage{pdfpages}
\usepackage{eso-pic}

%% These three packages together set up STIX fonts alongside Helvetica
\usepackage[T1]{fontenc}
\usepackage[scaled]{helvet}
\usepackage{stix}           

\usepackage{xspace}

%% Define variables for the report title and number
\makeatletter
\newcommand{\ReportNumber}[1]{\renewcommand{\ReportNumber}{#1}}
\newcommand{\ReportTitle}[1]{\renewcommand{\ReportTitle}{#1}}
\newcommand{\ReportReference}{Report No.\xspace \ReportNumber, \@date\xspace}
\newcommand{\Metrologist}[1]{\renewcommand{\Metrologist}{#1}}
\newcommand{\ChiefMetrologist}[1]{\renewcommand{\ChiefMetrologist}{#1}}
\makeatother

%% Paragraph formatting
\usepackage{parskip}         

%% This package finds the number of pages in a document.
%% The complier must be run twice to find the LastPage number.
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}

%% This command composes shows who has been involved in the report. 
\newcommand{\MetrologyTeam}{
           \Metrologist \\
    \textbf{Authorised by:} \> \ChiefMetrologist
}

%% A command to produce the title page
\newcommand{\maketitlepage}{%

    % Suppress header and footer 
    \thispagestyle{empty}    

    % Page 1 of N is written at the top left
    \thispageof
    
    % A vertical space is inserted before writing
    % the report title in a large font size 
    \vspace*{50mm}
    \textbf{\huge \ReportTitle}\\

    \vspace{\baselineskip}
    \textbf{\LARGE \ReportReference}
    
    \vspace{\baselineskip}
    
    \begin{minipage}{\textwidth}
    \begin{tabbing}
        \textbf{Prepared by:}\hspace{5mm} \= 
        \MetrologyTeam
    \end{tabbing}   
    \end{minipage}

    \vfill
    
    % Load the background image
    \AddToShipoutPictureBG*{%
        \includegraphics[scale=1]{LMI_cover.pdf}
    }
    % These commands must come after everything else.
    % They insert the background image and signal that 
    % the page is completed. 
    \ClearShipoutPicture
    \clearpage
}

%% 
\Metrologist{Luke Skywalker}
\ChiefMetrologist{Princess Leia Organa}
\ReportNumber{12345} % Usage example
\ReportTitle{A report on the calibration of an type-N male open}
\date{17 May 2025}

\begin{document}
\maketitlepage

\end{document}
{% endraw %}
{% endhighlight %}
Note, the body of this LaTeX document consists of just one command: `\maketitlepage`. 
Everything before `\begin{document}` is called the LaTeX 'preamble' and is where commands and macros are defined, and public LaTeX packages are imported to provide particular functions and features. 
The preamble of this file is where we configure the appearance of the title page.

#### Page dimensions
Public LaTeX packages[^1] are imported with the `\usepackage` command. Package options may be specified as keywords or key-value pairs. For example,
```tex
\usepackage[
    a4paper,
    headheight=30pt,
    margin=25mm]{geometry}
``` 
This imports the `geometry` package with options that specify a particular headheight, and the margins on all sides. 

#### Handling background
The next packages are needed to manage the background artwork
```tex
\usepackage{pdfpages}
\usepackage{eso-pic}
```

#### Fonts
LaTeX fonts work in a different way from desktop word processing applications. 
The next packages work together to provide the Helvetica font family, with the STIX package giving high quality math symbols. 
```tex
\usepackage[T1]{fontenc}
\usepackage[scaled]{helvet}
\usepackage{stix}           
```  

#### Report detail macros
We have already seen a collection of macro commands that collect information about a report. These are defined by
```tex
\usepackage{xspace}

%% Define variables for the report title and number
\makeatletter
\newcommand{\ReportNumber}[1]{\renewcommand{\ReportNumber}{#1}}
\newcommand{\ReportTitle}[1]{\renewcommand{\ReportTitle}{#1}}
\newcommand{\ReportReference}{Report No.\xspace \ReportNumber, \@date\xspace}
\newcommand{\Metrologist}[1]{\renewcommand{\Metrologist}{#1}}
\newcommand{\ChiefMetrologist}[1]{\renewcommand{\ChiefMetrologist}{#1}}
\makeatother
```
The `\makeatletter` and `\makeatother` are needed because we are defining these commands in the preamble of the source document (the `@` character is reserved). 
These commands mostly follow the same simple pattern. 
They take an argument the first time they are used, but thereafter they behave as macros that expand to provide the initial argument. 
We use these macros to fill some fields in the title page.
For example, the `\MetrologyTeam` command uses the `\Metrologist` and `\ChiefMetrologist` commands
```tex
\newcommand{\MetrologyTeam}{
           \Metrologist \\
    \textbf{Authorised by:} \> \ChiefMetrologist
}
```

### Assembling the page
The `\maketitlepage` command assembles all the different elements of the title. 
{% highlight tex %}
{% raw %}
\newcommand{\maketitlepage}{%

    % Suppress header and footer 
    \thispagestyle{empty}    

    % Page 1 of N is written at the top left
    \thispageof
    
    % A vertical space is inserted before writing
    % the report title in a large font size 
    \vspace*{50mm}
    \textbf{\huge \ReportTitle}\\

    \vspace{\baselineskip}
    \textbf{\LARGE \ReportReference}
    
    \vspace{\baselineskip}
    
    \begin{minipage}{\textwidth}
    \begin{tabbing}
        \textbf{Prepared by:}\hspace{5mm} \= 
        \MetrologyTeam
    \end{tabbing}   
    \end{minipage}

    \vfill
    
    % Load the background image
    \AddToShipoutPictureBG*{%
        \includegraphics[scale=1]{LMI_cover.pdf}
    }
    % These commands must come after everything else.
    % They insert the background image and signal that 
    % the page is completed. 
    \ClearShipoutPicture
    \clearpage
}
{% endraw %}
{% endhighlight %}

#### Header, footer and page numbers
`\thispagestyle{empty}` sets the LaTeX style to a blank page with no automatic header, footer, or page numbering.
The next command `\thispageof` writes the page number and the total number of pages in the top left of the page body.
The `\thispageof` command is defined by
```tex
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}
``` 

#### Title
The report title is placed on the page by
```tex
\vspace*{50mm}
\textbf{\huge \ReportTitle}\\
 ```
 A vertical space of 5 cm is introduced and then `\ReportTitle` is set in the `\huge` font size (about 19 pt).
 
 #### Report number and date
 Another space is introduced before the report number and date are written in `\LARGE` font (about 16 pt) by
 ```tex
 \vspace{\baselineskip}
 \textbf{\LARGE \ReportReference}
 
 \vspace{\baselineskip}
 ```
 
 #### The people involved
 The people involved in making the report are displayed by 
 ```tex
 \begin{minipage}{\textwidth}
    \begin{tabbing}
        \textbf{Prepared by:}\hspace{5mm} \= 
        \MetrologyTeam
    \end{tabbing}   
    \end{minipage}
```

#### Loading the background
Finally, the background is loaded the page is finished (a new page is started by `\clearpage`)
```tex {% raw %}
    \vfill
    
    % Load the background image
    \AddToShipoutPictureBG*{%
        \includegraphics[scale=1]{LMI_cover.pdf}
    }
    % These commands must come after everything else.
    % They insert the background image and signal that 
    % the page is completed. 
    \ClearShipoutPicture
    \clearpage
{% endraw %} ```

[^1]: LaTeX packages are available on the Comprehensive TeX Archive Network [CTAN](https://ctan.org/). Documentation for every package can be found there. 

{% include links.html %}
