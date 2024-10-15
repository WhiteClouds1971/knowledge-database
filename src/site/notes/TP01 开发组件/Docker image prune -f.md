---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker image prune -f/","dgPassFrontmatter":true,"created":"2024-10-09T10:25:27.815+08:00","updated":"2024-10-15T17:21:45.221+08:00"}
---


`docker image prune -f` 是 Docker 命令行工具中的一个命令，用于清理不再使用的 Docker 镜像。这个命令的各个部分解释如下：

- `docker`：这是 Docker 的命令行客户端。
- `image`：指定要操作的是 Docker 镜像。
- `prune`：是一个动词，表示“修剪”或“清理”。在这个上下文中，它意味着移除满足某些条件的对象。
- `-f` 或者 `--force`：这是一个选项，它会跳过确认提示，直接执行清理动作。

当你运行 `docker image prune -f` 时，Docker 会删除所有未被任何容器使用的悬空镜像（dangling images）以及未被标记且未被任何容器使用的镜像。这可以帮助释放磁盘空间，并保持 Docker 环境的整洁。

需要注意的是，使用 `-f` 选项会立即执行命令而不会要求确认，因此在执行前请确保你确实想要删除这些镜像，因为一旦删除，除非有备份或者可以从源重新拉取，否则数据将无法恢复。

如果你只想查看哪些镜像会被删除而不实际执行删除操作，可以省略 `-f` 选项，即只运行 `docker image prune`，这样 Docker 会先显示将被删除的镜像列表并询问你是否继续。