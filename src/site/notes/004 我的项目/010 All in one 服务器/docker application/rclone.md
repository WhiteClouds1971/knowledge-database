---
{"dg-publish":true,"permalink":"/004 我的项目/010 All in one 服务器/docker application/rclone/","dgPassFrontmatter":true,"created":"2024-06-21T17:23:23.061+08:00","updated":"2024-06-21T17:38:50.194+08:00"}
---

> rclone 官网：[Rclone](https://rclone.org/)
# 安装

1. docker 命令安装：

```zsh
docker run  
  -d  
  --name='Nacho-Rclone-Native-GUI'  
  --net='br0'  
  --ip='192.168.10.102'  
  -e TZ="Asia/Shanghai"  
  -e HOST_OS="Unraid"  
  -e HOST_HOSTNAME="MainServer"  
  -e HOST_CONTAINERNAME="Nacho-Rclone-Native-GUI"  
  -e 'TCP_PORT_5572'='5572'  
  -e 'America/New_York'='America/New_York'  
  -e 'PGID'='100'  
  -e 'PUID'='99'  
  -l net.unraid.docker.managed=dockerman  
  -l net.unraid.docker.webui='http://[IP]:[PORT:5572]/'  
  -l net.unraid.docker.icon='https://raw.githubusercontent.com/rclone/rclone/master/graphics/logo/logo_symbol/logo_symbol_color_256px.png'  
  -v '/mnt/user/appdata/Rclone':'/config/rclone':'rw' 'rclone/rclone' rcd  
  --rc-web-gui  
  --rc-web-gui-update  
  --rc-web-gui-force-update  
  --rc-web-gui-no-open-browser  
  --rc-addr :5572  
  --rc-user rclone  
  --rc-pass rclone  

7601f4dff9fc50a7278508cc585c30aeef6ff977e6a5c11f9ad5f2638670f132
```

2. 图像界面

![Pasted image 20240621173533.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240621173533.png)