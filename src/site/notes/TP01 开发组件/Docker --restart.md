---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker --restart/","dgPassFrontmatter":true,"created":"2024-10-29T10:25:06.524+08:00","updated":"2024-11-27T10:23:18.017+08:00"}
---

```zsh
docker run --name my-web-server --restart always -d nginx
```

`--restart` 选项用于设置 Docker 容器的自动重启策略：

- **no**：容器不会自动重启（默认）。
- **on-failure**：仅在容器异常退出时重启。
- **always**：无论退出状态如何，总是重启。
- **unless-stopped**：总是重启，除非容器被手动停止。