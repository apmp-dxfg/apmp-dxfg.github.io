---
title: PDF/A-3 Documents
last_updated: May 11, 2024
summary: "Information about generating PDF/A-3 documents using LaTeX"
permalink: dxfg-pdfa-tut.html
toc: true
mathjax: true
layout: page
---
{% include important.html content="This page is currently being written and has not yet been reviewed" %}

## Preliminaries
The PDF/A-3 file format offers a pathway to produce human-readable documents that are also containers for machine-readable documents. So, PDF/A-3 file formats can deliver readable metrological documents (i.e., PDF reader software can display the document) with digital files embedded. This format can be used by calibration laboratories to tailor their reporting to the needs of customers. 

The suggestion to use PDF/A-3 formats for reporting metrological data was made by [METAS](https://doi.org/10.1016/j.measen.2021.100282). A proof-of-concept package has been published on [github](https://github.com/metas-ch/metas-ecertificate), which uses open-source tools to create PDF/A files---notably the LaTeX system for typesetting documents. 

Since 2020, LaTeX has embarked on a multi-year [development project](https://pdfa.org/presentation/tagged-and-accessible-pdf-with-latex/) to produce tagged and accessible PDF from existing LaTeX source files with no or only minimal configuration adjustments.  This project has already simplified the generation of PDF/A-3 documents. In the longer term, PDF files produced using LaTeX could contain rich semantic machine-readable content. This is an exciting prospect for digital transformation in metrology.
 
In these pages the DXFG is making information and examples available that will help to develop PDF/A capability tailored to the specific needs of different members. A [github repository](https://github.com/apmp-dxfg/pdfa3-documents) is also available. 

## Resources

A [LaTeX](latex-res.html) page has information about LaTeX software for producing PDF/A-3 documents.

Independent software is needed to validate PDF/A-3 documents against the official PDF [ISO standard](https://pdfa.org/resource/iso-19005-pdfa/). We use the [veraPDF](https://verapdf.org/home/) *Implementation Checker* tool to validate documents.

Files embedded in a PDF/A-3 document can be extracted automatically by software, or by hand using any of the popular PDF reader applications. We use the Python package [pypdf](https://pypi.org/project/pypdf/) to extract files in the examples below.

## Examples

### Embedding a text file

This example produces a PDF/A-3 file with an embedded text file `attachme.txt` (files shown are available in the `minimal` folder of the [github respository](https://github.com/apmp-dxfg/pdfa3-documents)). 

The contents of `attachme.txt` are simply
```
This file will be attached to a PDF document
``` 

A LaTeX file (`ex.tex`) is used to create the final output, `ex.pdf`:
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
The PDF file `ex.pdf` is generated using the `pdflatex` command, like this:
```
> pdflatex ex.tex
```

In the LaTeX source file, the `\DocumentMetadata` command activates new features of LaTeX. This command must come before `\documentclass`. 

Files are embedded using the `embedfile` package command `\embedfile`.  

The veraPDF checker can be used to confirm that the PDF file produced complies with the PDF/A-3b standard.

To extract the contents of the embedded file `attachme.txt`, we can use the Python package `pypdf`:
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

### Embedding a spreadsheet
This example produces a PDF/A file with an embeded spreadsheet. The LaTeX file for this example is `ex_xlsx.tex`, in the `minimal` folder of the [github respository](https://github.com/apmp-dxfg/pdfa3-documents)). 

We must specify `mimetype=application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` in the command `\embedfilesetup`.  Here is the LaTeX source code:
```tex
\DocumentMetadata{
    pdfversion=1.7,
    pdfstandard=A-3b,
}
\documentclass{article}

\usepackage{embedfile}
\embedfilesetup{     
      filesystem=URL,
      mimetype=application/vnd.openxmlformats-officedocument.spreadsheetml.sheet,
      afrelationship={/Data},
      stringmethod=escape
}

\begin{document}

You can check that this PDF document complies with the PDF/A-3 standard by using the veraPDF tool. A text file, \texttt{xl.xlsx}, is embedded in the PDF document. 

\embedfile{xl.xlsx}

\end{document}
```

We can use Python (using the spreadsheet package `openpyxl`) to access the spreadsheet:
```py
import io
import openpyxl

from pypdf import PdfReader

attached = PdfReader("ex_xlsx.pdf").attachments  

if len(attached) != 1:
    raise RuntimeError("Expect a single attachment")

for file_name, content_list in attached.items():

    # Elements in content_list are Python byte-literals
    # Assuming byte_string contains the byte data of the XLSX file
    xlsx_data = io.BytesIO( content_list[0] )
    
    # Load the Excel workbook from the byte data
    workbook = openpyxl.load_workbook(xlsx_data)
    
    # Access the sheets in the workbook
    sheets = workbook.sheetnames

    # Iterate over each sheet and extract data
    for sheet_name in sheets:
        sheet = workbook[sheet_name]
        print(f"Sheet: {sheet_name}")
        for row in sheet.iter_rows(values_only=True):
            print(row)
```

### XMP metadata
Adobe allows information formatted in XML to be included in a PDF file. This is known as [XMP metadata](https://en.wikipedia.org/wiki/Extensible_Metadata_Platform). PDF reader software is often able to display this metadata, but it is primarily intended for easy sharing and transfer of files across products, vendors, platforms, without metadata getting lost. 

Here is an example where the title, author, date, and publisher are recorded in the PDF/A file as metadata (a complete list of supported metadata is given in the [hyperref](http://mirrors.ctan.org/macros/latex/contrib/hyperref/doc/hyperref-doc.pdf)) package documentation. 
```tex
\DocumentMetadata{
    pdfversion=1.7,
    pdfstandard=A-3b,
}
\documentclass{article}
\usepackage{hyperref}
\hypersetup{
    pdftitle={On a heuristic viewpoint concerning the production and transformation of light},
    pdfsubtitle={[en-US]Putting that bum Maxwell in his place},
    pdfauthor={Albert Einstein},
    pdfdate={1905-03-17},
    pdfpublisher={Wiley-VCH},
}

\begin{document}
You can check that this PDF document complies with the PDF/A-3b standard by using the \href{https://verapdf.org/}{veraPDF} tool. 

\end{document}
``` 

Python can read this metadata using `pypdf`. However, `pypdf` only makes a few elements accessible as Python attributes, as shown below. Access to the all XMP data is more complicated. So, we will deal with that elsewhere. 
```py
from pypdf import PdfReader

meta = PdfReader("ex.pdf").xmp_metadata 

print( meta.dc_title )
print( meta.dc_creator )
print( meta.dc_date )
print( meta.dc_publisher )
```
{% include links.html %}
