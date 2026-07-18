+++
title = 'Running terminal commands with Python'
date = 2026-07-18T11:37:28-07:00
tags = []
+++

Running a terminal command with Python usually looks like this:

```py
import subprocess

subprocess.run(["git", "init"], check=True)
```

Using `check=True` will make `subprocess.run` throw an exception if the command fails. If you don't use `check=True`, you should probably check [the return code](https://docs.python.org/3/library/subprocess.html#subprocess.CompletedProcess) yourself or the command could fail silently.

You can catch errors like this:

```py
try:
    subprocess.run(["git", "init"], check=True)
except subprocess.CalledProcessError as err:
    print(f"git init failed with return code {err.returncode}")
```

To get any error messages that may be in stderr, you can use `capture_output=True` and `text=True`.

```py
try:
    subprocess.run(["git", "init"], check=True, capture_output=True, text=True)
except subprocess.CalledProcessError as err:
    print(f"git init failed with return code {err.returncode}")
    assert isinstance(err.stderr, str)
    print(err.stderr)
```

Also use those two options if you want stdout:

```py
result = subprocess.run(
    ["git", "remote"],
    check=True,
    capture_output=True,
    text=True,
)
assert isinstance(result.stdout, str)
print(f"The git remote(s) are: {result.stdout}")
```

If you want to neither capture stdout & stderr nor have them shown in the terminal, you can send them to /dev/null if the command doesn't have a `--quiet` flag.

```py
def is_docker_daemon_running() -> bool:
    try:
        subprocess.run(
            ["docker", "info"],
            check=True,
            stdout=subprocess.DEVNULL,
            stderr=subprocess.DEVNULL,
        )
        return True
    except subprocess.CalledProcessError:
        return False
```

Of course, you can use variables in the command. If you don't use `shell=True`, you're safe from shell injection attacks because the shell is bypassed.

```py
print(f"Creating a {visibility} GitHub repository")
subprocess.run(
    [
        "gh",
        "repo",
        "create",
        repo_name,
        f"--{visibility}",
        "--source=.",
        f"--description={repo_description}",
        "--push",
    ],
    check=True,
)
```

> this library will not implicitly choose to call a system shell. This means that all characters, including shell metacharacters, can safely be passed to child processes. If the shell is invoked explicitly, via shell=True, it is the application’s responsibility to ensure that all whitespace and metacharacters are quoted appropriately to avoid [shell injection](https://en.wikipedia.org/wiki/Shell_injection#Shell_injection) vulnerabilities. On [some platforms](https://docs.python.org/3/library/shlex.html#shlex-quote-warning), it is possible to use [shlex.quote()](https://docs.python.org/3/library/shlex.html#shlex.quote "shlex.quote") for this escaping.
> 
> On Windows, batch files (\*.bat or \*.cmd) may be launched by the operating system in a system shell regardless of the arguments passed to this library. This could result in arguments being parsed according to shell rules, but without any escaping added by Python. If you are intentionally launching a batch file with arguments from untrusted sources, consider passing shell=True to allow Python to escape special characters. See [gh-114539](https://github.com/python/cpython/issues/114539) for additional discussion.
> 
> — [subprocess — Subprocess management — Python 3.14.6 documentation](https://docs.python.org/3/library/subprocess.html#subprocess-security:~:text=this%20library%20will,for%20additional%20discussion.)

If you do use `shell=True`, put the entire command in one string instead of a list of strings.

Any command that might get stuck should probably use a timeout. The `timeout` parameter is in seconds and can take an int or a float.

```py
try:
    result = subprocess.run(
        ["restic", "snapshots", "--json", "--latest", "1"],
        check=True,
        capture_output=True,
        text=True,
        timeout=10,  # seconds
        env={"HOME": os.environ["HOME"]},
    )
except subprocess.TimeoutExpired:
    print(
        "restic snapshots timed out. Maybe resticprofile made restic "
        "cache files with the wrong owner?"
    )
```

Notice in the example above how you can pass a dictionary to the `env` parameter when a command needs environment variables.

The subprocess module is great, but sometimes Python has an easier way to do things. For example, to check whether a command exists or get its executable's path, you can use `shutil.which`:

```py
import shutil
import subprocess

if shutil.which("brew"):
    subprocess.run(["brew", "upgrade"], check=True)

gh_exe_path: str = shutil.which("gh")
```

That covers most use cases of subprocess, but there's so much more you can do. Further reading:

- [subprocess — Subprocess management](https://docs.python.org/3/library/subprocess.html)
- [shutil — High-level file operations](https://docs.python.org/3/library/shutil.html)
