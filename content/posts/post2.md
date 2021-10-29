---
title: "SSL Proxy with Docker"
layout: "post"
#url: "/archives/"
summary: Nginx-SSL Proxy for my Own-Webserver
date: "2021-07-01T14:43:40"
---

For hosting any Web-Application on my Home-Server I've setup a SSL Proxy to encrypt all traffic.

The whole environement is built with Docker-Containers to easly move ot to an other machine.

The Nginx-proxy detects every container startet in the ```frontend``` network with the variables ```VIRTUAL_HOST``` and ```LETSENCRYPT_HOST```.
The TLS Certificates are issued by Lets Encrypt and automatically renewed by Certbot.

```yml
version: '2'
services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./certs:/etc/nginx/certs:ro
      - /etc/nginx/vhost.d
      - /var/run/docker.sock:/tmp/docker.sock:ro
      - /usr/share/nginx/html
    labels:
      - com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy=true

  nginx-proxy-ssl:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: nginx-proxy-ssl
    volumes:
      - ./certs:/etc/nginx/certs:rw
      - /var/run/docker.sock:/var/run/docker.sock:ro
    volumes_from:
      - nginx-proxy

networks:
  default:
    external:
      name: frontend
```

## Default Container configuration

For a Container to be detected, it has to be in the ```frontend``` network with the environement variables ```VIRTUAL_HOST``` and ```LETSENCRYPT_HOST``` set.

```yml
version: '3.3'
services:
  apache1:
    image: httpd:2.4
    ports:
      - 8001:80
    volumes:
      - ./src:/usr/local/apache2/htdocs/
    environment:
      VIRTUAL_HOST: www.dunkelarts.ch
      LETSENCRYPT_HOST: www.dunkelarts.ch
    networks:
      - "frontend"

networks:
  frontend:
    external: true
```
