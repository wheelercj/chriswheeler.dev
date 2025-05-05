+++
title = 'Types of secrets management'
date = 2025-05-05T01:07:06-07:00
tags = []
+++

When creating software, it's common to deal with secrets like database passwords, API access tokens, etc. There are many ways to make secrets available to software. Below are notes on some of the ways. [Threat Modeling](https://owasp.org/www-community/Threat_Modeling) might help you determine which way is best for what you are doing.

## Environment variables

Putting secrets in environment variables (env vars) is very easy and might be fine if your setup is simple and otherwise very secure. It's certainly better than putting secrets directly into source code. However, it's important to know what the alternatives are and when to use them, which is covered by the other sections of this post.

If you're using Docker and any `.env` files or other secrets files, you should probably have a `.dockerignore` that makes Docker ignore them. Definitely don't define any secret env vars in `Dockerfile` because that would include them in the image (images should be safe to share). Linux users should probably restrict `.env` file permissions with `chmod 600 .env` to make the file only accessible to the file's owner.

- [Chmod Command in Linux (File Permissions) \| Linuxize](https://linuxize.com/post/chmod-command-in-linux/)

### Encryption

`.env` files are usually plaintext, but there are many tools that can encrypt them for you. Encrypting `.env` files or other key-value files is mainly helpful for simple CI/CD pipelines that involve committing the encrypted files into Git to send them to a production server that automatically decrypts the files and redeploys. [SOPS](https://github.com/getsops/sops) is probably the most widely used tool for this kind of pipeline, and it supports many more types of key-value files than just `.env`. Encrypting `.env` files that are just staying in your secure development environment is generally not worth the time and effort.

## DevOps tools

Tools like Docker, Ansible, and Terraform each have multiple features for helping keep secrets safe. For example, Docker has [Docker Secrets](https://docs.docker.com/compose/how-tos/use-secrets/) and Ansible has [Ansible Vault](https://www.redhat.com/en/blog/ansible-playbooks-secrets).

### Docker Secrets

Docker recommends using Docker Secrets because:

> Environment variables are often available to all processes, and it can be difficult to track access. They can also be printed in logs when debugging errors without your knowledge. Using secrets mitigates these risks.
> 
> — [Secrets in Compose \| Docker Docs](https://docs.docker.com/compose/how-tos/use-secrets/#:~:text=environment%20variables%20are%20often%20available%20to%20all%20processes%2C%20and%20it%20can%20be%20difficult%20to%20track%20access.%20they%20can%20also%20be%20printed%20in%20logs%20when%20debugging%20errors%20without%20your%20knowledge.%20using%20secrets%20mitigates%20these%20risks.)

> environment variables can unintentionally be leaked between containers (for instance, if you use `--link`).
> 
> — [Manage sensitive data with Docker secrets \| Docker Docs](https://docs.docker.com/engine/swarm/secrets/#:~:text=environment%20variables%20can%20unintentionally%20be%20leaked%20between%20containers%20%28for%20instance%2C%20if%20you%20use%20%2D%2Dlink%29.)

Docker Secrets is mainly designed for use with Docker Swarm. If you use Docker Secrets with Docker Compose, you will have to put each secret in a plaintext file outside the container. This is still more secure than putting secrets in unencrypted env vars.

Remember to add any secrets files that are in a project folder to `.gitignore` and `.dockerignore`. Linux users should probably `chmod 600 secrets/*` to restrict access to them.

- [Introduction to Docker Secrets \| Baeldung on Ops](https://www.baeldung.com/ops/docker-secrets)
- [I don't understand Docker Secrets. How am I more protected? : r/docker](https://www.reddit.com/r/docker/comments/1dl5hcr/i_dont_understand_docker_secrets_how_am_i_more/)
- [Protecting sensitive data with Ansible vault — Ansible Community Documentation](https://docs.ansible.com/ansible/latest/vault_guide/index.html)
- [What is container orchestration?](https://www.redhat.com/en/topics/containers/what-is-container-orchestration)
- [Postgres' Docker image can use Docker Secrets directly](https://github.com/docker-library/docs/blob/master/postgres/README.md#docker-secrets)
- [Ansible vs. Docker \| Pure Storage Blog](https://blog.purestorage.com/purely-educational/ansible-vs-docker/)

## Secrets managers

Secrets managers are services that protect secrets with many features including access control, versioning, auditing, and automatic rotation. Software usually gets secrets from a secrets manager by making web requests rather than by reading environment variables or files. Secrets managers are great for businesses with many employees, but are generally considered to be completely overkill and too expensive for use in secure development environments and locally hosted services.

[Discussion on the purpose of secrets managers](https://www.reddit.com/r/devops/comments/8h5fml/hashicorp_vault_explanation/)

There are many secrets managers. Many of the major cloud platforms have their own, such as:

- [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
  - [Boto3: the official Python AWS SDK](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/secrets-manager.html)
- [Google Cloud Secret Manager](https://cloud.google.com/security/products/secret-manager?hl=en)
- [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/)

There are also secrets managers that are not locked in to a specific cloud platform, including:

- [HashiCorp Vault](https://developer.hashicorp.com/vault)
  - [Lessons Learned Using Vault As A Secret Store · malgregator](https://www.malgregator.com/post/lessons-learned-using-vault-secret-store/)
  - [HVAC: Python 3.X client for HashiCorp Vault](https://github.com/hvac/hvac)
- [Infisical](https://github.com/Infisical/infisical)
- [Bitwarden Secrets Manager](https://bitwarden.com/products/secrets-manager/), not to be confused with their password manager
- [OpenBao](https://github.com/openbao/openbao)
- [CyberArk Conjur](https://github.com/cyberark/conjur)

### Secrets manager comparisons

- [AWS Secrets Manager vs HashiCorp Vault ⦋2025⦌](https://infisical.com/blog/aws-secrets-manager-vs-hashicorp-vault)
- [Any Hashicorp Vault selhosted here ? : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/m71aji/any_hashicorp_vault_selhosted_here/)
- [HashiCorp Vault vs AWS Secrets Manager : r/devops](https://www.reddit.com/r/devops/comments/127tqjf/hashicorp_vault_vs_aws_secrets_manager/)
- [Anyone using Infisical? : r/devops](https://www.reddit.com/r/devops/comments/1blxy9u/anyone_using_infisical/)
- [Self-hosted Secrets Manager (or something alike) : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/12zb3m9/selfhosted_secrets_manager_or_something_alike/)
- [Self-Hosted Secrets Management Service Recommendations? : r/devops](https://www.reddit.com/r/devops/comments/10a7hmd/selfhosted_secrets_management_service/)

## Operating System (OS) keystores

Each major OS has a built-in way to securely store secrets. They have different interfaces, but there are libraries that abstract them into one interface.

- [99designs/keyring: Go library providing a uniform interface across a range of secure credential stores](https://github.com/99designs/keyring)
- [zalando/go-keyring: Cross-platform keyring interface for Go](https://github.com/zalando/go-keyring)
- [jaraco/keyring: The Python keyring library](https://github.com/jaraco/keyring)
- [search for more](https://github.com/search?q=keyring&type=repositories)

## Hardware

- [Hardware security module - Wikipedia](https://en.wikipedia.org/wiki/Hardware_security_module)

## Further reading

- [secret-management · GitHub Topics](https://github.com/topics/secret-management)
- [Best practices for storing API keys and passwords securely in Golang? : r/golang](https://www.reddit.com/r/golang/comments/12pg11w/best_practices_for_storing_api_keys_and_passwords/)

This post focused on getting secrets into running software, but there's so much more to the topic of secrets management. For example, another big part of it is scanning for leaked secrets.
