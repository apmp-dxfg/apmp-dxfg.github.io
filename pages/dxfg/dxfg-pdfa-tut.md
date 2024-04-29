---
title: PDF/A-3 Documents
last_updated: April 28, 2024
summary: "Information about generating PDF/A-3 documents using LaTeX"
permalink: dxfg-pdfa-tut.html
toc: false
mathjax: true
layout: page
---
{% include important.html content="This page is currently being written and has not yet been reviewed" %}

## Preliminaries
The PDF/A-3 file format offers a pathway, from purely human-readable documents to machine-readable documents. PDF/A-3 file formats can deliver readable metrological documents (i.e., PDF reader software can display the document) with digital files attached. This format can be used by calibration laboratories to tailor reporting to the needs of their customers as they move closer to using digital data. 

The suggestion to use PDF/A-3 formats for reporting metrological data was made by [METAS](https://doi.org/10.1016/j.measen.2021.100282). A proof-of-concept package has been published on [github](https://github.com/metas-ch/metas-ecertificate), which uses open-source tools---notably the LaTeX system for typesetting document---to create PDF/A files. 

However, since 2020, LaTeX has embarked on a multi-year [development project](https://pdfa.org/presentation/tagged-and-accessible-pdf-with-latex/) to produce tagged and accessible PDF from existing LaTeX sources with no or only minimal configuration adjustments.  This project has already simplified generation of PDF/A-3 documents. In the longer term, PDF files produced using LaTeX will contain rich semantic content that is machine readable. This is an exciting prospect for digital transformation in metrology.
 
The DXFG is making information and examples available here that will help develop PDF/A capability tailored to individual needs. A [github repository](https://github.com/apmp-dxfg/pdfa3-documents) is also available. 

## Resources

The [LaTeX](latex-res.html) page has information about LaTeX software that can produce PDF/A-3 documents.

Independent software is needed to PDF/A-3 documents against the official PDF standard. We use the [veraPDF](https://verapdf.org/home/) *Implementation Checker* tool to validate documents.

Files embedded in a PDF/A-3 document can be extracted automatically using software or by hand using any of the popular PDF reader applications. We use the Python package [pypdf](https://pypi.org/project/pypdf/) to extract files.

## A Simple Example
A short example illustrates how to produce PDF/A-3 with LaTeX (the files shown may be accessed in the `minimal` folder of the [github respository](https://github.com/apmp-dxfg/pdfa3-documents)). 

In this example, a text file `attachme.txt` is embedded in the PDF output. A LaTeX file, `ex.tex`, is used to create the output `ex.pdf`. 

The contents of `attachme.txt` are
```
This file will be attached to a PDF document
``` 
The contents of `ex.tex` are
```tex
\DocumentMetadata{
    pdfversion=1.7,
    pdfstandard=A-3b,
}
\documentclass{article}

\usepackage{embedfile}
\embedfilesetup{     
      filesystem=URL,
      mimetype=application/octet-stream,
      afrelationship={/Data},
      stringmethod=escape
}

\begin{document}

You can check that this PDF document complies with the PDF/A-3 standard by using the veraPDF tool. 
A text file, \texttt{attachme.txt}, is embedded in the PDF document. 

\embedfile{attachme.txt}

\end{document}
```
Processing the LaTeX file generates a PDF output. The command line instruction to do this is
```
> pdflatex ex.tex
```

In the LaTeX file, the `\DocumentMetadata` command activates the new tagged-data features of LaTeX. This command must come before `\documentclass`. Files can be embedded using the `embedfile` package command `\embedfile`.  

The veraPDF checker can be used to confirm that the PDF file produced complies with the PDF/A-3b standard.

The embedded file `attachme.txt` can be extracted from `ex.pdf` and its contents displayed, as shown here:
```py
from pypdf import PdfReader

attached = PdfReader("ex.pdf").attachments  

if len(attached) != 1:
    raise RuntimeError("Expect a single attachment")

for file_name, content_list in attached.items():

    # Elements in content_list are Python byte-literals
    # Here we convert the bytes to a string.
    content_as_str = content_list[0].decode('utf-8')
    
    print(f"\nThe {file_name} file contents are:\t{content_as_str!r}")
``` 
{% include links.html %}
