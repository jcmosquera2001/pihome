# pihome

Actualización del sistema:
apt update
apt upgrade

Instalación de Docker:

curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

sudo usermod -aG docker $USER

Instalación de Docker Compose:

apt install docker-compose

Instalación de Portainer:

docker volume create portainer_data

docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest
    

Creación de carpetas para las configuraciones de los containers:

sudo mkdir /docker
sudo chown root.docker /docker
sudo chmod 774 /docker


Estructura del stack Docker Compose:

```yaml
version: '3'
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - /PATH_TO_YOUR_CONFIG:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
    
  node-red:
    image: nodered/node-red:latest
    environment:
      - TZ=Europe/Amsterdam
    ports:
      - "1880:1880"
    volumes:
      - /PATH_TO_YOUR_CONFIG:/data
```