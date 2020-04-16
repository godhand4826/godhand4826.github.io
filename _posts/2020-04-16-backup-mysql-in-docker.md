---
layout: post
title:  "Backup MySQL in Docker"
date:   2020-04-20 16:05:03 +0800
categories: mysql
---
## Backup
```
docker exec CONTAINER /usr/bin/mysqldump -u USER --password=PASS DATABASE > backup.sql
```
## Restore
```
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysqldump -u USER --password=PASS DATABASE
```
