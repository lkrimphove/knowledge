# Installation
1. Update and install [ufw](ufw.md) (optional) and curl
```shell
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y ufw curl
```

2. Install docker
```shell
curl -fsSL https://get.docker.com -o get-docker.sh sudo sh get-docker.sh sudo usermod -aG docker $USER su - $USER
```

3. Install docker-compose
```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

4. [Initial hardening with ufw](ufw.md#Initial%20hardening) (optional)