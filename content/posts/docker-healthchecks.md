+++
title = 'Docker healthchecks'
date = 2026-06-02T22:58:12-07:00
tags = []
+++

Whether a Docker container is healthy is not just about whether it's running. A service could stop working properly without crashing. Docker can detect some of those kinds of invalid states when we define custom healthchecks.

- [`healthcheck` in Docker Compose \| Docker Docs](https://docs.docker.com/reference/compose-file/services/#healthcheck)
- [`HEALTHCHECK` in Dockerfile \| Docker Docs](https://docs.docker.com/reference/dockerfile/#healthcheck)

The way it works is you define a command for Docker to periodically run inside the container, and Docker uses that command's exit code to determine whether the service is healthy.

> The command's exit status indicates the health status of the container. The possible values are:
> 
> - 0: success - the container is healthy and ready for use
> - 1: unhealthy - the container isn't working correctly
> - 2: reserved - don't use this exit code
> 
> — [Dockerfile reference \| Docker Docs](https://docs.docker.com/reference/dockerfile/#healthcheck:~:text=The%20command%27s%20exit,this%20exit%20code)

Many healthchecks use a command like `["CMD", "curl", "-f", "http://localhost:3000/health"]`, where the `-f` flag means "fail fast with no output at all on server errors" (source: curl's man page). Any valid command could be used though. Some projects include a custom command for checking health, such as [Beszel](https://www.beszel.dev/): `["CMD", "/beszel", "health", "--url", "http://localhost:8090"]`.

A Docker healthcheck is only as useful as its command. If the command doesn't accurately report the service's health, Docker won't either. There's a great post that covers this topic and more: [Docker healthcheck best practices: what it actually measures \| Juanchi.dev](https://juanchi.dev/en/blog/docker-healthcheck-what-it-measures-best-practices). One of its main points is that a healthcheck should usually measure _readiness_, not _liveness_. In other words, the healthcheck should try to measure whether the service can respond correctly, not just whether it's running.
