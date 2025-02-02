+++
title = 'Bookmarklets'
date = 2024-07-01T00:20:37-07:00
lastmod = 2025-02-02T00:24:53-08:00
tags = []
+++

[Bookmarklets](https://en.wikipedia.org/wiki/Bookmarklet) are browser bookmarks that run some JavaScript instead of navigating to a website. They can do anything JavaScript can do including change fonts, search for phone numbers, remove all images, change the background color, automatically fill a form, and so much more.

[This guide](https://www.hongkiat.com/blog/100-useful-bookmarklets-for-better-productivity-ultimate-list/) by Hongkiat Lim explains bookmarklets in more detail and lists many of the most popular ones.

## More examples

- Here's one I made. You can drag it into your bookmarks bar: {{< safeHtml >}}<a href="javascript: navigator.clipboard.writeText('[' + document.title + '](' + location.href + ')');">copy markdown link</a>{{< /safeHtml >}}. It puts into the clipboard a markdown link for the current page using the tab's title as the link title.
- [Here's an HN discussion](https://news.ycombinator.com/item?id=42902395) with many links to various bookmarklets.
- [web-automation/bookmarklets](https://github.com/madacol/web-automation/tree/master/bookmarklets), by Marco D'Agostini, is a collection of bookmarklets
- [Obsidian Web Clipper Bookmarklet](https://gist.github.com/kepano/90c05f162c37cf730abb8ff027987ca3), by Steph Ango, converts websites to markdown and saves them directly to Obsidian
- [Jesse's Bookmarklets Site](https://www.squarefree.com/bookmarklets/) (last updated in 2004)

## Bookmarklet editors

- [Bookmarklet Editor](https://www.gibney.org/bookmarklet_editor), by Marek Gibney, also has a few examples you can see by clicking the question mark on the page
- [Bookmarklet Creator with Script Includer](https://mrcoles.com/bookmarklet/) by Peter Coles
- [Bookmarkify.it](https://bookmarkify.it/) by Achilleas Buisman
