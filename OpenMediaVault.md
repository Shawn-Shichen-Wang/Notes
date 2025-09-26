[TOC]

# 前言



# 解决i219v网卡问题

## 安装网卡驱动

## 改网卡型号

Omv-extras

# 通知



# Docker

## 常用指令

- `docker ps` 显示所有正在运行的容器
- `docker inspect <container_id_or_name>` 查看容器的详细信息
- 

## 具体Docker

- docker compose 

- portainer

  ```
  docker run -d \
    --name portainer \
    -p 9000:9000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /HGST/DOCKER/appdata/portainer:/data \
    --restart always \
    portainer/portainer-ce:2.31.1
  ```

- heimdall

- Jellyfin

  ```yaml
  version: "2.1"
  services:
    cloudnas:
      image: cloudnas/clouddrive2:latest
      cpus: 1
      mem_limit: 2g
      memswap_limit: 2g
      mem_reservation: 1g
      container_name: clouddrive2
      environment:
         - TZ=Asia/Shanghai
         - CLOUDDRIVE_HOME=/Config
      volumes:
        - /HGST/clouddrive:/CloudNAS:shared
        - /HGST/DOCKER/appdata/clouddrive:/Config
      devices:
        - /dev/fuse:/dev/fuse
      restart: unless-stopped
      pid: "host"
      privileged: true #or you can try capp_add -SYS_ADMIN
      #cap_add: #SYS_ADMIN cap may fail on some OSes, use privileged: true instead
      # - SYS_ADMIN
      network_mode: "host" #if network_mode doesn't work, use port mapping
      #ports:
      #   - 19798:19798
      
    jellyfin:
      image: lscr.io/linuxserver/jellyfin:latest
      depends_on:
        - cloudnas
      container_name: jellyfin
      environment:
        - PUID=1001
        - PGID=100
        - TZ=Asia/Shanghai
        - JELLYFIN_PublishedServerUrl=192.168.2.142 #optional
        - DOCKER_MODS=linuxserver/mods:jellyfin-opencl-intel-23.30.26918.9-a80ee7d743fad6e66fc5323fd0db690082b41a38
      volumes:
        - /HGST/DOCKER/appdata/jellyfin:/config
        - /srv/dev-disk-by-uuid-70ae9955-1f24-4ef8-a148-57b098d93529/clouddrive:/clouddrive:rslave
  
        - /srv/dev-disk-by-uuid-70ae9955-1f24-4ef8-a148-57b098d93529/Media:/media
        - /HGST/DOCKER/appdata/jellyfin/dejavu :/usr/share/fonts/truetype/dejavu
        - /WD/Media:/media_WD
      ports:
        - 8096:8096
        - 8920:8920
      restart: unless-stopped
      devices: 
        - /dev/dri:/dev/dri
      mem_limit: 6g
      memswap_limit: 6g
      mem_reservation: 3g 
  ```

## 网络

- host
- bridge

# 备份

## Rsync

## Backup



  

  


# 公网访问

## nginx

`sudo ln -s /etc/nginx/sites-available/qbittorrent /etc/nginx/sites-enabled/
sudo systemctl restart nginx`

`/etc/nginx/sites-enabled/reverse-proxy.conf -> /etc/nginx/sites-available/reverse-proxy.conf`

# 权限管理

- 递归

# SSL证书

## 阿里云证书

- 申请

## OpenClash Dashboard

