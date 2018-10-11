---
layout: post
title:  "Jenkins setup in Linux"
date:   2018-10-11 20:33:03 +0800
categories: jenkins 
---
## Installation in Linux
```
$ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
$ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
$ sudo apt-get update
$ sudo apt-get install jenkins
```
> You can verify successful installation by checking [127.0.0.1:8080]() with the web-browser.

## Upgrade
```
$ sudo apt-get update
$ sudo apt-get install jenkins
```
## Configuration
#### Port
- The default port is __8080__.
- If you wanna change the port, replace __8080__ with  __8082__(new port) in `/etc/init.d/jenkins` and `/etc/default/jenkins` 
- Restart the service.

```
$ sudo service jenkins restart
```
> You can verify successful configuration by checking [127.0.0.1:8082]() with the web-browser.

#### Default Password
- the default account is __admin__ and the password is at `/var/lib/jenkins/secrets/initialAdminPassword`

## CLI
#### Downloading the client
- Go to [127.0.0.1:8082/jnlpJars/jenkins-cli.jar]() to download the `jenkins-cli.jar`.

#### ssh authentication (optional)
- Create `id_rsa` and `id_rsa.pub`.

```
$ cd ~/.ssh/
$ ssh-keygen -t rsa -b 4096
```
- Go to [127.0.0.1:8082/user/admin/configure]() and copy-paste the contents of `id_rsa.pub`.
- Go to [127.0.0.1:8082/confugureSecurity/]() and set the __SSHD__ port to __Random__.


#### CLI commands
Now you can control jenkins by the following command.
```
$ java -jar jenkins-cli.jar -s http://localhost:8082/ -auth admin:PASSWORD [command]
```
- Add a job [command]

```
create-job JOB_NAME < config.xml
```
> The format of config.xml file please checkout `/var/lib/jenkins/jobs/JOB_NAME/config.xml`.

- Delete a job [command]

```
delete-job JOB_NAME 
```

## References
- [Jenkins](https://jenkins.io/)