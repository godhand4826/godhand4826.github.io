---
layout: post
title:  "Forward proxy and reverse proxy via ssh"
date:   2021-1-15 10:00:00 +0800
categories: linux ssh
---
```bash
# forward proxy
#   request -> LOCAL -> SSH_SERVER -> TARGET
ssh -NfL $LOCAL_PORT:$TARGET:$TARGET_PORT $USER@$SSH_SERVER

# reverse proxy
#   request -> SSH_SERVER -> LOCAL -> TARGET
ssh -NfR $SSH_SERVER_PORT:$TARGET:$TARGET_PORT $USER@$SSH_SERVER
```
- N: Do not execute a remote command. This is useful for just forwarding ports.
- f: background
- L: Specifies that connections to the given TCP port or Unix socket on the local (client) host are to be forwarded to the given host and port, or Unix socket, on the remote side. 
- R: Specifies that connections to the given TCP port or Unix socket on the remote (server) host are to be forwarded to the local side.