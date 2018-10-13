---
layout: post
title:  "Windows Subsystem for Linux"
date:   2018-10-12 14:46:00 +0800
categories: wsl
---
## wsl
- Run __PowerShell__ as admin

```
$ Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
- Reboot the computer
- Download __Ubuntu__ form __Microsoft Store__
- Install and set username and password

## ZSH
```
$ sudo apt-get update
$ sudo apt-get install zsh
$ which zsh
/usr/bin/zsh
$ chsh -s /usr/bin/zsh
```
## Oh My ZSH
```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
- Edit `~/.zshrc` to change theme and add plugins.

```
ZSH_THEME="ys"

plugins=(
  git
  npm
  node
  sudo
)
```
## References
1. [Windows Subsystem for Linux 環境配置 (最新 1709 版) – Hungys.blog() – Medium](https://medium.com/hungys-blog/windows-subsystem-for-linux-configuration-caf2f47d0dfb)
1. [Plugins · robbyrussell/oh-my-zsh Wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins)