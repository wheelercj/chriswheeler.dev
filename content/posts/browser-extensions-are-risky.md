+++
title = 'Browser extensions are risky'
date = 2025-08-31T23:57:57-07:00
tags = []
+++

During the time I've been making browser extensions, I've learned about the amazing things they can do and how helpful they are, but also how dangerous they can be. Below are some examples of risks and ways to mitigate them.

## Some of the risks

Even though browsers put limits on what extensions can do and require them to ask permission for some things, extensions can still do more than what most people expect. The relatively well-known risk is that some extensions silently gather and sell people's browsing history to data brokers. A lesser-known risk is that it's possible for some browser extensions to collect your usernames and passwords every time you log into a website, and send those credentials anywhere without you knowing.

Many extensions require permission to access your data for all websites. If you grant this permission, the extension can do almost anything and your browser can't protect you. When you install an extension and/or grant permissions, you're choosing to trust the extension's developers.

## Some ways to reduce the risks

### Uninstall or disable extensions you don't need

This is one of the easiest highly effective ways to protect yourself from malicious extensions. Periodically check which extensions you have installed and whether you stopped using any of them. Uninstall or disable any that are not important to you.

### Open some sites in private windows

Private windows are also known as incognito windows. By default, private windows don't load your browser extensions. That means any site you open in a private window cannot be read by any of your extensions. For sites you log into on each visit and don't need browser extensions for, you might as well open them in private windows.

It's easy to forget to use private windows for those sites. To help, you could block them using a browser extension. The sites won't be blocked in a private window since that extension won't load. There are many extensions that can block sites. For example, here are [uBlock Origin static filters](https://github.com/gorhill/uBlock/wiki/Static-filter-syntax) to block some sites:

```text
! These sites are blocked so I remember to open them in private windows.
||vault.bitwarden.com
||login.gov
||irs.gov
||backblaze.com
||porkbun.com
||venmo.com
||annualcreditreport.com
||transunion.com
||experian.com
||equifax.com
```

### Use multiple browser profiles

Profiles let you create groups of browser extensions and quickly switch between them. For example, you might have some extensions you use for work and a different set for personal use. Profiles contain not only extensions, but also browsing history, cookies, bookmarks, and more. This compartmentalization can help reduce the impact of security and privacy breaches.

Creating a browser profile only takes a few seconds; it doesn't involve creating another account. Here are instructions:

- Firefox: [Profile Manager - Create, remove or switch Firefox profiles](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles)
- Chrome: [Manage Chrome with multiple profiles](https://support.google.com/chrome/answer/2364824)

Alternatively, you could use multiple browsers or multiple versions of the same browser to achieve the same effect with different tradeoffs.

### Disable automatic updates for extensions

Extensions update automatically by default. Even if an extension is safe now, that could change at any time. One of the driving forces making this true is that extension developers sometimes receive offers of money in exchange for control over the extension or even the account publishing the extension. Sold extensions then have spyware added and start selling your browsing history and other private data with no indication of any change to users. In [Temptations of an open-source browser extension developer](https://github.com/extesy/hoverzoom/discussions/670), Oleg Anashkin shares hundreds(!) of offers he has received (and declined) to monetize his extension. He has received over 150 offers in about 10 years. The replies include similar stories from other extension developers. My extensions don't have enough users to attract these offers, but I will decline any I receive in the future.

In Firefox, you can toggle automatic updates on a per-extension basis: enter `about:addons` into the address bar, click an extension's button with three dots, choose "Manage", and then next to "Allow automatic updates", click "Off".

In Chrome, it's not nearly as easy to turn off automatic updates, unfortunately. There are guides you can find online that cover various ways to do this.

### Do a little research

Each extension *should* explain somewhere why it requires each of the browser permissions that it does. For example, [here are the explanations I wrote for Stardown](https://github.com/Stardown-app/Stardown/blob/main/docs/permissions.md). Read about the permissions for each extension you install and look for anything that doesn't make sense.

Every extension has a file named `manifest.json`. This file may list many important things including permissions, optional permissions, and more. If you can find the file, make sure it doesn't list any permissions you don't expect. In Firefox, you can find an installed extension's manifest by entering `about:debugging#/runtime/this-firefox` into the address bar and clicking a manifest URL.

If you know how to read code, reading some of the extension's code could be good. Maybe try searching for things like `fetch`, `XMLHttpRequest`, `$.get`, `Ajax.Request`, and `document.createElement('img')` to look for any web requests the extension could make to send data somewhere. It's common for good extensions to make web requests; the questions are what data is being sent (if any) and why.

### Create your own

If you have the skills, some time, and only need simple features, you could create your own browser extension for yourself so you don't have to risk installing someone else's. Browsers let you load extensions from their source code without uploading them to an extension marketplace. I wrote some tips for getting started in [Making browser extensions](/posts/making-browser-extensions.md).

This is why I created [Drumline: A simple browser extension for managing your time on websites of your choice](https://github.com/wheelercj/drumline). I put almost zero effort into the extension's appearance (at time of writing), and the features are very limited, but it meets my needs with much less risk.
