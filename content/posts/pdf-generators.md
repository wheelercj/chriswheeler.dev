+++
title = 'PDF generators'
date = 2026-04-26T14:54:13-07:00
tags = []
+++

PDFs can be generated programmatically in several ways. A common use case is converting a website to PDF, but some of the tools listed below work for other use cases too.

## Converting a website to PDF

If you're okay with text not being selectable or searchable, you could use [html-to-image](https://github.com/bubkoo/html-to-image) to turn the site into images, and then put the images into a new PDF with [jsPDF](https://github.com/parallax/jsPDF). Use JPEGs so the resulting file isn't too big.

If you're willing to rewrite some of the site's code, you can define the PDF in full detail using [react-pdf](https://github.com/diegomura/react-pdf), which is similar to React Native.

If you would prefer generating PDFs on a server and sending them to the client, you could set up [Puppeteer](https://github.com/puppeteer/puppeteer/tree/main), which can apparently create PDFs that look almost identical to the site but also keep the text selectable and searchable.

## Other options

There are also web APIs and many other tools for generating PDFs. I'm not sure all of these are good, but they look promising.

- [HTML to PDF conversion API - html2pdf.app](https://html2pdf.app/)
- [Automate PDF Document Generation - CraftMyPDF.com](https://craftmypdf.com/)
- [Pdflayer API \| Free, High Quality HTML to PDF API](https://pdflayer.com/)
- [PDF Tools for Documents and Web Pages - PrintFriendly](https://www.printfriendly.com/)
- [htmldocs - Build and generate documents with React](https://htmldocs.com/)
- [LibrePDF/OpenPDF: OpenPDF is an open-source Java library for creating, editing, rendering, and encrypting PDF documents, as well as generating PDFs from HTML. It is licensed under the LGPL and MPL.](https://github.com/LibrePDF/OpenPDF)
- [Apache PDFBox - Wikipedia](https://en.wikipedia.org/wiki/Apache_PDFBox)
- [QPDF - Wikipedia](https://en.wikipedia.org/wiki/QPDF)
- [Pandoc - index](https://pandoc.org/)
- [ImageMagick \| Image Formats](https://imagemagick.org/formats/)
- [Convert API - Cloudmersive APIs](https://cloudmersive.com/convert-api)
- [HN discussion with more](https://news.ycombinator.com/item?id=39027543)

## Related

- [PDF tools](/posts/pdf-tools)
