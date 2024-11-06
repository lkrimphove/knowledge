---
title: vaultwarden
draft: false
tags:
  - homelab
  - tools
  - docker-compose
---
# Vaultwarden

- Unofficial [[bitwarden]] compatible server written in Rust, formerly known as bitwarden_rs

## docker-compose.yml
```yml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    environment:
      # DOMAIN: "https://vaultwarden.example.com"  # required when using a reverse proxy; your domain; vaultwarden needs to know it's https to work properly with attachments
      SIGNUPS_ALLOWED: "true" # Deactivate this with "false" after you have created your account so that no strangers can register
    volumes:
      - ./vw-data:/data # the path before the : can be changed
    ports:
      - 11001:80
```
[more info](https://github.com/dani-garcia/vaultwarden/wiki/Using-Docker-Compose)

---

## Resources
- https://github.com/dani-garcia/vaultwarden
- https://github.com/dani-garcia/vaultwarden/wiki
- https://bitwarden.com/