[介绍](https://github.com/vernesong/OpenClash/pull/2386)

# 登陆安全



# chatGPT帮助

docker pull linuxserver/qbittorrent

# Linux命令

## 登陆

## SCP

## 文件管理

- 复制
- 删除
- 赋权

## 日志



# 系统工具

## Systemctl



# 硬盘工具

## SoftLink

## HardLink



# 

## 格式化

`sudo mkfs.btrfs /dev/sdX`

## 挂载

`sudo mount /dev/sdb /srv/disk-by-uuid-xxx`

## df

- `df-h` 显示所有挂载的文件系统及其使用情况

## fdisk

## smartctl

```shell
# 查看硬盘信息
sudo smartctl -i /dev/sdX

# 查看所有 SMART 信息
sudo smartctl -a /dev/sdX

# 启用 SMART（如果未启用）
sudo smartctl -s on /dev/sdX

# 执行简短自检测试
sudo smartctl -t short /dev/sdX

# 查看测试状态
sudo smartctl -c /dev/sdX

# 执行长时间自检测试
sudo smartctl -t long /dev/sdX

# 查看最近的自检测试结果
sudo smartctl -l selftest /dev/sdX

# 查看所有 SMART 属性
sudo smartctl -A /dev/sdX

# 查看设备错误日志
sudo smartctl -l error /dev/sdX

# 查看设备健康状态
sudo smartctl -H /dev/sdX

# 查看设备日志目录
sudo smartctl -l directory /dev/sdX
```



## fsck

# 日志

0. ~~备份升级~~
    - omv-backup
    - apt-get update && omv-upgrade
1. ~~安装NPM~~
2. ~~解决Portainer SSL可以登录但没有权限控制的问题~~
3. ~~Qbittorrent“点击劫持”保护、启用跨站请求伪造 (CSRF) 保护、启用 cookie 安全标志（需要 HTTPS或本地连接）关闭后SSL正常登陆使用，打开无法登陆的原因~~
4. Heimdall更改标签页图标及文字描述
5. ~~Scrutiny~~、Prometheus、Grafana
6. MoviePilot
7. MariaDB ( or MySQL )
8. Nextcloud AIO
9. WordPress
10. Home Assistant
11. ~~**ZeroTier** / **WireGuard**~~
12. Fail2ban ( CrowdSec )
13. 邮件通知
14. 安装多个Docker或者服务，包含：scritiny、nginx ui、nginx proxy、grafana、prometheus、moviepilot、nextcloud（可能预先需要安装数据库MariaDB或者MySQL）、wordpress、homeassistant、邮件发送服务、防火墙、fail2ban邮件配置、防病毒软件、ZeroTier或其他的VPN服务、Clonezilla或者其他方案备份OMV和DOCKER配置文件
15. DIFY
16. n8n

```
---
version: "2.1"
services:
  qbittorrent:
    image: linuxserver/qbittorrent:libtorrentv1
    cpus: 2
    container_name: qbittorrent
    environment:
      - PUID=1001
      - PGID=100
      - TZ=Asia/Shanghai
      - WEBUI_PORT=8082
    volumes:
      - /HGST/DOCKER/appdata/qbittorrent:/config
      - /HGST/PT_HGST:/PT_HGST
      - /WD/PT_WD:/PT_WD
      
    ports:
      - 8082:8082
      - 18978:18978
      - 18978:18978/udp
    mem_limit: 8g
    memswap_limit: 8g
    mem_reservation: 8g
    restart: unless-stopped
```

```
version: "2.4"

services:
  qbittorrent:
    image: linuxserver/qbittorrent:libtorrentv1
    container_name: qbittorrent

    environment:
      - PUID=1001
      - PGID=100
      - TZ=Asia/Shanghai
      - WEBUI_PORT=8082

    volumes:
      - /HGST/DOCKER/appdata/qbittorrent:/config
      - /HGST/PT_HGST:/PT_HGST
      - /WD/PT_WD:/PT_WD

    ports:
      - 8082:8082
      - 18978:18978
      - 18978:18978/udp

    # ---------- 资源限制 ----------
    cpuset: "2-3"         # 固定到第 2、3 号逻辑核
    cpu_quota: 150000     # 1.5 核
    cpu_shares: 512

    mem_limit: 4g         # 物理内存硬上限
    mem_reservation: 2g   # 软阈值
    memswap_limit: 4g     # 不额外用 swap

    blkio_config:
      weight: 300
      device_read_bps:
        - path: "/dev/sda"
          rate: "40mb"
        - path: "/dev/sdb"
          rate: "40mb"
      device_write_bps:
        - path: "/dev/sda"
          rate: "40mb"
        - path: "/dev/sdb"
          rate: "40mb"

    restart: unless-stopped

```

```
version: "3"
services:
  app:
    image: jc21/nginx-proxy-manager:latest
    restart: unless-stopped
    ports:
      - "80:80"      # HTTP
      - "81:81"      # Admin UI
      - "443:443"    # HTTPS
    volumes:
      - /HGST/DOCKER/appdata/nginx-proxy-manager:/data
      - ./letsencrypt:/etc/letsencrypt
```



