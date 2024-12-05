---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker compose如何自动分配网段/","dgPassFrontmatter":true,"created":"2024-11-29T10:09:38.762+08:00","updated":"2024-11-29T10:17:14.220+08:00"}
---

# Docker 网络 IP 地址分配与冲突避免

在 Docker 中，当创建多个网络时，Docker 会使用 IP 地址管理（IPAM）机制自动分配 IP 地址并避免网络之间的冲突。以下是 Docker 如何避免网络冲突的详细介绍。
## 1. 自动选择未被占用的子网

如果没有显式指定子网，Docker 会自动为每个新创建的网络选择一个未被占用的子网。它会扫描当前已存在的网络，确保新网络的网段与现有网络不冲突。通常，Docker 会从私有 IP 地址范围内选择网段，如 `172.17.0.0/16`、`192.168.0.0/16` 等。
## 2. 用户指定的子网

用户可以在创建自定义网络时显式指定子网，以避免冲突。例如：

```yaml
version: '3.8'

services:
  web:
    image: nginx
    networks:
      - frontend

  app:
    image: myapp
    networks:
      - backend

  db:
    image: postgres
    networks:
      - backend

networks:
  frontend:
    driver: bridge
    ipam:
      config:
        - subnet: "192.168.1.0/24"  # 用户指定的子网
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: "192.168.2.0/24"  # 用户指定的子网
```
## 3. IPAM 自动管理

如果没有显式指定子网，Docker 会自动选择一个合理的子网，同时避免与现有网络冲突。Docker 的 IPAM 机制在创建新网络时会自动管理子网和 IP 地址。
## 4. 冲突检测机制

如果显式指定的子网发生冲突（例如，两个网络使用相同的 `192.168.1.0/24` 子网），Docker 会在网络创建时报错，提示子网已经被占用：

```zsh
docker network create --subnet=192.168.1.0/24 my_network1 
docker network create --subnet=192.168.1.0/24 my_network2
```

返回错误信息：

```csharp
Error response from daemon: Pool overlaps with other one on this address space
```