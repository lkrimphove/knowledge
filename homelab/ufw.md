---
title: UFW
draft: false
tags:
  - guide
  - homelab
  - server
  - firewall
---
# UFW

> [!warning]
> If you are running Docker, by default Docker directly manipulates iptables. Any UFW rules that you specify do not apply to Docker containers.


## Install UFW

```bash
sudo apt-get install ufw
```


## Update rules

```shell
# Deny all non-explicitly allowed ports
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH access
sudo ufw allow ssh

# Enable UFW
sudo ufw enable
```


---
## Resources
- [UFW](https://wiki.ubuntu.com/UncomplicatedFirewall)
- [Guides - How to Configure a Firewall with UFW](https://www.linode.com/docs/guides/configure-firewall-with-ufw/)