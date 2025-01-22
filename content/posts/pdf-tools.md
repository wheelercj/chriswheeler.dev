+++
title = 'PDF tools'
date = 2024-05-09T12:22:27-07:00
lastmod = 2024-11-18T13:07:45-08:00
tags = []
+++

Here are a bunch of tools I found for reading, editing, and more with PDFs. I have not tried all of these but they all look promising.

* [Sumatra PDF](https://www.sumatrapdfreader.org/free-pdf-reader) is the best PDF reader for Windows.
* [SimplePDF](https://simplepdf.eu/) is an in-browser PDF editor.
* [Zotero](https://www.zotero.org/) includes a PDF reader that lets you add annotations to PDFs, and makes it easy to save PDFs and info about them from websites.
* [Pandoc](https://pandoc.org/) converts files between many formats.
* [Ghostscript](https://ghostscript.com/index.html) is an interpreter for the PostScript® language and PDF files.
* [ImageMagick](https://imagemagick.org/script/formats.php) is a suite of command line tools for manipulating images, including PDFs by using Ghostscript.
* [DocuSeal](https://github.com/docusealco/docuseal) is an open source DocuSign alternative for creating, filling, and signing documents.
* [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF) performs optical character recognition (OCR) to add a text layer to PDFs, making them searchable, among other benefits.
* [Marker](https://github.com/vikparuchuri/marker) converts PDF to markdown, optionally using OCRmyPDF.
* [MinerU](https://github.com/opendatalab/MinerU) converts PDF to markdown or JSON, optionally using [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR).
* [pydigitize](https://news.ycombinator.com/item?id=30615279) uses OCRmyPDF and other tools to convert paper documents to PDFs ready for archival.
* [Diff-pdf](https://news.ycombinator.com/item?id=40854319) is for visualizing the differences between two PDFs.
* [PDF-Diff](https://news.ycombinator.com/item?id=32353479) is another tool for visualizing the differences between two PDFs.
* [PDF Annotator](https://pdf-annotator.repeat.day/) is for signing and annotating PDFs.
* [ripgrep-all](https://github.com/phiresky/ripgrep-all) is like ripgrep, but also searches PDFs and many other file types.
* [Bank Statement Converter](https://bankstatementconverter.com/) converts PDF bank statements to Excel's XLS format, though most banks let you export CSV files.
* [PrintFriendly](https://www.printfriendly.com/) converts many file formats to PDF for better printing.
* [PrintWhatYouLike.com](https://www.printwhatyoulike.com/) lets you print the good parts of any web page while skipping ads and other junk.
* [this HN discussion](https://news.ycombinator.com/item?id=39027543) covers several tools that can be used to generate PDFs using HTML and CSS.
* [here's an HN discussion](https://news.ycombinator.com/item?id=41377960) that lists various tools for analyzing PDFs, such as to look for malicious code.

## Security

PDFs are not just static files. They can contain code. For example, someone built [Tetris in a PDF](https://news.ycombinator.com/item?id=42645218). Below are some tools that could help with PDF security.

* [VirusTotal](https://www.virustotal.com/gui/home/upload) can analyze files, websites, and more for security threats. This online service makes submissions public.
* [Jotti's malware scan](https://virusscan.jotti.org/en-US/scan-file) also scans files.
* [zbetcheckin's PDF analysis](https://github.com/zbetcheckin/PDF_analysis?tab=readme-ov-file) is a collection of tools for analyzing PDFs for security threats.
* [Canarytokens.org](https://canarytokens.org/nest/) can create PDFs and many other things that notify you when they're opened by certain applications to help you detect security breaches.
* [Submit a file for malware analysis - Microsoft Security Intelligence](https://www.microsoft.com/en-us/wdsi/filesubmission). Use this for files that you believe are malware or are incorrectly classified as malware by Windows Defender.

## Collections

These applications and websites are collections of many PDF tools.

| | [PDFTool](https://www.pdftool.org) | [PDFEquips](https://www.pdfequips.com) | [iLovePDF](https://www.ilovepdf.com/) | [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF) | [Xodo](https://xodo.com/tools) | [Foxit](https://www.foxit.com/) |
| --- | --- | --- | --- | --- | --- | --- |
| type | in-browser | online | online, desktop, & mobile | self-hosted | online, desktop, & mobile | online |
| add text, images, etc. | ? | ? | ✅ | ✅ | ✅ | ✅ |
| split | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| merge | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| rearrange | ? | ✅ | ✅ | ✅ | ✅ | ✅ |
| rotate | ✅ | ✅ | ✅ | ✅ | ✅ | ? |
| crop | ? | ? | ? | ✅ | ✅ | ✅ |
| flatten | ? | ? | ? | ✅ | ✅ | ✅ |
| remove pages | ✅ | ? | ? | ✅ | ✅ | ✅ |
| number pages | ? | ✅ | ✅ | ✅ | ? | ✅ |
| OCR | ✅ | ✅ | ✅ | ? | ✅ | ✅ |
| translate | ? | ✅ | ? | ? | ? | ? |
| convert file type | ? | ✅ | ✅ | ✅ | ✅ | ✅ |
| compare | ? | ? | ✅ | ✅ | ✅ | ✅ |
| optimize | ✅ | ? | ? | ? | ? | ? |
| repair | ? | ? | ✅ | ? | ? | ? |
| compress | ? | ✅ | ✅ | ✅ | ✅ | ✅ |
| sign | ✅ | ? | ✅ | ✅ | ✅ | ✅ |
| watermark | ? | ✅ | ✅ | ✅ | ? | ✅ |
| redact | ? | ? | ✅ | ? | ✅ | ✅ |
| lock (encrypt) | ✅ | ✅ | ✅ | ✅ | ? | ✅ |
| unlock (decrypt) | ✅ | ✅ | ✅ | ✅ | ? | ✅ |
| change metadata | ? | ? | ? | ✅ | ? | ? |
| adjust colors/contrast | ? | ? | ? | ✅ | ? | ? |
| extract images | ? | ? | ? | ✅ | ? | ? |
| automation | ? | ? | ? | ✅ | ? | ✅ |

[ImageMagick](https://imagemagick.org/script/formats.php) might also be able to do many of these, but requires more technical skill as it is a suite of command line tools.
