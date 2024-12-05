---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker network/","dgPassFrontmatter":true,"created":"2024-11-29T10:01:05.671+08:00","updated":"2024-11-29T10:02:00.046+08:00"}
---

# Docker Compose 网络介绍

在 Docker Compose 中，`networks` 配置项用于定义服务之间的网络连接。它允许你为不同的服务指定网络，提供隔离或共享通信通道。通过合理配置网络，可以优化容器之间的通信并提高应用的安全性和可管理性。

## 示例：

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
      - frontend
      - backend

  db:
    image: postgres
    networks:
      - backend

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

## 网络类型：

- **bridge**：默认的网络驱动，用于同一主机上的容器。
- **host**：容器共享主机网络堆栈。
- **overlay**：跨多个主机的容器网络。
- **none**：禁用容器的网络连接。