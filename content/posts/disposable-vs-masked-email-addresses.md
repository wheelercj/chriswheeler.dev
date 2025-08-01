+++
title = 'Disposable vs. masked email addresses'
date = 2025-07-31T12:34:29-07:00
tags = []
+++

Sometimes it's helpful to give the terms "disposable" and "masked" two different meanings when referring to types of email addresses.

Both disposable addresses and masked addresses are great at protecting you from spam and phishing emails. Instead of giving out your main address, you can give out ones that are temporary or easy to replace.

Some email protection services have different sets of features:

- addresses with inboxes that are completely public
- addresses with private inboxes, but you only get access to a random address for a short time (say, 10 minutes)
- addresses that you keep access to as long as you want, and all emails they receive are automatically forwarded to your main address

I'll refer to the last of those as "masked addresses" and the others as "disposable addresses" even though [Wikipedia's page on disposable addresses](https://en.wikipedia.org/wiki/Disposable_email_address) doesn't make a distinction between them. Most masked address services seem to prefer using the term "masked" over "disposable", while most disposable address services don't seem to mind either one. It makes sense. A masked address belongs to you, tends to be used for many years, and is like a mask over your main address rather than a separate inbox. They may be easy to replace, but they usually never need to be (at least in my experience). The ones I'm calling disposable addresses, on the other hand, are always disposed.

Another sign it's helpful to make a distinction between masked and disposable addresses is that some lists of disposable addresses don't appear to include masked address services. For example, [disposable-email-domains](https://github.com/disposable-email-domains/disposable-email-domains) doesn't include any of the masked address services I'm familiar with (listed below). I know some sites don't distinguish between the two though. When I created an account at a domain registrar using a masked address, I couldn't buy/rent a domain until I changed my account's email address to my main one. Their support described the address I first used as "disposable". In hindsight, it's better to use your main address for the most important accounts like banking, government, domain registrars, etc. anyways because you don't want people to wonder if you're the real owner of those accounts.

If you use masked email addresses, check out a FOSS application I made called [Email Linter](https://github.com/wheelercj/email-linter) that makes it easier to detect spam and phishing emails received by masked addresses.

## Example services

I don't keep track of what disposable address services exist anymore because I don't see any reason to use them now that masked address services are widely available.

Some examples of masked address services are [DuckDuckGo's Email Protection](https://duckduckgo.com/email), [Fastmail's Masked Email](https://www.fastmail.help/hc/en-us/articles/4406536368911-Masked-Email), [Proton's hide-my-email aliases](https://proton.me/pass/aliases), [Firefox Relay](https://relay.firefox.com/), and [iCloud+'s Hide My Email](https://support.apple.com/en-us/105078).

It might be good to use more than one of these email protection services so that if one you're using shuts down or has some problem, you won't have to change the email addresses for all of your online accounts at once. Don't put all your eggs in one basket.

## Similar ways to protect addresses

If you have a Gmail address (e.g. `example@gmail.com`), when entering your address somewhere, you can add a plus sign followed by letters of your choice (e.g. `example+yourNoteHere@gmail.com`). Then when you receive spam with `+yourNoteHere` in the address, you know that the site you entered that on sold you out or had a security breach. I don't use this trick anymore because spammers could just remove that part of the address, and because we have masked addresses now. I guess you could make a habit of always using this `+` note trick and considering any emails without that to be spam.

If you own a domain (e.g. `example.com`), you could make a new email address for each account (e.g. `google@example.com` for your Google account, `microsoft@example.com` for your Microsoft account, etc.).
