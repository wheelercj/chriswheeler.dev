+++
title = 'Copy repos between devices using bash'
date = 2025-10-21T20:51:58-07:00
lastmod = 2026-07-20T14:12:37-07:00
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

# Copies a git repo either locally or to/from another machine. Files and
# folders ignored by git are not copied.
# https://chriswheeler.dev/posts/copy-repos-between-devices-using-bash/

if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_repo> <destination>" >&2
    exit 1
fi

SRC="$1"
DEST="$2"

# get a list of files and folders ignored by git
EXCLUDES_FILE="$(mktemp)"
trap 'rm -f $EXCLUDES_FILE' EXIT
if [[ "$SRC" =~ ^([^:]+):(.+)$ ]]; then
    # SRC is remote
    HOST="${BASH_REMATCH[1]}"
    REMOTE_PATH="${BASH_REMATCH[2]}"
    # shellcheck disable=SC2029
    ssh "$HOST" "cd \"$REMOTE_PATH\" && git ls-files --exclude-standard --others --ignored --directory" > "$EXCLUDES_FILE"
else
    # SRC is local
    (cd "$SRC" && git ls-files --exclude-standard --others --ignored --directory) > "$EXCLUDES_FILE"
fi

rsync --recursive --compress --rsh=ssh --perms --times --group \
    --exclude-from="$EXCLUDES_FILE" \
    "$SRC/" "$DEST"
```

I learned most of how to write this by combining a few answers in [this StackOverflow discussion](https://stackoverflow.com/questions/13713101/rsync-exclude-according-to-gitignore-hgignore-svnignore-like-filter-c).

- `--recursive` recursively copies all directories
- `--compress` compresses the data before sending, making the transfer significantly faster
- `--rsh=ssh` tells rsync to make the connection over SSH to ensure encryption
- `--perms` preserves permissions
- `--times` preserves modification times
- `--group` preserves group
- `--exclude-from` makes rsync ignore files & folders listed in a file

`git ls-files --exclude-standard --others --ignored --directory` lists all files and folders in the repo that are being ignored by Git.

I chose to start the `,cp-repo` command's name with a comma because that makes it much less likely to conflict with future commands as explained in [Start all of your commands with a comma](https://rhodesmill.org/brandon/2009/commands-with-comma/).
