# server-config
Config files for server

## Deployment step

### 0. SSH to the machine
```bash
ssh coeusadmin@coeus.sit.kmutt.ac.th
```
then type the password

### 1. install [docker](https://docs.docker.com/engine/install/ubuntu/)

1.1. remove old docker engine.
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
1.2. set up apt repository
```bash
sudo apt-get update
```
```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    git
```
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

1.3. install docker engine
```bash
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
 ```
 in case of specific version
 ```bash
 sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
 ```
 
### 2. install [docker-compose](https://docs.docker.com/compose/install/)
 2.1. download the compose
 ```bash
 sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 ```
 2.2. make compose exceutable
 ```bash
 sudo chmod +x /usr/local/bin/docker-compose
 ```
 2.3. verify the installation
  ```bash
 docker-compose --version
 ```
### 3. nginx 
3.1. install nginx
```bash
sudo apt update
sudo apt install nginx
```
3.2. allow firewall
```bash
sudo ufw allow 'Nginx Full'
```

3.3. write [config](https://github.com/cs-coeus/server-config)
```bash
git clone https://github.com/cs-coeus/server-config.git
```

```bash
sudo cp server-config/coeus /etc/nginx/sites-available/coeus
```

copy config from coeus file from the link above

3.4. link config to sites-enable
```bash
sudo ln -s /etc/nginx/sites-available/coeus  /etc/nginx/sites-enabled/coeus
```

3.5. remove default config
```bash
sudo rm /etc/nginx/sites-available/default
sudo rm /etc/nginx/sites-enabled/default
```


### 4. deploy docker container using [compose](https://github.com/cs-coeus/server-config/blob/main/docker-compose.yml)
4.1. change directory
```bash
cd server-config
```
4.2. deploy
```bash
sudo docker-compose up -d
```

### 5. start nginx service
5.1. restart nginx
```bash
sudo systemctl restart nginx
```
