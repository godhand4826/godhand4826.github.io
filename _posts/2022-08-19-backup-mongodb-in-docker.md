---
layout: post
title:  "Backup MongoDB in Docker"
date:   2022-08-19 10:56:00 +0800
categories: mongodb
---
## Backup
```bash
docker exec CONTAINER mongodump -u USER -p PASS --authenticationDatabase AUTH_DB --gzip --archive > backup.gzip
```
## Restore
```bash
cat backup.gzip | docker exec -i CONTAINER mongorestore -u USER -p PASS --authenticationDatabase AUTH_DB --gzip --archive [--nsInclude DATABASE.COLLECTION]
```
