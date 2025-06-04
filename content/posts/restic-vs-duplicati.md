+++
title = 'Restic vs. Duplicati'
date = 2025-06-02T15:41:11-07:00
tags = []
+++

[Restic](https://restic.net/) and [Duplicati](https://duplicati.com/) are both great for making backups of your computer's files. I have experience with both. They're mostly the same, but they have some differences that might be important depending on your needs.

## Similarities

They're the same in many ways, including:

- free
- open source
- versioning of backups
- choose specific files and folders to back up
- encryption
- compression
- deduplication
- verification
- scheduling of backups and retention (I now use [Resticprofile](https://creativeprojects.github.io/resticprofile/index.html) for this)
- support for many storage locations (I happen to use Backblaze B2)

## Differences

### Setup

Duplicati requires less technical skill to set up because its user interface is graphical. By contrast, Restic is a command line tool. It has many options to choose from, but it might not be immediately obvious which ones are best for you. I remember feeling a little surprised how it took several times longer to set up Restic than Duplicati. I'm glad I went through that setup process though; it was worth it for me. However, I wouldn't recommend Restic to anyone who isn't comfortable with terminals.

### Speed

Restic runs much faster than Duplicati. A typical backup with Duplicati would usually take about 10 minutes. The same backup with Restic takes only 7 seconds. While impressive, this might be less important because backups can run in the background. Although my backups have never been more than 50 GB, I've heard that Duplicati can get stuck and stop working if you try to restore around 500 GB or more.

### Configuration

**Excluding**

As a software developer, I have many large folders of temporary data that are pointless to back up. Their names are predictable but their locations are not. Even though storage space is cheap, storing this extra data adds up and costs a lot of money.

With Duplicati, you have to manually deselect each and every folder that you don't want to back up (if it's within a folder you are backing up). This is time consuming and prone to mistakes.

Restic is much more flexible. You can create `includes.txt` and `excludes.txt`, and just list paths and/or names(!) of files and folders you want to include and exclude from backups. [Patterns and environment variables](https://restic.readthedocs.io/en/stable/040_backup.html#backup-excluding-files:~:text=let%E2%80%99s%20say%20we%20have%20a%20file%20called%20excludes.txt%20with%20the%20following%20content%3A) are supported. Here are the command options to use includes and excludes files: `restic backup --files-from=includes.txt --exclude-file=excludes.txt`.

Having includes and excludes files also makes it easier to detect when there are new files and folders that are not being backed up but maybe should be. I wrote a small script for this. In folders that are not being backed up but contain many things that are, the script searches for anything in neither includes.txt nor excludes.txt and warns me about anything it finds.

```py
import os


def check_restic_backup_config() -> None:
    print("Checking Restic backup configuration")
    folders: list[str] = ["/home/chris", "/home/chris/.config"]

    with open(restic_includes_path, "r", encoding="utf8") as file:
        includes: list[str] = file.read().strip().splitlines()
    with open(restic_excludes_path, "r", encoding="utf8") as file:
        excludes: list[str] = file.read().strip().splitlines()

    found_count: int = 0
    for folder in folders:
        with os.scandir(folder) as it:
            for entry in it:
                if entry.path not in includes and entry.path not in excludes:
                    print("\t" + entry.path)
                    found_count += 1

    if found_count > 0:
        print(
            f"Found {found_count} files and/or folders that are not being backed up but maybe should be"
        )
    else:
        print("The Restic config files look good")
```

The function above is part of the script I described in [Why not cron on workstation](https://til.chriswheeler.dev/why-not-cron-on-workstation/).

**Troubleshooting**

It's easier to troubleshoot configuration problems in Restic. For example, one of my manually triggered backups took a lot longer than I expected. I was able to find out why using Restic's commands, and I wrote a script to make this easier: [wheelercj/find-restic-anchor: Find the largest files in your latest Restic backup](https://github.com/wheelercj/find-restic-anchor).

### Security

I'm not sure how meaningful this is, but it's interesting that [one of Google's security experts, Filippo Valsorda, uses Restic](https://words.filippo.io/restic-cryptography/) (or at least used to).

## My experience

I used Duplicati for several years and then switched to Restic in late 2024 because Duplicati does not have a Linux version and I had just switched from Windows 11 to Ubuntu 22.

During the years I used Duplicati, it saved my bacon multiple times. The only trouble I had with it besides a few small points described above was when I tried to back up to a Samba file server. For a few months of daily backups, everything seemed fine. Then, I suddenly got an error about the backups being corrupted or something to that effect. Duplicati's tool for fixing the backups wasn't able to save them, and online discussions about the problem said the backups were unrecoverable. I had to delete all the backups and start over. If I remember correctly, the problem was that Duplicati requires a lot of control over the metadata of the files it creates, but Samba changes file metadata to help with serving the files. Don't use Duplicati with a Samba file server! I don't know what would happen if you use Restic with a Samba file server.

I only started using Restic about 5 months ago, but I haven't had any problems with it so far.

## Conclusion

Both Restic and Duplicati are excellent choices. I recommend Restic for everyone comfortable with terminals and Duplicati for almost everyone else. I haven't tried other backup software yet, but I've heard good things about [Borg](https://www.borgbackup.org/) and I'm planning on making system image backups so that I have backups of settings, applications, the OS, etc.

## Related

- [Object lock with Restic](https://til.chriswheeler.dev/object-lock-with-restic/)
- [2024 Hacker News discussion comparing backup software](https://news.ycombinator.com/item?id=39117155)
