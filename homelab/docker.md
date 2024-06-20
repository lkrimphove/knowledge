---
title: docker
draft: false
tags:
  - homelab
  - server
  - docker
  - docker-compose
---
# Docker

## Install Docker Engine

1. Set up Docker's `apt` repository
```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install the Docker packages.
```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. Verify that the Docker Engine installation is successful by running the `hello-world` image. (optional)
```shell
sudo docker run hello-world
```

4. Create the `docker` group.
```bash
sudo groupadd docker
```

5. Add your user to the `docker` group.
```bash
sudo usermod -aG docker $USER
```

6. Log out and log back in so that your group membership is re-evaluated.

> [!tip]
> You can also run the following command to activate the changes to groups:
> ```bash
> newgrp docker
> ```

7. Verify that you can run `docker` commands without `sudo`. (optional)
```bash
docker run hello-world
```

8. Configure Docker to start on boot with `systemd`.
```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```


## Configure Log Rotation

```bash
sudo nano /etc/docker/daemon.json
```

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```


## Install Docker Compose

```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin
```


---
## Resources
- [Docker](https://docs.docker.com/)
- [Install Docker](https://docs.docker.com/engine/install/ubuntu/)
- [Post Installation](https://docs.docker.com/engine/install/linux-postinstall/)
- [Log Rotation](https://docs.docker.com/config/containers/logging/json-file/)
- [Docker Compose](https://docs.docker.com/compose/install/linux/)