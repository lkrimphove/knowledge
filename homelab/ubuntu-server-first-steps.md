# Guide - First Steps On Ubuntu Server

> [!info]
> Most of this will also work with any other Linux distro


## Perform System Updates

```bash
sudo apt update && sudo apt upgrade
```


## Set the Timezone

```bash
timedatectl set-timezone 'Europe/Berlin'
```

> [!tip]
> Use `timedatectl` to list all available timezones
> ```bash
> timedatectl list-timezones
> ```
> Use the arrow keys, `Page Up`, and `Page Down` to navigate through the list. Copy or make note of your desired time zone and press **q** to exit the list.

> [!tip]
> Use `date` to view the current date and time according to your server and to verify your settings
> ```
> root@localhost:~# date
> Thu Feb 16 12:17:52 EST 2018
> ```


## Configure a Custom Hostname

```bash
hostnamectl set-hostname example-hostname
```

## Update `hosts`file

The `hosts` file creates static associations between IP addresses and hostnames or domains which the system prioritizes before DNS for name resolution.

[Guide](https://www.linode.com/docs/products/compute/compute-instances/guides/set-up-and-secure/?tabs=linux%2Cmost-distributions%2Cubuntu-debian-kali-linux#update-your-systems-hosts-file)


## Add user

```bash
sudo adduser example_user
sudo adduser example_user sudo
```

Exit and log in as the new user.

## Harden SSH

Upload the SSH key to your target server.

```bash
ssh-copy-id example_user@192.0.2.17
```

> [!tip]
> If you don't already have a SSH key, generate one using `ssh-keygen`.

Connect to your remote server and set permissions for the public key directory and the key file itself.

```bash
sudo chmod -R 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

Try exiting and connecting to the server. You should be able to connect to the server without entering a password.


## SSH Daemon Options

```bash
sudo nano /etc/ssh/sshd_config
```

Change the corresponding settings.

```aconf
# Authorization
...
PermitRootLogin no

# Change to no to disable tunnelled clear text passwords
PasswordAuthentication no
```
	
Restart the SSH service.

```bash
systemctl enable --now ssh.service
```


## Next steps
- [[ufw]]
- [[docker]]


---
## Resources
- [Guides - Setting Up and Securing a Compute Instance](https://www.linode.com/docs/products/compute/compute-instances/guides/set-up-and-secure/)


## Tags
#guide #homelab #server