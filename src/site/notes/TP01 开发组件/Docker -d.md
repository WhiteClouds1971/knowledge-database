---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker -d/","dgPassFrontmatter":true,"created":"2024-05-22T09:41:20.708+08:00","updated":"2024-06-01T10:50:23.601+08:00"}
---

```zsh
docker run \
-e RABBITMQ_DEFAULT_USER=itcast \
-e RABBITMQ_DEFAULT_PASS=123321 \
--name mq \
--hostname mq1 \
-p 15672:15672 \
-p 5672:5672 \
-d \
rabbitmq:3-management
```

- `docker run`: 启动一个新的 Docker 容器。
- `-e RABBITMQ_DEFAULT_USER=itcast`: 设置 RabbitMQ 的默认用户名为 `itcast`。
- `-e RABBITMQ_DEFAULT_PASS=123321`: 设置 RabbitMQ 的默认密码为 `123321`。
- `--name mq`: 为容器指定一个名称，叫做 `mq`。
- `--hostname mq1`: 设置容器的主机名为 `mq1`。
- `-p 15672:15672`: 将宿主机的 15672 端口映射到容器的 15672 端口。这个端口用于 RabbitMQ 管理控制台。
- `-p 5672:5672`: 将宿主机的 5672 端口映射到容器的 5672 端口。这个端口用于 AMQP 协议的客户端连接。
- **`-d`: 以守护进程模式（后台运行）启动容器。**
- `rabbitmq:3-management`: 使用带有管理插件的 RabbitMQ 3 版本的镜像。

这个命令将启动一个带有管理插件的 RabbitMQ 容器，允许你通过 `http://localhost:15672` 访问 RabbitMQ 管理控制台，并使用 `itcast` 作为用户名和 `123321` 作为密码进行登录。同时，应用可以通过 `amqp://localhost:5672` 连接到这个 RabbitMQ 实例。