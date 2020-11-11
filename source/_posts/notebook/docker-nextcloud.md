# Docker-NextCloud


## docker-compose.yml
```
version: '3'

services:
  nextcloud:
    image: nextcloud
    restart: "no"
    volumes:
      - ~/docker_nextcloud/data/:/var/www/html
    ports:
      - "25000:80"
```