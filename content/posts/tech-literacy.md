+++
title = 'Tech literacy'
date = 2024-01-21T10:04:19-08:00
lastmod = 2024-02-01T23:43:33-08:00
tags = []
+++

There's always room for improvements worth their time to make. Over years of extensively using computers, there are some tricks I've found that I don't see others use as often as I expect. I'm not affiliated with any of the services, products, or companies mentioned here.

## learn about tech gradually and bravely

Everyone becomes overwhelmed or frustrated by technology sometimes. People good with tech tend to:

* return to difficult challenges to try again after some rest
* reduce how often they become frustrated by spreading out challenges over time
* take notes to make it easier to continue where they left off after breaks
* not be afraid to guess and try things
* ask others for help

If this page gets overwhelming, maybe bookmark it, close it, and read another section later.

## search features

I sometimes see people spend a lot of time deciding which folder to put a file or bookmark into, and also a lot of time searching through their folders for something. Many applications have great search features that allow you to spend little to no time organizing but still quickly find what you need. Find these search features and make use of them to save tons of time. Some applications have significantly better search features than others; consider this when choosing between apps.

## password managers

Choices related to security are usually a trade-off between convenience and security, but password managers increase both in the long run. I happen to use [Bitwarden](https://bitwarden.com/) and it's great; it has an excellent free tier and can optionally be self-hosted. I've also heard good things about [1Password](https://1password.com/) and [KeePass](https://keepass.info/). I recommend avoiding password managers built into a specific operating system or browser in case you want your passwords to be available elsewhere in the future.

## email protection services

Preventing spam is much easier when you avoid giving out your main email address by using email protection services like [DuckDuckGo's Email Protection](https://duckduckgo.com/email), [Fastmail's Masked Email](https://www.fastmail.help/hc/en-us/articles/4406536368911-Masked-Email), [Proton's hide-my-email aliases](https://proton.me/pass/aliases), [Firefox Relay](https://relay.firefox.com/), or [iCloud+'s Hide My Email](https://support.apple.com/en-us/105078). If you start getting spam, you will know exactly where it's coming from and can immediately and completely stop the spam by disabling the compromised disposable email address.

## canaries

Canaries are email addresses, URLs, files, credit card numbers, and more that can detect and alert you when accounts and devices are compromised. Security is not just about preventing breaches, but also responding to them well when they do happen. How can you respond to a security breach if you never know about it or only notice it days later? With well-placed canaries, you can be alerted in seconds. The canaries from [canarytokens.org](https://canarytokens.org/generate), for example, send you an email when they are accessed. Some of their canaries are easy to try, so maybe make one and trigger it yourself to see what data it can gather from attackers. Disposable email addresses (see the "email protection services" section above) also act kind of like canaries when you use a different one for each account.

Canaries have the unique property that when you tell others you use canaries, your security increases despite the fact you're telling others part of how your security works. When attackers know you use canaries, they are more hesitant. By the way, I use canaries extensively.

## automated backups

This is different from syncing. If you only sync your files, the accidental loss of important files will also sync. But with good backup software, you will have multiple copies of all your important files, each from a different time. Having automated backups can also give you the freedom to take more risks and save time. [Duplicati](https://www.duplicati.com/) backing up to [Backblaze B2 Cloud Storage](https://www.backblaze.com/) has been working well for me, and there are other options out there that are easier to use but have fewer features or cost more. At the time of writing, I'm paying about $0.07 USD per month to back up about 36 GB.

### test your backups

Sometimes, for whatever reason, a backup can't be restored. It could be a backup index file got corrupted, or maybe the backup software didn't take into account an important difference between the operating systems the backup was made to and from. I used to back up files to a [Samba](https://en.wikipedia.org/wiki/Samba_(software)) file server with Duplicati and found out the hard way that they (at least at the time) were incompatible.

If you wait to try to restore your backups until you actually need to, you might lose everything. No matter what backup software you use, test it occasionally.

### email backups

If you use Gmail or a similar email service, you can set up automated backing up of your emails by connecting it to any local email client, such as [Thunderbird](https://www.thunderbird.net). If you're interested, I recommend searching online for a guide for how to do this.

## specialized apps

Although apps that do everything can sometimes be good, I have often had better experiences using multiple apps that each specialize in doing one thing well. For example, calendars and task lists might seem obvious to combine, but doing so will slow down how you use one or both while providing little benefit. Similarly, an app made by a company that specializes in making that one app tends to be better than a competing app made by a company that doesn't specialize.

## for Windows users: PowerToys

[PowerToys](https://www.fourth-wall.co.uk/post/powertoys-11-awesome-features-microsoft-won-t-add-to-windows) is a collection of helpful tools made by Microsoft, such as for resizing images, picking colors from anywhere on the screen, bulk file renaming, creating custom keyboard shortcuts, changing what your mouse buttons do, window management, and much more.

## custom domains for emails

Since I bought the `chriswheeler.dev` domain, I can create email addresses like `anything@chriswheeler.dev` and receive emails at almost any email service with those addresses. This means that in addition to the addresses looking professional and the fact I use the domain for websites, if I ever switch to a different email service, I get to continue using the same email addresses. Prices for domains vary; mine is about $11 USD per year.

## related

* [Don't put all your eggs in one account](/dont-put-all-your-eggs-in-one-account)
