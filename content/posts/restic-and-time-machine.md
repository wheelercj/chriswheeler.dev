+++
title = 'Restic and Time Machine'
date = 2026-03-26T22:04:37-07:00
tags = []
+++

[Restic](https://github.com/restic/restic) and [Time Machine](https://en.wikipedia.org/wiki/Time_Machine_%28macOS%29) (TM) are backup programs that complement each other with their different strengths and weaknesses. I now use both: Restic for remote backups and Time Machine for most local backups.

## Platforms

TM is only available for MacOS while Restic runs almost anywhere.

## Ease of use

TM is much easier to use than Restic. When you connect an external drive to your computer, a popup will appear asking if you want to back up your computer to that drive. It only takes a minute to start the first backup. Restic is kind of the opposite because it requires using several terminal commands and probably configuration files. Installing Restic, reading the documentation, and setting everything up could take hours.

## Remote backups

Restic is much better than TM for remote backups. While TM technically supports remote backups, they can only be made to disks that are using one of Apple's filesystems: APFS or HFS+. My understanding is that means you can't back up to any S3-compatible storage, which rules out all of the cheap, reliable, commonly used cloud storage options. You might even have to set up the physical disk somewhere yourself. In any case, many online discussions recommend using TM only for local backups.

## Backup size and speed

TM tends to use significantly more time and disk space than Restic. In fact, if you try to set up Time Machine to back up to a drive that has less than double the capacity of the drive you're backing up, TM will show you a warning and recommend using a drive with more capacity. One reason why is Restic lets you include and exclude files and folders in multiple ways including by name, but TM includes everything and only allows excluding by choosing existing files and folders. This is especially a problem for software developers with folders like node_modules that are massive, appear in many places, and don't need to be backed up. It *might* be possible to write a script that scans your entire computer for new folders and files to exclude and updates TM's list of excludes, then somehow set that script to run before every backup.

Backup speed usually doesn't matter as much since backups can run in the background, but you can expect TM backups to a local hard disk drive to take hundreds of times longer than Restic backups to remote storage. I haven't looked into whether Restic's deduplication and compression are more efficient.

## Specialization

There's a chance that TM backs up more file metadata than alternatives since it's specialized for MacOS. I haven't looked into this though.

## Why do I use both?

Using two backup programs requires a little more setup, but it increases redundancy. If one backup program has a bug or some other problem that corrupts the backups, the other program's backups will probably still work. On top of that, in the unlikely event that Apple mistakenly bans you from all of their services, you'll have backups that you know they can't touch. This is less important than what [the 3-2-1 backup rule](https://en.wikipedia.org/wiki/Backup) covers, but it's a nice addition beyond it.

## MacOS packages

Note that many Mac applications store their data not as files in folders, but as [packages](https://en.wikipedia.org/wiki/Package_(macOS)). A package is like a single file that stores all of the data an application has, such as all of the photos and videos for the Photos app. I haven't tested it yet, but MacOS's use of packages might make it impossible to restore individual files from within packages.
