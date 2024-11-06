---
title: SSH
draft: true
tags:
  - security
---
# SSH

## Key Generation

```bash
ssh-keygen -t rsa
```

## Copy Key to Host
```bash
ssh-copy-id user@hostname.example.com
```

Useful if you only have the pub file:
```bash
cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'cat >> .ssh/authorized_keys && echo "Key copied"'
```

## Config
Store host configuration in `.ssh/config`:
```ini
Host marteria
    HostName marteria.example.com
    User marten

Host casper
    HostName 192.168.1.10
    User ben
    Port 2022
    IdentityFile ~/.ssh/casper.key
```
- use `ssh <host-name>` to access host
- works wonderfully with [Wishlist](https://github.com/charmbracelet/wishlist)

---

## Resources
- https://docs.oracle.com/en/cloud/cloud-at-customer/occ-get-started/generate-ssh-key-pair.html
- https://linuxize.com/post/using-the-ssh-config-file/