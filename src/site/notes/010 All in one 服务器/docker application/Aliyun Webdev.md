---
{"dg-publish":true,"permalink":"/010 All in one 服务器/docker application/Aliyun Webdev/","dgPassFrontmatter":true,"created":"2024-06-21T15:13:51.938+08:00","updated":"2024-06-22T10:27:57.495+08:00"}
---

> 已弃用，推荐使用 alist 见[[010 All in one 服务器/docker application/Alist\|Alist]]
# 安装

项目仓库地址：[GitHub - messense/aliyundrive-webdav: 阿里云盘 WebDAV 服务](https://github.com/messense/aliyundrive-webdav)

1. docker 配置

![Pasted image 20240621160742.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240621160742.png)

对应的 docker 命令：

```zsh
docker run  
  -d  
  --name='aliyunwebdev'  
  --net='br0'  
  --ip='192.168.10.101'  
  -e TZ="Asia/Shanghai"  
  -e HOST_OS="Unraid"  
  -e HOST_HOSTNAME="MainServer"  
  -e HOST_CONTAINERNAME="aliyunwebdev"  
  -e 'WEBDAV_AUTH_USER'=''  
  -e 'WEBDAV_AUTH_PASSWORD'=''  
  -e 'REFRESH_TOKEN'=''  
  -l net.unraid.docker.managed=dockerman  
  -l 'restart'='unless-stopped' 'messense/aliyundrive-webdav'
```

2. 如果镜像拉不下来配置代理

在 Openwrt 的代理中配置如下信息

```zsh
hub.docker.com
production.cloudflare.docker.com
```

![Pasted image 20240621160840.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240621160840.png)

3. 连接访问

![Pasted image 20240621160930.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240621160930.png)