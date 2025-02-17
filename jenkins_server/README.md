# Setup your own Jenkins Server

- Type: single node setup i.e. no agent (worker) nodes

## Instance Configuration

- Image: `Ubuntu 22.04 LTS`
- Size: `t3.micro`
- VPC: `tech501-sameem-2-subnet-vpc-vpc`
  - Subnet: `tech501-sameem-2-subnet-vpc-subnet-public1-eu-west-1a`
- NSG rules: allow ssh and http, local machine access only

## Installation Commands for Debian/Ubuntu

### Java Installation

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
openjdk version "17.0.13" 2024-10-15
OpenJDK Runtime Environment (build 17.0.13+11-Debian-2)
OpenJDK 64-Bit Server VM (build 17.0.13+11-Debian-2, mixed mode, sharing)
```

### Jenkins Installation

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

## Configure Nginx Reverse Proxy

- Create new nginx jenkins site.

```bash
`sudo nano /etc/nginx/sites-available/jenkins`
```

- Add the below:

```bash
server {
    listen 80;
    server_name your-public-ip-or-domain;

    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}
```

- Enable the configuration.

```bash
sudo ln -s /etc/nginx/sites-available/jenkins /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
```

- Restart nginx.

```bash
sudo systemctl restart nginx
```

- Should be able to access nginx at default port 80 now i.e. without specifying 8080.

## Get Started Commands

- start at boot: `sudo systemctl enable jenkins`
- start service: `sudo systemctl start jenkins`
- check status: `sudo systemctl status jenkins`

## Post Installation Wizard

- When you first access a new Jenkins controller, you are asked to unlock it using an automatically-generated password, stored in: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

## Plugins installation

- Have chosen `Install suggested plugins` for now.

## Create first admin user

- Post plugins page, will ask you to create first admin user.
- Enter username and password etc.
- Will be able to start using Jenkins after this!
