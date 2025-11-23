+++
title = 'Exit in Python'
date = 2025-11-22T21:29:53-08:00
lastmod = 2025-11-23T12:19:23-08:00
tags = []
+++

Python has several ways to make a program exit.

[`exit`](https://docs.python.org/3/library/constants.html#exit) should probably only be used to exit a Python REPL because it's not safe to assume it's defined.

The [`sys.exit`](https://docs.python.org/3/library/sys.html#sys.exit) function is safe, but you might prefer to raise [`SystemExit`](https://docs.python.org/3/library/exceptions.html#SystemExit) instead because:

1. they do the same thing and have the same interface
2. `sys.exit` requires an additional import (`import sys`)
3. `sys.exit` raises `SystemExit` without being explicit about the fact it raises an exception

You can exit with a status code, such as `raise SystemExit(0)` or `raise SystemExit(1)`. If you want to print an error message to stderr and then exit with status code 1, you can use `raise SystemExit("your error message here")`.

The `SystemExit` exception is derived from `BaseException`, not `Exception`, so the common `try: ...; except Exception: ...` will not catch it (which is good).

Exiting by raising an exception is important for many reasons including making code easier to test. However, there are rare cases where a program should be exited immediately without raising an exception. In those cases, you can use the [`os._exit`](https://docs.python.org/3/library/os.html#os._exit) function.

## CLI parsing libraries

CLI parsing libraries often have their own functions or exceptions for exiting. Some of them print extra info such as the CLI's usage message.

- [argparse's exit functions](https://docs.python.org/3/library/argparse.html#exiting-methods)
- [click's exit exceptions](https://click.palletsprojects.com/en/stable/api/#exceptions)

> [!warning]
> Click's exit exceptions are derived from `Exception`.

### click.ClickException

`raise click.ClickException("your error message here")`

result:

```
Error: your error message here
```

### click.BadOptionUsage

`raise click.BadOptionUsage("option_name_here", "your error message here")`

result:

```
Usage: deploy [OPTIONS]
Try 'deploy --help' for help.

Error: your error message here
```

### click.BadArgumentUsage

`raise click.BadArgumentUsage("your error message here")`

result:

```
Usage: deploy [OPTIONS]
Try 'deploy --help' for help.

Error: your error message here
```

Click has a few other exception types that you probably won't use directly.
