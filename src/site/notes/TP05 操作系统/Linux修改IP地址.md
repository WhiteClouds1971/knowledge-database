---
{"dg-publish":true,"permalink":"/TP05 操作系统/Linux修改IP地址/","dgPassFrontmatter":true,"created":"2024-09-30T10:07:36.496+08:00","updated":"2024-10-15T17:23:50.479+08:00"}
---

# 永久修改 IP

1. 编辑配置文件

```zsh
vim /etc/network/interfaces
```

2. 重启网卡

```zsh
systemctl restart networking
```