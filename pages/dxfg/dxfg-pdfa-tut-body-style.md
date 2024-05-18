---
title: Report Body Style
last_updated: May 18, 2024
summary: "LaTeX specification of the style used for the body of Lunar Metrology Institute reports is explained"
permalink: dxfg-pdfa-tut-body-style.html
toc: true
mathjax: true
layout: page
---
{% include important.html content="This page is currently being written and has not yet been reviewed" %}

## Report Body Style

We explain how the appearance of material in the body of Lunar Metrology Institute reports is controlled in LaTeX. 

### Page style in the preamble 

Here is a LaTeX file with commands that specify appearance and some content, which will be presented in the report.
Appearance is specified in the preamble of a LaTeX file (before `\begin{document}`).
The report content itself is between `\begin{document}` and `\end{document}`.
A title page is not produced.
Some of the configuration commands have already been discussed when considering the [title page](dxfg-pdfa-tut-title-style). 
We explain the other details below.

{% highlight latex %}
{% raw %}
\DocumentMetadata{
    pdfversion=1.7,
    pdfstandard=A-3b,
}
\documentclass[11pt,a4paper]{article}

% Set the paper size and margins 
\usepackage[
    a4paper,
    headheight=2\baselineskip,
    hmargin=25mm,
    vmargin=30mm]{geometry}

%% These three packages together set up STIX fonts 
%% alongside Helvetica
\usepackage[T1]{fontenc}
\usepackage[scaled]{helvet}
\usepackage{stix}           

\usepackage[UKenglish]{babel} % British English conventions
\usepackage[UKenglish,cleanlook]{isodate}   % Date format: day month year

%% For mathematical expressions and units
\usepackage{upgreek}
\usepackage{amsmath}
\usepackage{siunitx}

%% To make captions without using LaFeX figure or table environments 
\usepackage{capt-of}

%% For including images
\usepackage{graphicx}

%% Define variables for the report title and number
\usepackage{xspace}

