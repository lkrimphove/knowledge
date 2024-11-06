---
title: Soft-Serve
draft: true
tags:
  - homelab
  - git
  - docker-compose
---
# Soft-Serve

- self-hostable Git server for the command line

## docker-compose.yml
```yml
services:
  soft-serve:
    image: charmcli/soft-serve:latest
    container_name: soft-serve
    volumes:
      - ./data:/soft-serve
    ports:
      - 23231:23231
      - 23232:23232
      - 23233:23233
      - 9418:9418
	environment:
      - SOFT_SERVE_INITIAL_ADMIN_KEYS='ssh-ed25519 AAAAC3N...'
    restart: unless-stopped
```
[more info](https://github.com/charmbracelet/soft-serve/blob/main/docker.md)

> [!warning]
> Be sure to add `SOFT_SERVE_INITIAL_ADMIN_KEYS` to the environment variables. Otherwise you will have to delete the contents of the data folder (you can keep the `config.yaml`) and recreate the container.

---

## Resources
- https://github.com/charmbracelet/soft-serve
- 