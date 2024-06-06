---
title: Report Body Style Development
last_updated: May 18, 2024
summary: "LaTeX specification of the style used for the body of Lunar Metrology Institute reports is explained"
permalink: dxfg-pdfa-tut-body-style.html
toc: true
mathjax: true
layout: page
---
## Report Body Style

We explain how the appearance of material in the body of Lunar Metrology Institute reports is controlled in LaTeX. 

### LaTeX file for report body

Here is a LaTeX file with commands that specify appearance and some content in the report.
The source file is available on [github](https://github.com/apmp-dxfg/pdfa3-documents/tree/main/LMI-body-dev){:target="_blank"}.


The appearance can be specified in the preamble of a LaTeX file (before `\begin{document}`) by importing packages and declaring commands. 
Appearance is also influenced by the LaTeX 'class' file (`article`), which is imported by `\documentclass`. 
The report content itself goes in between `\begin{document}` and `\end{document}`.

A title page is not produced.
Some of the configuration commands were already discussed in relation to the layout of the [title page](dxfg-pdfa-tut-title-style). 
We explain other details below.

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

%% Make captions without using LaTeX floating figure or table environments 
\usepackage{capt-of}
\newcommand{\LMICaption}[3]{
    \begin{minipage}{.8\textwidth}        
        \captionof{#1}{\small #3}
        \label{#2}     
    \end{minipage}
    \vspace{.5\baselineskip}
}

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
%% The compilier must be run twice to find the LastPage number.
\usepackage{lastpage}       
\newcommand{\thispageof}{Page~\thepage~of~\pageref*{LastPage}}

 
\usepackage{parskip}        % Paragraph formatting
\usepackage{setspace}       % Control of line spacing

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
    \renewcommand{\headrulewidth}{0pt}  % Suppress a line ruled by fancyhdr
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
	
\vspace*{4\baselineskip}
\textbf{\LARGE\ReportTitle}    
	
\section{Description}
The components are from a USC vector network analyser calibration kit model 8599. 

\section{Identification}
The component serial number is 2221X.

\section{Client}
United Spacecraft Corporation, 51 Mare Tranquillitatis, The Moon.

\section{Date of Calibration}
The measurements were performed on February 7\textsuperscript{th}, 2035.

\section{Conditions}
Ambient temperature was maintained within \SI{\pm 1}{\celsius} of \SI{-123}{\celsius}.

\section{Method}
Measurements of the voltage reflection coefficient were made according to procedure LMIT.E.063.005. 

\clearpage    % Anticipate the page break
\section{Results}

% Although not used here, it is also possible to have a \subsubsection{}
\subsection{Open (male), SN 54673}

\begin{center} 
    \small	% smaller font size for the table entries

    % Increases the vertical spacing between rows slightly  
    \setlength{\extrarowheight}{3pt}
    %
    % the 'S' array column type will align numbers on the decimal 
    % Note 'S[group-minimum-digits=3]' or '\sisetup{group-minimum-digits=3 }'
    % would be used to force a space separator every 3 digits (this
    % does not happen by default until there are more than 4 digits)
    \begin{tabular}{SSSSS}
        \multicolumn{1}{c}{ frequency } & 
        \multicolumn{2}{c}{ magnitude } &
        \multicolumn{2}{c}{ phase } 
        \\
		% 2nd line 
        \multicolumn{1}{c}{ (/\si{\mega\hertz}) } &  
        \multicolumn{2}{c}{  } &
        \multicolumn{2}{c}{ (/\si{\degree}) } 
        \\
  		% 3rd line 
        & $\rho$ & ${U(\rho)}$ & $\phi$ & ${U(\phi)}$ 
        \\ \hline % Underline the headings

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
        9000 &    0.996 &    0.018 &    +62.7 &      1.1  
		
    \end{tabular}
        
    \LMICaption{table}{tab1}{%
    Magnitude and phase data, using a linear scale for magnitude and units of degrees for phase. 
    Expanded uncertainties decorated by a $\dagger$ fall outside the scope of accreditation (see Uncertainty section).
    }
	
\end{center}


\section{Uncertainty}
A coverage factor $k=1.96$ was used to calculate the expanded uncertainties $U(\cdot)$ at a level of confidence of approximately \SI{95}{\percent}. 
The number of degrees of freedom associated with each measurement result was large enough to justify this coverage factor.  

Some of the expanded uncertainty values reported fall outside LMI's current scope of accreditation. 
These values are decorated by a $\dagger$ in Table~\ref{tab1}. 
The least expanded uncertainty for a measured magnitude close to unity in the LMI scope of accreditation is currently 0.0024. 

\paragraph{Note:} \referenceGUM	

\embedfile[mimetype=application/vnd.openxmlformats-officedocument.spreadsheetml.sheet]{ex_data.xlsx}

\end{document}
{% endraw %}
{% endhighlight %}

### Page style definitions in the preamble 

#### Conventional language and maths options
A certain style of typesetting (here UK English) can be selected by importing the following packages
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

#### Figure and table captions
LaTeX has 'floating' environments for figures and tables that automatically place these objects to balance the layout of a document. We do not wish to use these features. 

We must define a special environment, called `\LMICaption`, to add captions, and refer to figures and tables from within the document body. 
The syntax is `\LMICaption{<type>}{<label>}{<caption-body>}`, where `type` is either 'figure' or 'table'; `label` is our choice of label that refers to the object; and `caption-body` is the text to appear in the caption.
```tex
\RequirePackage{capt-of}
\newcommand{\LMICaption}[3]{
    \begin{minipage}{.8\textwidth}        
        \captionof{#1}{\small #3}
        \label{#2}     
    \end{minipage}
    \vspace{.5\baselineskip}
}
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
    \renewcommand{\headrulewidth}{0pt}  % Suppress a line ruled by fancyhdr
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

### Report body contents

#### Header, footer, and left alignment of text 
The first lines after `\begin{document}` set the page style to 'fancy', in which a header and footer have been defined. 
The `\raggedright` command tells LaTeX not to justify the lines of text, which gives a ragged appearance on the right hand side of the text.  
The report title is printed, below 4 lines of vertical space, in LaTeX's `\LARGE` sized print. 
```tex
\begin{document}
\pagestyle{fancy}
\raggedright        % Does not adjust the right margin
	
\vspace*{4\baselineskip}
\textbf{\LARGE\ReportTitle}    
```

#### Special content markup
There are a few examples of content mark-up in the body of the report.
* `\textsuperscript` places text in a superscript position 
* The `\SI` command can be used to format units and numbers correctly
* The command `\clearpage` forces a page break  
* Inline mathematical content is enclosed in a pair of `$`
* `\ref{<label>}` returns the number allocated to whatever object was labelled
* Content can be centred horizontally on the page between `\begin{center}` and `\end{center}` 
 
#### Tables
The structure of tables in LaTeX is rather intricate. 
There are many features available. 
However, tables are usually designed according to specific needs.
So, we have just included one table, as an example, in this report. 

The body of the table is created by the `tabular` environment. 
The syntax of the environment opening statement is `\begin{tabular}{cols}`, where `cols` is a sequence of characters that define the alignment of content in the table columns.
There are five columns each of type `S`, which will align numbers on the decimal point
```tex
\begin{tabular}{SSSSS}
```

The `\multicolumn` command allows content to span several adjacent columns. 
The command syntax is `\multicolumn{n}{col}{text}`, where `n` is the number of columns that will be joined, `cols` defines the alignment of content in the column, and `text` is the content.
The `&` character acts as a separator between columns.
So, a table with a three-line header followed by a horizontal line is created by 
```tex
    \multicolumn{1}{c}{ frequency } & 
    \multicolumn{2}{c}{ magnitude } &
    \multicolumn{2}{c}{ phase } 
    \\
    % 2nd line 
    \multicolumn{1}{c}{ (/\si{\mega\hertz}) } &  
    \multicolumn{2}{c}{  } &
    \multicolumn{2}{c}{ (/\si{\degree}) } 
    \\
    % 3rd line 
    & $\rho$ & ${U(\rho)}$ & $\phi$ & ${U(\phi)}$
    \\ \hline % Underline the headings
```

Numerical data are entered between `&` separators, mathematical symbols are enclosed in a pair of `$`, and each row ends with the newline command `\\`.
The table is completed by closing the environment.
```tex
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
    9000 &    0.996 &    0.018 &    +62.7 &      1.1  
\end{tabular}
```
[^1]: LaTeX packages are available on the Comprehensive TeX Archive Network [CTAN](https://ctan.org/){:target="_blank"}. Documentation for every package can be found there. 

{% include links.html %}
