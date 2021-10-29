---
title: "Minecraft Server with Docker"
layout: "post"
#url: "/archives/"
summary: Using Docker to host a private Minecraft Server
date: "2021-10-28T09:43:40"
---

## Docker-Compose

This is a quick Demonstration on how to set up a local Minecraft server on Docker using itzg's Docker image.

```yml
version: "3"

services:
  mc:
    image: itzg/minecraft-server
    ports:
      - 25565:25565
    environment:
      EULA: "TRUE"
      MEMORY: "8G"
      VERSION: 1.17.10
    restart: unless-stopped
    volumes:
      - ./minecraft-data:/data
```

## Whitelist.json

```json
[
  {
    "uuid": "f430dbb6-5d9a-444e-b542-e47329b2c5a0",
    "name": "username1"
  },
  {
    "uuid": "e5aa0f99-2727-4a11-981f-dded8b1cd032",
    "name": "username2"
  }
]
```

## minecraft-data directory structure

```
minecraft-data
├── minecraft_server.$VERION.jar
├── whitelist.json
├── ops.json
├── server-icon.png
├── world
│   └── world data
└── server.properties
```