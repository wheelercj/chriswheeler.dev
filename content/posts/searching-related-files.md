+++
title = 'Searching related files'
date = 2026-07-19T21:32:46-07:00
tags = []
+++

Searching a category of files can be done in many ways. For example, a script that searches for a phrase in all GitHub Actions workflows on a computer could take only around a dozen lines of code in most languages.

The best way overall I've found so far is with [rg (ripgrep)](https://github.com/BurntSushi/ripgrep) in a bash script. It's great because writing it doesn't take long, the output formatting is useful, and it's easy to adjust the output without changing the script.

```bash
#!/usr/bin/env bash
set -euo pipefail

# Searches the contents of all GitHub Actions workflows.

REPOS_FOLDER="$HOME/repos"

rg \
    --ignore-case \
    --hidden \
    --glob='**/.github/workflows/*.{yml,yaml}' \
    --glob='!archive' \
    --glob='!build' \
    --glob='!libraries' \
    --glob='!node_modules' \
    --glob='!venv' \
    --glob='!.venv' \
    "$@" "$REPOS_FOLDER"
```

The exclusion of some folders makes the search around twice as fast in my case.

Here's what the output looks like when there's one match (I named the script `,search-actions`):

![example output](/search-actions-1.png)

I can also use more of `rg`'s options without editing the script, such as `-A` to see some lines after each match:

![example output](/search-actions-2.png)
