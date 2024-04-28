---
title: PDF/A-3 Documents
last_updated: April 28, 2024
summary: "Information about generating PDF/A-3 documents using LaTeX"
permalink: dxfg-pdfa-tut.html
toc: false
mathjax: true
layout: page
---
{% include important.html content="This page is currently being written and reviewed" %}

## Preliminaries
The PDF/A-3 file format offers a pathway, from purely human-readable documents to machine-readable documents. PDF/A-3 file formats can deliver readable metrological documents (i.e., PDF reader software can display the document) with digital files attached. This format can be used by calibration laboratories to tailor reporting to the needs of their customers as they move closer to using digital data. 

The suggestion to use PDF/A-3 formats for reporting metrological data was made by [METAS](https://doi.org/10.1016/j.measen.2021.100282). A proof-of-concept package has been published on [github](https://github.com/metas-ch/metas-ecertificate), which uses open-source tools---notably the LaTeX system for typesetting document---to create PDF/A files. 

However, since 2020, LaTeX has embarked on a multi-year [development project](https://pdfa.org/presentation/tagged-and-accessible-pdf-with-latex/) to produce tagged and accessible PDF from existing LaTeX sources with no or only minimal configuration adjustments.  This project has already simplified generation of PDF/A-3 documents. In the longer term, PDF files produced using LaTeX will contain rich semantic content that is machine readable. This is an exciting prospect for digital transformation in metrology.
 
The DXFG is making information and examples available here that will help develop PDF/A capability tailored to individual needs. A [github repository](https://github.com/apmp-dxfg/pdfa3-documents) is also available. 

## Resources
See [LaTeX](latex-res.html) for information about the tools we use to produce PDF/A-3 documents.

LaTeX cannot validate PDF/A-3 documents against the official PDF standard. Independent software is needed for that. We use the [veraPDF](https://verapdf.org/home/) *Implementation Checker* tool to validate documents.

{% include links.html %}
