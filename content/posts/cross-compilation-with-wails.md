+++
title = "Cross-compilation with Wails"
date = 2025-12-10T16:49:19-08:00
tags = []
+++

On 2025-12-9, I tested the cross-compilation of the official Svelte template of [Wails](https://github.com/wailsapp/wails) v2.11.0, a desktop app framework for Go, on my amd64 laptop running Ubuntu 24 with Go v1.25.5. Of course, the results may change in the future as Wails and Go are being improved. Go is known for making cross-compilation much easier than most other languages, and that is mostly still true when using frameworks like Wails. Creating executable files and/or installers for each of several platforms is sometimes as simple as listing those platforms in a command:

    wails build -clean -nsis \
        -platform 'linux/amd64,windows/amd64,windows/arm64,windows/386'

The above command creates these files:

```
build
└── bin
    ├── my-app-386.exe
    ├── my-app-amd64.exe
    ├── my-app-amd64-installer.exe
    ├── my-app-arm64.exe
    ├── my-app-arm64-installer.exe
    └── my-app-linux-amd64
```

With that, we already have self-contained executables and installers for Linux and Windows! I didn't include MacOS in the list of platforms because Apple generally does not allow cross-compilation. I don't think there's anything Wails or Go can do about that. (When I include darwin in the list, the platform is skipped with the message `WARNING  Crosscompiling to Mac not currently supported`.)

When I accidentally included a platform that doesn't exist, I got a helpful message that included this:

    Supported platforms: darwin,darwin/amd64,darwin/arm64,darwin/universal,linux,linux/amd64,linux/arm64,linux/arm,windows,windows/amd64,windows/arm64,windows/386

## What about `linux/arm64`?

Unfortunately, cross-compiling from `linux/amd64` to `linux/arm64` doesn't work right now (see output below). I searched Wails' repo and found others have the same problem. There doesn't seem to be a workaround. Here's the output:

```
$ wails build -platform 'linux/arm64'
Wails CLI v2.11.0


# Build Options

Platform(s)        | linux/arm64
Compiler           | /usr/local/go/bin/go
Skip Bindings      | false
Build Mode         | production
Devtools           | false
Frontend Directory | /home/chris/repos/my-app/frontend
Obfuscated         | false
Skip Frontend      | false
Compress           | false
Package            | true
Clean Bin Dir      | false
LDFlags            |
Tags               | []
Race Detector      | false


# Building target: linux/arm64

  • Generating bindings: Done.
  • Installing frontend dependencies: Done.
  • Compiling frontend: Done.
  • Compiling application: # runtime/cgo
gcc_arm64.S: Assembler messages:
gcc_arm64.S:30: Error: no such instruction: `stp x29,x30,[sp,'
gcc_arm64.S:34: Error: operand size mismatch for `mov'
gcc_arm64.S:36: Error: no such instruction: `stp x19,x20,[sp,'
gcc_arm64.S:39: Error: no such instruction: `stp x21,x22,[sp,'
gcc_arm64.S:42: Error: no such instruction: `stp x23,x24,[sp,'
gcc_arm64.S:45: Error: no such instruction: `stp x25,x26,[sp,'
gcc_arm64.S:48: Error: no such instruction: `stp x27,x28,[sp,'
gcc_arm64.S:52: Error: operand size mismatch for `mov'
gcc_arm64.S:53: Error: operand size mismatch for `mov'
gcc_arm64.S:54: Error: operand size mismatch for `mov'
gcc_arm64.S:56: Error: no such instruction: `blr x20'
gcc_arm64.S:57: Error: no such instruction: `blr x19'
gcc_arm64.S:59: Error: no such instruction: `ldp x27,x28,[sp,'
gcc_arm64.S:62: Error: no such instruction: `ldp x25,x26,[sp,'
gcc_arm64.S:65: Error: no such instruction: `ldp x23,x24,[sp,'
gcc_arm64.S:68: Error: no such instruction: `ldp x21,x22,[sp,'
gcc_arm64.S:71: Error: no such instruction: `ldp x19,x20,[sp,'
gcc_arm64.S:74: Error: no such instruction: `ldp x29,x30,[sp],'
  ERROR   exit status 1
```

I get the same error message when I use the `-compiler` option with `go1.24.11` or `go1.23.12`. (You can [install multiple versions of Go](https://go.dev/doc/manage-install).)

## The `dAppServer/wails-build-action` GitHub Action

Wails' website includes [a sample GitHub action for building executables](https://wails.io/docs/guides/crossplatform-build/) which uses the `dAppServer/wails-build-action` action. I tested the sample with its actions updated to their latest stable versions.

A few things that may or may not make someone hesitant to use `dAppServer/wails-build-action`:

- the sample on Wails' site fails with the error message `ditto: Cannot get the real path for source './build/bin/App.app'`
- fixes for that bug have been submitted to the `dAppServer/wails-build-action` repo in pull requests, but they have been ignored for 5 months and counting (example: https://github.com/Snider/build/pull/49/files)
- it has a copyleft license
- much of the project's documentation was written by an LLM

The error message mentioned above appears if you keep `darwin/universal` in the list of platforms to target. (The Linux and Windows builds succeeded when I tested it.)

## GoReleaser

[GoReleaser](https://goreleaser.com/) is an awesome tool that automates releases, including creating executables and installers. I couldn't get Wails to work with it though.

Here's part of the `.goreleaser.yaml` I tried:

```yaml
builds:
  - id: "wails-build"
    goos:
      - linux
      - windows
    tool: "go1.23.12"
      # how: https://go.dev/doc/manage-install
      # why: https://github.com/wailsapp/wails/issues/4605
    hooks:
      post:
        - wails build -clean -nsis -compiler 'go1.23.12' -platform 'linux/amd64,linux/arm64,windows/amd64,windows/arm64,windows/386'
        - cp ./build/bin/. ./dist
```

I then tested this with `goreleaser release --snapshot --clean` and got this error:

```
  ⨯ release failed after 6s
    error=
    │ post hook failed: shell: 'wails build -clean -nsis -compiler 'go1.23.12' -platform linux/amd64,linux/arm64,windows/amd64,windows/arm64,windows/386': exit status 1: Wails CLI v2.11.0
    │ # Build Options
    │
    │ Platform(s)        | linux/amd64,linux/arm64,windows/amd64,windows/arm64,windows/386
    │ Compiler           | /home/chris/go/bin/go1.23.12
    │ Skip Bindings      | false
    │ Build Mode         | production
    │ Devtools           | false
    │ Frontend Directory | /home/chris/repos/my-app/frontend
    │ Obfuscated         | false
    │ Skip Frontend      | false
    │ Compress           | false
    │ Package            | true
    │ Clean Bin Dir      | true
    │ LDFlags            |
    │ Tags               | []
    │ Race Detector      | false
    │ # Building target: linux/amd64
    │
    │   • Generating bindings:   ERROR
    │
    │           fork/exec /tmp/wailsbindings: no such file or directory
    │   ERROR
    │
    │           fork/exec /tmp/wailsbindings: no such file or directory
```

I got the same error with `go1.24.11`, and with using Go v1.25.5 by not specifying the `tool` and `compiler`. I searched Wails' repo for any issues, PRs, etc. that might help with this, but I couldn't find anything promising. There might be a way to get it working, but I might just create releases manually for now.

Just in case the error only occurs when I run `goreleaser release --snapshot --clean` and not when it runs in GitHub Actions, I tried the latter as well. The error message changed, but still doesn't seem to have a workaround:

```
  ⨯ release failed after 5s                         
    error=
Error:     │ build failed: exit status 1: main.go:11:12: pattern all:frontend/dist: no matching files found
    target=linux_386_sse2
Error: The process '/opt/hostedtoolcache/goreleaser-action/2.13.1/x64/goreleaser' failed with exit code 1
```
