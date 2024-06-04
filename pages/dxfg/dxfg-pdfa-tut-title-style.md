---
title: Title Page Style Development
last_updated: May 18, 2024
summary: "LaTeX specification of the title page style for Lunar Metrology Institute reports is explained"
permalink: dxfg-pdfa-tut-title-style.html
toc: true
mathjax: true
layout: page
---

## Title Page Style

We explain how the appearance of the title page for Lunar Metrology Institute reports is created in LaTeX. 

### Artwork
The first page of calibration reports usually contains logo images and often too some text that never changes. 
The design of this cover artwork is often handled by someone with computer graphics skills. 
Details about the report itself (title, date, etc.) are printed on the first page with this artwork in the background. 

For this example, we created artwork for a title page using [LibreOffice Writer](https://www.libreoffice.org/){:target="_blank"}. 
Our background [artwork](supplied/LMI_cover.pdf){:target="_blank"} was exported from LibreWriter in PDF/A format.  
The source file is available on [github](https://github.com/apmp-dxfg/pdfa3-documents/tree/main/artwork){:target="_blank"}.

### Page Style
A complete LaTeX file that places details about a calibration report over background artwork is shown below. 
The source file and PDF artwork are available on [github](https://github.com/apmp-dxfg/pdfa3-documents/tree/main/LMI-title-dev){:target="_blank"}.

We explain the different parts of this code below.

Note, `\maketitlepage` is the only command in the body of this LaTeX document. 
Everything that comes before `\begin{document}` in a LaTeX file is called the 'preamble'. This is where commands and macros are defined, and public LaTeX packages are imported that provide particular functions and features. 
The preamble of our file is where the appearance of the title page is configured.

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

%% This command displays those involved in issuing the report. 
\newcommand{\MetrologyTeam}{
           \Metrologist \\
    \textbf{Authorised by:} \> \ChiefMetrologist
}

%% Paragraph formatting
\usepackage{parskip}         

%% This package finds the number of pages in a document.
%% The compilier must be run twice to find the LastPage number.
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}

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

#### Page dimensions
Public LaTeX packages[^1] are imported using the `\usepackage` command. 
Package options may be specified as keywords or key-value pairs. 
For example, the following lines import the `geometry` package and specify options that set a particular headheight, and the margins on all sides. 
```tex
\usepackage[
    a4paper,
    headheight=30pt,
    margin=25mm]{geometry}
``` 

#### Background artwork handling 
The packages needed to manage the background artwork are import next
```tex
\usepackage{pdfpages}
\usepackage{eso-pic}
```

#### Helvetica font
LaTeX fonts are handled in a different way from desktop word-processing applications. 
The topic of fonts in LaTeX is a complicated one. 
For the Lunar Metrology Institute style, we would like to use the Helvetica font family, which closely resemble the commercial Arial fonts.
The next packages work together to provide Helvetica in conjunction with the STIX LaTeX package that produces high quality symbols for mathematics. 
```tex
\usepackage[T1]{fontenc}
\usepackage[scaled]{helvet}
\usepackage{stix}           
```  

#### Report detail macros
We have already seen a collection of macro commands that collect information about a report. 
These are defined by
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
The `\makeatletter` and `\makeatother` are needed because the `@` character is reserved in LaTeX source file. 
We need prevent LaTeX from complaining when we access the internally defined macro `\@date`.

Several of these commands shown follow a simple pattern. 
The first time they are used they take an argument but thereafter they expand to provide this argument. 
We use these macros to fill some fields in the title page.
For example, the `\MetrologyTeam` command uses the values in `\Metrologist` and `\ChiefMetrologist`
```tex
\newcommand{\MetrologyTeam}{
           \Metrologist \\
    \textbf{Authorised by:} \> \ChiefMetrologist
}
```

#### Page numbering
The command `\thispageof` produces text showing the page number and the total number of pages. 
```tex
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}
``` 

### Assembling the title page
The `\maketitlepage` command assembles the different elements of the title page. 
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

#### Title
A blank page style, with no header, footer, or automatic page numbering, is set by
```tex
\thispagestyle{empty}

% Page 1 of N is written at the top left
\thispageof    
```
The command `\thispageof` places the page information in the top left of the page.

The report title is placed on the page by
```tex
\vspace*{50mm}
\textbf{\huge \ReportTitle}\\
```

A vertical space of 5 cm is introduced and then `\ReportTitle` is set in the `\huge` font size (about 19 pt).

#### Report number and date
The report number and date are written one line below the title, in `\LARGE` font (about 16 pt), 
```tex
\vspace{\baselineskip}
\textbf{\LARGE \ReportReference}
 
\vspace{\baselineskip}
```
 
#### The people involved
The key people involved in issuing the report are displayed by 
```tex
\begin{minipage}{\textwidth}
\begin{tabbing}
    \textbf{Prepared by:}\hspace{5mm} \= 
    \MetrologyTeam
\end{tabbing}   
\end{minipage}
```

#### Loading the background
Finally, the background artwork is loaded and the page is finalised (a new page is started by `\clearpage`)
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

[^1]: LaTeX packages are available on the Comprehensive TeX Archive Network [CTAN](https://ctan.org/){:target="_blank"}. Documentation for every package can be found there. 

{% include links.html %}
