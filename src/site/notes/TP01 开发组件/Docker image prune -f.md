---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker image prune -f/","dgPassFrontmatter":true,"created":"2024-10-09T10:25:27.815+08:00","updated":"2024-10-29T10:23:55.722+08:00"}
---

```zsh
docker image prune -f
```

- `docker`: Docker 的命令行客户端。
- `image`: 指定操作对象为 Docker 镜像。
- `prune`: 清理不再使用的镜像。
- `-f` 或 `--force`: 跳过确认提示，直接执行清理。

运行 `docker image prune -f` 会立即删除所有未被任何容器使用的悬空镜像（dangling images）和未被标记的镜像，帮助释放磁盘空间。

**注意**：使用 `-f` 选项会立即执行清理，因此请确保您确实想要删除这些镜像。如果只想查看将被删除的镜像列表而不实际删除，可以运行 `docker image prune`。