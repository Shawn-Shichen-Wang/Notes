[TOC]

# Intro

## Distributions

- Red Hat Enterprise Linux
  - 企业用户
  - Not Free
- ubuntu
  - 对软件提供一致更新

- Debian
  - 以==稳定性==和==可靠性==出名
    - Debian的名称是由他当时的女友（现在为其前妻[[13\]](https://zh.wikipedia.org/wiki/Debian#cite_note-13)）**Deb**ra和**Ian** Murdock自己的名字合并而成的，所以Debian一词是根据这两个名字在[美国英语](https://zh.wikipedia.org/wiki/美國英語)的发音而读作/'dɛbiːjən/。[[14\]](https://zh.wikipedia.org/wiki/Debian#cite_note-14) （笑😊
  - 更新周期长
  - 是ubuntu的父发行版(parent distribution)
- CoreOS

[Distro](https://distrowatch.com/)  免费开源操作系统新闻

## Important Directories

- `home` 存放每个用户的所有文件
- `etc` 存储配置文件
- `var` 存储可变文件(随着时间变化会改变大小，如缓存文件)
- `bin` 存储可执行的 `二进制文件`
- `sbin` 与`bin`类似，只能由路由用户(route user)用于系统管理和维护
- `lib` 存储支持系统中存储的二进制文件的库
- `usr` 存储用户程序

## `$PATH`

- `echo $PATH`
  - `:`分隔，先从从第一个路径查找可执行文件
- [AskUbuntu: How to Update $PATH](http://askubuntu.com/questions/60218/how-to-add-a-directory-to-my-path)
- [Linux环境变量设置](https://www.jianshu.com/p/38d6ae3f911b)

---

# Security

## **最重要的安全性规则**

- ***==最小权限规则==***
- (The Rule of Least Privilege)

## ==强烈建议== 

- 设置新服务器时，禁用以root身份远程登陆
- 以自己创建的用户身份登陆

## `sudo`

- 不用`su`被视为最佳做法

## Package Source Lists

- 类似AppStore
- 位于`/etc/apt/sources.list`

## Update

- 确保系统安全==最重要且最简单==的方式之一

###  `sudo apt-get update` 更新软件包

### `sudo apt-get upgrade` 更新已安装的软件

- 先在非生产环境下测试每个操作

- Other Package Related Tasks
  - `sudo apt-get autoremove` 自动删除不需要的软件包
  - `sudo apt-get install finger` 安装名为'finger'的软件包
## Discovering Packages

  - [Ubuntu](https://packages.ubuntu.com/)

  - [Debian](https://www.debian.org/distrib/packages)

## `/etc/passwd`

- `root:x:0:0:root:/root:/bin/bash` 
  - 用户名: username
  - 加密过的密码: password
  - 用户ID: UID
    - the user’s ID number in the system. 0 is root, 1-99 are for predefined users, and 100-999 are for other system accounts
  - 组ID: GID 
  - 更多说明: user ID info
  - 用户主目录: home directinary
  - 用户默认shell: command/shell

## Creating a New User

  - `sudo adduser season`


## Connecting as the New User

- `ssh [username]@[ip address] -p [port] `
  
  - 127.0.0.1 标准IP地址，始终代表本地主机或当前所在的同一计算机
  
  - 长时间未响应，输入`~.` 终止ssh连接
- `sudo cat /etc/sudoers` 查看有权使用sudo的用户
- 直接用visudo编辑，但是版本升级会覆盖此文件，造成用户丢失
  - 通过编辑`/etc/sudoers.d`目录内的文件添加sudo用户
  
- `touch season`/`cat > season`/`echo > season`
    - `season ALL=(ALL) NOPASSWD:AL`

## [sudo: command not found](https://unix.stackexchange.com/questions/354928/bash-sudo-command-not-found)

- By default sudo is not installed on Debian, but you can install it. First enable su-mode:
  `su -`

  Install sudo by running:
  `apt-get install sudo -y`

## sudo: unable to resolve host xxx

- 通常更改主机名后会遇到此错误
- `sudo nano /etc/hosts`
  - 添加`127.0.0.1	xxx `
  - 或者将旧的主机名改为新的主机名

## Resetting Passwords

- `sudo passwd -e season`
  - -e 强制下次登录重设密码

## Key Based Authentication

==永远不要与任何人共享私有密匙==

==私有密匙应始终牢牢掌握在自己手中==

因此，始终==在本地生成密匙对==

1. [生成新 SSH 密钥并添加到 ssh-agent](https://docs.github.com/cn/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

   `ssh-keygen` ==在本地计算机运行，详情可查看上述链接==

   xxx.pub 相当于锁

   xxx 相当于钥匙

2. 把锁传给远程Linux服务器

   1. `mkdir .ssh` 

   2. `touch .ssh/authorized_keys`

   3. `cat .ssh/xxx.pub`

      **本地**复制xxx.pub的文本内容

   4. `nano .ssh/authorized_keys`

      粘贴复制的内容

   5. `chmod 700 .ssh`

      `chmod 644 .ssh/authorized_keys`

      更改目录及文件权限

3. 使用密匙登陆

   `ssh [name]@[ip address] -p [port] -i ~/.ssh/[name of key]`
   
4. Forcing Key Based Authentication

   - `sudo nano /etc/ssh/sshd_config`
   - `PasswordAuthentication no`, 保存
   - `sudo service ssh restart`
   - SSHD 是运行在服务器上监听所有SSH连接的服务

   

## Public Key Encryption

![image-20201225153255733](Linux_Web_Servers.assets/image-20201225153255733.png)

1. 远程服务器发送信息给本地计算机
2. 本地计算机使用私有密匙将==加密后的信息==回复给远程服务器
3. 远程服务器使用公共密匙解密收到的信息，如果==解密后的值==与发送的值相等，验证通过

## File Permissions

### `ls -al` 

- 首位`d`代表是一个目录, `-`代表是文件
- `root staff` 代表staff组的root用户(owner)

### rwx

![image-20210102161659512](Linux_Web_Servers.assets/image-20210102161659512.png)

- r: read
  - r = 4
- w: write
  - w = 2
- x: execute 执行 如果是`-`表示无执行权限
  - x = 1

### change

- `sudo chmod 644 [file]`
  - 更改文件权限
- `sudo chgrp [group] [file]`
  - 更改组权限
- `sudo chown [own] [file]`
  - 更改用户权限

## Ports

- 对于每个请求类型，服务器通过端口得知对应哪个应用程序
  - 每个应用程序都配置 为响应针对指定端口的请求
- 使用防火墙控制服务器允许哪个端口接受请求
- 常用端口
  - HTTP: 80
  - HTTPS: 443
  - SSH: 22
  - FTP: 21
  - POP3: 110
  - SMTP: 25
- [TCP/UDP端口列表](https://zh.wikipedia.org/wiki/TCP/UDP%E7%AB%AF%E5%8F%A3%E5%88%97%E8%A1%A8)

## UFW

- Ubuntu预装了名为`ufw`的防火墙
- `sudo ufw status`
  - 查看ufw状态
- `sudo ufw default deny incoming`
  - 阻止所有传入请求，然后仅允许需要的请求
- `sudo ufw default allow outgoing`
  - 为服务器向互联网发出的任何请求建立默认规则

- Configuring Ports in UFW
  - `sudo ufw allow ssh`
  - `sudo allow 2222/tcp`
  - `sudo ufw allow www` HTTP server
- `sudo ufw enable` 启用防火墙
  - 启用后SSH断了的话说明搞砸了
  - 某些云服务提供商，通过终端控制面板提供重新获取系统访问权限的办法；但大多数人没这么幸运:sob:
  - 所以建议在服务器设置流程的早期就配置防火墙
- [DigitalOcean文档](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server)

## 服务器配置练习

Here are a few tutorials that will walk you through how to configure your server in a variety of use cases:

- ["LAMP" Stack (Linux, Apache, MySQL, PHP)](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04)
- ["LEMP" Stack (Linux, nginx, MySQL, PHP)](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04)
- [PEPS Mail and File Storage](https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-and-file-storage-with-peps-on-ubuntu-14-04)
- [Mail-in-a-Box Email Server](https://www.digitalocean.com/community/tutorials/how-to-run-your-own-mail-server-with-mail-in-a-box-on-ubuntu-14-04)
- [Lita IRC Chat Bot](https://www.digitalocean.com/community/tutorials/how-to-install-the-lita-chat-bot-for-irc-on-ubuntu-14-04)

---

# Initial Server Setup With Debian9

[DigitaloceanDoc](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-debian-9)

# Web Application Servers

1. [Install_apache](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-debian-9)

   `sudo apt install apache2`

   `sudo ufw allow 'WWW'`

2. Install mod_wsgi

3. Install PostgreSQL

