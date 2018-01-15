# OpenHAB Docker Template

## Contains

- OpenHAB
- Homegear (BidCoS Module)
- mySQL-Persistance

## Requirements

- Docker
- Docker-Compose

## Install

```
git clone https://github.com/MarioWi/openhab-docker.git openhab-docker
cd openhab-docker
docker-compose up -d
```
###Required configuration (best bevor first run)


- OpenHAB:  http://[ip]:8080/
            https://[ip]:8443
- Node-Red: http://[ip]:1880/

## Defaults

- OpenHab-WEB:      8080
- OpenHab-WEB SSL:  8443
- mySQL-Port:       3306
- mySQL-User:       openhabuser
- mySQL-Password:   abcD1234!
