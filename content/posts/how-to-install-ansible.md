+++
title = 'How to install Ansible'
date = 2026-02-07T21:00:00-08:00
tags = []
+++

Here's a way to install Ansible that's easier than [the official instructions](https://docs.ansible.com/projects/ansible/latest/installation_guide/index.html).

`uv tool install ansible-core` (requires [uv](https://docs.astral.sh/uv/))

That's it. You should now be able to use commands such as `ansible`, `ansible-playbook`, etc.

I don't know why the official instructions have so many suggestions that just end in error messages on most systems.
