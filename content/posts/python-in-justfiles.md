+++
title = 'Python in Justfiles'
date = 2025-09-30T23:41:21-07:00
tags = []
+++

[Just](https://just.systems/) is a tool for defining and running custom commands for specific projects. Justfiles are often compared favorably to makefiles. If you're new to Just, check out [Just Programmer's Manual](https://just.systems/man/en/).

A command definition in a justfile (a recipe) usually contains code written in the system's default shell language, but Just lets you choose from many other languages including Python! This is especially great because it allows more recipes to be cross-platform. Recipes can use other languages by defining `script-interpreter` or by using a shebang as explained in [Python Recipes with uv](https://just.systems/man/en/python-recipes-with-uv.html).

For example, I recently wrote a recipe that runs a command either inside or outside a Docker container. (I ended up changing it into two simpler recipes, but this is still a good demo of what is possible with Just.) The user of this recipe chooses whether to run the command inside or outside a container by entering the names of the Docker compose files used to create the container.

```just
container_name := "my_container_name"

# Apply all unapplied migrations
[group('migrations')]
up *compose_files:
    #!/usr/bin/env -S uv run --script
    import subprocess
    compose_files: str = "{{compose_files}}"
    if compose_files:
        compose_files = "-f " + " -f ".join(compose_files.split())
        cmd = f"docker compose {compose_files} exec -T {{container_name}} uv run db.py up"
    else:
        cmd = f"uv run db.py up"
    subprocess.run([cmd], check=True, shell=True)
```

The recipe receives the `compose_files` argument in the same way a non-shebang recipe would: `{{compose_files}}` is replaced with the recipe's input, then Just puts the Python into a temporary file and runs it as a script. This means you can put anything in the recipe that you could into a normal script, such as [Inline script metadata](https://packaging.python.org/en/latest/specifications/inline-script-metadata/) that defines third-party dependencies.

The shebang looks like it wouldn't work correctly in Windows, but it more or less does work because of how Just processes it. See [Shebang Recipes](https://just.systems/man/en/shebang-recipes.html) and [Script Recipes](https://just.systems/man/en/script-recipes.html) for more details.