\makeatletter
\newcommand{\ReportNumber}[1]{\renewcommand{\ReportNumber}{#1}}
\newcommand{\ReportTitle}[1]{\renewcommand{\ReportTitle}{#1}}
\newcommand{\ReportReference}{Report No.\xspace \ReportNumber, \@date\xspace}
\makeatother

%% This package finds the number of pages in a document.
%% The complier must be run twice to find the LastPage number.
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}

%% Fine control of line spacing 
\usepackage{parskip}        % Paragraph formatting
\usepackage{setspace}

\usepackage{hyperref}
\hypersetup{
    colorlinks=true,        % links are coloured
    urlcolor=blue,          % URLs, etc
    linkcolor=black,        % Pages, etc
    citecolor=blue
}

%% Set up a header and footer
\usepackage{fancyhdr}
\fancypagestyle{fancy}{%
    \renewcommand{\headrulewidth}{0pt}  % Supress a line ruled by fancyhdr
    \fancyhf{}
    \fancyhead[L]{\small\textnormal{\ReportReference}}
    \fancyhead[C]{\small\textbf{\LMI}\\}
    \fancyhead[R]{\small\textnormal{\thispageof}}
    \fancyfoot[C]{\small\textit{\RestrictionsLMI}}
}

% Pre-defined strings
\newcommand{\LMI}{Lunar Metrology Institute}

\newcommand{\RestrictionsLMI}{%
This report may not be reproduced, 
except in full, without the written consent of the\\
Chief Metrologist, Lunar Metrology Institute}

\newcommand{\referenceGUM}{
For information about uncertainty terminology, see: 
BIPM, IEC, IFCC, ISO, IUPAC, IUPAP and OIML, 
\textit{``Evaluation of measurement data---Guide to the expression of uncertainty in measurement''}, 
BIPM Joint Committee for Guides in Metrology, Paris, S\`evres, edition 1, JCGM~100:2008, 2008. 
A PDF version is available on-line:

\begin{center}
	\href{%
		https://www.bipm.org/documents/20126/2071204/JCGM_100_2008_E.pdf
	}{%
		https://www.bipm.org/documents/20126/2071204/JCGM\_100\_2008\_E.pdf
	}
\end{center}
}


% Fonts!!
% Arial is not in the public domain but Helvetica is very similar.
% So, we use Helvetica (phv).
% The default normal text should be 11-pt. In that case, 
%   \HUGE ~= 23 pt
%   \huge ~= 19 pt
%   \LARGE ~= 16 pt
%   \Large ~= 14 pt
%   \large ~= 12 pt
%   \normal ~= 11 pt
%   \small ~= 10 pt
%   \footnotesize ~= 9 pt
%   \scriptsiize ~= 8.5 pt
%   \tiny ~= 7 pt
%
% The third parameter is the fontseries: `bc` stands for bold condensed
\newcommand{\lmiboldarialnarrow}{\usefont{T1}{phv}{bc}{n}}
\newcommand{\lmiarial}{\usefont{T1}{phv}{m}{n}}
\newcommand{\lmiboldarialsnarrowlanted}{\usefont{T1}{phv}{bc}{sl}}

%% Define the layout of section headings
\usepackage{titlesec}
\titleformat{\section}
  {\lmiboldarialnarrow\Large}{}{0pt}{}
\titlespacing*{\section}
  {0pt}{\baselineskip}{0.5\baselineskip} 

\titleformat{\subsection}
    {\lmiboldarialnarrow\large}{}{0.5em}{}
\titlespacing*{\subsection}
    {0pt}{\baselineskip}{0pt}

\titleformat{\subsubsection}
    {\lmiboldarialsnarrowlanted\normalsize}{}{0.5em}{}
\titlespacing*{\subsubsection}
    {0pt}{\baselineskip}{0pt}

%% For embedded files
\usepackage{embedfile}
\embedfilesetup{     
      filesystem=URL,
      afrelationship={/Data},
      stringmethod=escape
}

%% These details come before \begin{document}
\ReportNumber{12345} 
\ReportTitle{A report on the calibration of an type-N male open}
\date{17 May 2035}

%% XMP Metadata
\hypersetup{
    pdftitle={\ReportTitle},
    pdfauthor={\LMI},
    pdfnumpages={\pageref*{LastPage}}
}

\begin{document}
\pagestyle{fancy}
\raggedright        % Does not adjust the right margin
	
% Possible section headings: Description, Identification, Client
% Reference, Date(s) of Calibration (of Test), Objective
% Method, Conditions, Notes, Results, Uncertainty, Conclusion
%
\section{Description}
The components are from a USC vector network analyser calibration kit model 8599. 

\section{Identification}
The component serial number is 2221X.

\section{Client}
United Spacecraft Corporation, 51 Mare Tranquillitatis, The Moon.

\section{Date of Calibration}
The measurements were performed on the 7$^\mathrm{th}$ of February 2025.

\section{Conditions}
Ambient temperature was maintained within $\SI{\pm 1}{\celsius}$ of $\SI{-123}{\celsius}$.

\section{Method}
Measurements of the voltage reflection coefficient were made according to procedure LMIT.E.063.005. 

\section{Results}
Results in fig~\ref{fig1} are reported in polar coordinates (magnitude, $\rho$, and phase, $\phi$), using a linear scale for magnitude and units of degrees for phase. 

% Although not used here, it is also possible to have a \subsubsection{}
\subsection{Open (male), SN 54673}

 \begin{center} % Centered horizontally on the page
 
 % The report text has 1.5 line spacing, 
 % but that is too wide for tables 
 \begin{singlespace}
 
 	\small	% use a smaller font size for the table entries
 
  	% Increases the vertical spacing between rows slightly  
  	\setlength{\extrarowheight}{3pt}
  
	\[
		% the 'S' array column type will align numbers on the decimal 
        % Note 'S[group-minimum-digits=3]' or '\sisetup{group-minimum-digits=3 }'
        % would be used to force a space separator every 3 digits (this
        % does not happen by default until there more than 4 digits)
  		\begin{array}{SSSSS}
    		\multicolumn{1}{c}{ \text{frequency} } & 
    		\multicolumn{2}{c}{ \text{magnitude} } &
    		\multicolumn{2}{c}{ \text{phase} } 
    		\\
		% 2nd line 
    		\multicolumn{1}{c}{ \si{(MHz)} } &  
    		\multicolumn{2}{c}{ (\text{linear}) } &
    		\multicolumn{2}{c}{ \text{(/degree)} }  
    		\\
  		% 3rd line 
     		& {\rho} & {U(\rho)} & {\phi} & {U(\phi)} 
     		\\ \hline % Underline the headings

  		%%-----------------------------------------------
  		% Data here
		45 &   0.9998 &   0.0023$^\dagger$ &    -1.46 &     0.13     \\
		50 &   0.9998 &   0.0023$^\dagger$ &    -1.62 &     0.13     \\
		100 &   0.9999 &   0.0023$^\dagger$ &    -3.27 &     0.13    \\
		300 &   0.9998 &   0.0025 &    -9.80 &     0.14    \\
		500 &   0.9997 &   0.0026 &   -16.34 &     0.15    \\
		1000 &   1.0000 &   0.0032 &   -32.72 &     0.18   \\
		2000 &   0.9994 &   0.0054 &   -65.67 &     0.31  \\
		3000 &    1.000 &    0.011 &   -98.66 &     0.62   \\
		4000 &    0.999 &    0.013 &  -131.74 &     0.78   \\
		5000 &    0.999 &    0.016 &  -164.77 &     0.90   \\
		6000 &    0.998 &    0.017 &  +162.15 &     0.99   \\
		7000 &    0.997 &    0.018 &   +129.0 &      1.1   \\
		8000 &    0.997 &    0.018 &    +95.9 &      1.1   \\
		9000 &    0.996 &    0.018 &    +62.7 &      1.1  \\
		%%-----------------------------------------------
		
		\end{array}
	\]
	\captionof{figure}{Magnitude and phase data}
	\label{fig1}     % Must follow \captionof
	
\end{singlespace}
\end{center}


\section{Uncertainty}
A coverage factor $k=1.96$ was used to calculate the expanded uncertainties $U(\cdot)$ at a level of confidence of approximately 95\%. The number of degrees of freedom associated with each measurement result was large enough to justify this coverage factor.  

Some of the expanded uncertainty values reported fall outside LNI's current scope of accreditation. These values are indicated by a $\dagger$. The least expanded uncertainty for a magnitude measurement close to unity in the LNI scope is currently 0.0024. 

% A \paragraph is a lower heirarchy section. The 'heading' text will be in bold
% and the 'body' text will follow on the same line.
\paragraph{Note:} \referenceGUM	% Standard reference to the GUM

\embedfile[mimetype=application/vnd.openxmlformats-officedocument.spreadsheetml.sheet]{ex_data.xlsx}
\end{document}
{% endraw %}
{% endhighlight %}

#### Conventional language and maths options
A certain style of typesetting (here UK English) can be selected by importing for following packages
```tex
\usepackage[UKenglish]{babel} % British English conventions
\usepackage[UKenglish,cleanlook]{isodate}   % Date format: day month year
```

We also prefer to use standardised American Mathematical Society maths features, have upright Greek letters available, and SI units, all of which is provided by these packages:[^1]
```text
\usepackage{upgreek}
\usepackage{amsmath}
\usepackage{siunitx}
``` 

#### Control over paragraphs and line spacing
LaTeX indents the first line of new paragraphs by default. This can be avoided by importing the `parskip` package. Line spacing can be controlled by `setspace`:
```tex
\usepackage{parskip}        
\usepackage{setspace}
```

#### Hyperlinks
The `hyperref` package provides support for a wide variety of referencing options in documents. Here, we configure the appearance of links to URLs, pages within the document, and bibliographic reference links.
```tex
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,        % links are coloured
    urlcolor=blue,          % URLs, etc
    linkcolor=black,        % Pages, etc
    citecolor=blue
}
```

#### Header and footer contents
We standardise running header and footer contents using the `fancyhdr` package. 
Report details appear at the top left corner of the page, the institute name in the centre, and page number on the right. 
The institute name is placed one line above the left and right details (achieved by the newline command, `\\`, that follows `\small\textbf{\LMI}`).
The footer centres a standard disclaimer  about use of the report, which defined in the macro `\RestrictionsLMI}`.
```tex {% raw %}
\usepackage{fancyhdr}
\fancypagestyle{fancy}{%
    \renewcommand{\headrulewidth}{0pt}  % Supress a line ruled by fancyhdr
    \fancyhf{}
    \fancyhead[L]{\small\textnormal{\ReportReference}}
    \fancyhead[C]{\small\textbf{\LMI}\\}
    \fancyhead[R]{\small\textnormal{\thispageof}}
    \fancyfoot[C]{\small\textit{\RestrictionsLMI}}
}

% Pre-defined strings
\newcommand{\LMI}{Lunar Metrology Institute}

\newcommand{\RestrictionsLMI}{%
This report may not be reproduced, 
except in full, without the written consent of the\\
Chief Metrologist, Lunar Metrology Institute}

\newcommand{\referenceGUM}{
For information about uncertainty terminology, see: 
BIPM, IEC, IFCC, ISO, IUPAC, IUPAP and OIML, 
\textit{``Evaluation of measurement data---Guide to the expression of uncertainty in measurement''}, 
BIPM Joint Committee for Guides in Metrology, Paris, S\`evres, edition 1, JCGM~100:2008, 2008. 
A PDF version is available on-line:

\begin{center}
	\href{%
		https://www.bipm.org/documents/20126/2071204/JCGM_100_2008_E.pdf
	}{%
		https://www.bipm.org/documents/20126/2071204/JCGM\_100\_2008\_E.pdf
	}
\end{center}
}

{% endraw %}```

#### Section headings and fonts
The appearance of section headings can be controlled by the `titlesec` package.
We wish to use particular fonts in these headings too.

Macros that define distinct fonts may use the LaTeX command `usefont`. 
The syntax for this command is `\usefont{encoding}{family}{series}{shape}`.
We are using the 'T1' encoding and the Helvetica family ('phy'). 
The series option chooses the font weight (e.g., 'm'--medium, or 'bc'--bold condensed).
The shape option specifies the style (e.g., 'n'--normal, or 'sl'--slanted).
```tex
\newcommand{\lmiboldarialnarrow}{\usefont{T1}{phv}{bc}{n}}
\newcommand{\lmiarial}{\usefont{T1}{phv}{m}{n}}
\newcommand{\lmiboldarialsnarrowlanted}{\usefont{T1}{phv}{bc}{sl}}

\usepackage{titlesec}
\titleformat{\section}
  {\lmiboldarialnarrow\Large}{}{0pt}{}
\titlespacing*{\section}
  {0pt}{\baselineskip}{0.5\baselineskip} 

\titleformat{\subsection}
    {\lmiboldarialnarrow\large}{}{0.5em}{}
\titlespacing*{\subsection}
    {0pt}{\baselineskip}{0pt}

\titleformat{\subsubsection}
    {\lmiboldarialsnarrowlanted\normalsize}{}{0.5em}{}
\titlespacing*{\subsubsection}
    {0pt}{\baselineskip}{0pt}
```
The syntax for the `\titleformat` command is `\titleformat{<command>}[<shape>]{<format>}{<label>}{<sep>}{<before-code>}[<after-code>]`.
We accept the default value for `<shape>`, which displays the title and starts text on a new line.
We combine a font selection and a font size in the `<format>` option (e.g., `\lmiboldarialnarrow\Large`).
We leave the `<label>` option blank to avoid generation of section numbers.
Option `<sep>` is a horizontal separation between the end of the section number (even if none has been produced) and the beginning of the name of the section.
Option `<before-code>` is used to insert LaTeX commands before the section title is generated (such as vertical space). 

The `titlespacing` command  allows the spacing around section titles to be adjusted.
The syntax of the `\titlespacing` command is `\titlespacing{command}{left}{before-sep}{after-sep}[right-sep]`.
We use it to insert one blank line before new sections.

#### Attachments
Files may be embedded in the document as attachments.
The package `embedfile` enables this feature. 
```tex
\usepackage{embedfile}
\embedfilesetup{     
      filesystem=URL,
      afrelationship={/Data},
      stringmethod=escape
}
```

#### Document metadata
We capture certain document details in our macros and use the `hyperref` package to include this metadata in the document.
```tex
\ReportNumber{12345} 
\ReportTitle{A report on the calibration of an type-N male open}
\date{17 May 2035}

%% XMP Metadata
\hypersetup{
    pdftitle={\ReportTitle},
    pdfauthor={\LMI},
    pdfnumpages={\pageref*{LastPage}}
}
```
[^1]: LaTeX packages are available on the Comprehensive TeX Archive Network [CTAN](https://ctan.org/). Documentation for every package can be found there. 

{% include links.html %}
