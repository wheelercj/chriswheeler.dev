+++
title = 'Copy repos between devices using bash'
date = 2025-10-21T20:51:58-07:00
lastmod = 2025-10-25T11:10:30-07:00
tags = []
+++

Sometimes it's helpful to copy a Git repository directly from one machine to another without using a Git host like GitHub. If you can SSH into the machine(s) involved, you can use Bash's `rsync` command. Rsync's defaults aren't great for copying repos for several reasons, but a Bash script can fix that.

I named the script `,cp-repo` (copy repo):

```bash
,cp-repo . staging:/home/chris/repos/url-shortener
```

This copies the current directory (`.`) to `/home/chris/repos/url-shortener` in a machine named `staging` as defined in an SSH config file. The first argument is the source, and the second is the destination. Files and folders ignored by Git are not copied. This command can also be used to make a local copy of a local repo by not specifying any remote host.

Here's the contents of my `,cp-repo` file:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [ $# -ne 2 ]; then
    echo "Error: expected two arguments: source and destination" >&2
    exit 1
fi

rsync --recursive --compress --rsh=ssh --perms --times --group \
    --exclude-from=<(git -C "$1" ls-files --exclude-standard --others --ignored --directory) \
    "$1" "$2"
```

I learned most of how to write this by combining a few answers in [this StackOverflow discussion](https://stackoverflow.com/questions/13713101/rsync-exclude-according-to-gitignore-hgignore-svnignore-like-filter-c).

- `--recursive` recursively copies all directories
- `--compress` compresses the data before sending, making the transfer significantly faster
- `--rsh=ssh` tells rsync to make the connection over SSH to ensure encryption
- `--perms` preserves permissions
- `--times` preserves modification times
- `--group` preserves group
- `--exclude-from` makes rsync ignore files & folders listed in a file
- `<(command)` is a [process substitution](https://www.gnu.org/software/bash/manual/html_node/Process-Substitution.html); it's replaced by the name of a file containing the command's output

- `git -C "$1"` runs the Git command in the folder specified by `$1`
- `git -C "$1" ls-files --exclude-standard --others --ignored --directory` lists all files and folders in the repo that are being ignored by Git

I chose to start the `,cp-repo` command's name with a comma because that makes it much less likely to conflict with future commands as explained in [Start all of your commands with a comma](https://rhodesmill.org/brandon/2009/commands-with-comma/).
