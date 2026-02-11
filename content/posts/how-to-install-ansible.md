+++
title = 'How to install Ansible'
date = 2026-02-07T21:00:00-08:00
tags = []
+++

Here's a way to install Ansible that's easier and more likely to work than [the official instructions](https://docs.ansible.com/projects/ansible/latest/installation_guide/index.html).

1. Install [uv](https://docs.astral.sh/uv/) if you haven't already
2. `uv tool install ansible-core`

That's it. You should now be able to use commands such as `ansible`, `ansible-playbook`, etc.
