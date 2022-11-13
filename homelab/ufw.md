# Initial hardening
```shell
# Deny all non-explicitly allowed ports
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH access
sudo ufw allow ssh

# Enable UFW
sudo ufw enable
```

# Tags
#homelab #server #firewall