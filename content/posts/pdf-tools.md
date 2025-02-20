+++
title = 'PDF tools'
date = 2024-05-09T12:22:27-07:00
lastmod = 2025-01-22T13:04:34-08:00
tags = []
+++

Here are a bunch of tools I found for reading, editing, and more with PDFs. I have not tried all of these but they all look promising.

## Read

* [Sumatra PDF](https://www.sumatrapdfreader.org/free-pdf-reader) is the best PDF reader for Windows.
* The default PDF readers on other platforms seem to work well.

## Edit, annotate, sign, or fill

* [SimplePDF](https://simplepdf.eu/) is an in-browser PDF editor.
* Some browsers, such as Firefox, include PDF editing tools. You can select a PDF and "Open with" a browser.
* [PDF Annotator](https://pdf-annotator.repeat.day/) is for signing and annotating PDFs.
* [Zotero](https://www.zotero.org/) can save PDFs from websites and includes a PDF reader that lets you add annotations.
* [DocuSeal](https://github.com/docusealco/docuseal) is an open source DocuSign alternative for creating, filling, and signing documents.
* [ImageMagick](https://imagemagick.org/script/formats.php) is a suite of command line tools for manipulating images, including PDFs by using [Ghostscript](https://ghostscript.com/index.html).

## Add a text layer using optical character recognition (OCR)

Some PDFs don't come with a text layer. Adding one makes them searchable, copyable, etc.

* [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF)
* [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)

## Convert

* [Pandoc](https://pandoc.org/) converts files between many formats.
* [this HN discussion](https://news.ycombinator.com/item?id=39027543) covers and compares several tools that can be used to generate PDFs using HTML and CSS.
* [htmldocs](https://htmldocs.com/) is a React library for building and generating documents.
* [HTML to PDF conversion API \| html2pdf.app](https://html2pdf.app/)
* [Automate PDF Document Generation \| CraftMyPDF.com](https://craftmypdf.com/)
* [Cloudmersive's document and data conversion APIs](https://cloudmersive.com/convert-api)
* [Pdflayer API \| Free, High Quality HTML to PDF API](https://pdflayer.com/)
* [PrintFriendly](https://www.printfriendly.com/) converts many file formats to PDF for better printing.
* [PrintWhatYouLike.com](https://www.printwhatyoulike.com/) lets you print the good parts of any web page while skipping ads and other junk.
* [Marker](https://github.com/vikparuchuri/marker) converts PDF to markdown, optionally using [OCRmyPDF](https://github.com/ocrmypdf/OCRmyPDF).
* [MinerU](https://github.com/opendatalab/MinerU) converts PDF to markdown or JSON, optionally using [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR).
* [Bank Statement Converter](https://bankstatementconverter.com/) converts PDF bank statements to Excel's XLS format, though most banks let you export CSV files.

### Scanning paper documents

* [Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) helps you scan, index and archive all your physical documents.
* [pydigitize](https://news.ycombinator.com/item?id=30615279) uses OCRmyPDF and other tools to convert paper documents to PDFs ready for archival.

## Search

* [ripgrep-all](https://github.com/phiresky/ripgrep-all) is like ripgrep, but also searches PDFs and many other file types.
* [Zotero](https://www.zotero.org/) saves data alongside each saved document to make them easier to find.

## Organize

* [Zotero](https://www.zotero.org/) is excellent at organizing research. It's probably the best citation manager.

## Compare

* [Diff-pdf](https://news.ycombinator.com/item?id=40854319) is for visualizing the differences between two PDFs.
* [PDF-Diff](https://news.ycombinator.com/item?id=32353479) is another tool for visualizing the differences between two PDFs.

## Security

PDFs are not just static files. They can contain code. For example, someone built [Tetris in a PDF](https://news.ycombinator.com/item?id=42645218). Below are some tools that could help with PDF security.

* [VirusTotal](https://www.virustotal.com/gui/home/upload) can analyze files, websites, and more for security threats. This online service makes submissions public.
* [Jotti's malware scan](https://virusscan.jotti.org/en-US/scan-file) also scans files.
* [zbetcheckin's PDF analysis](https://github.com/zbetcheckin/PDF_analysis?tab=readme-ov-file) is a collection of tools for analyzing PDFs for security threats.
* [here's an HN discussion](https://news.ycombinator.com/item?id=41377960) that lists various tools for analyzing PDFs, such as to look for malicious code.
* [Canarytokens.org](https://canarytokens.org/nest/) can create PDFs and many other things that notify you when they're opened by certain applications to help you detect security breaches.
* [Submit a file for malware analysis \| Microsoft Security Intelligence](https://www.microsoft.com/en-us/wdsi/filesubmission). Use this for files that you believe are malware or are incorrectly classified as malware by Windows Defender.

## Automation

* [ImageMagick](https://imagemagick.org/script/formats.php) is a suite of command line tools.
* [OpenPDF](https://github.com/LibrePDF/OpenPDF) is a Java library.
* [Apache PDFBox](https://en.wikipedia.org/wiki/Apache_PDFBox)
* [QPDF](https://en.wikipedia.org/wiki/QPDF)

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

[ImageMagick](https://imagemagick.org/script/formats.php) might also be able to do many/all of these, but requires more technical skill as it is a suite of command line tools.
