---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker -d/","dgPassFrontmatter":true,"created":"2024-05-22T09:41:20.708+08:00","updated":"2024-10-29T10:22:46.922+08:00"}
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

- `-d`: 以守护进程模式（后台运行）启动容器。

这个命令启动了一个带有管理插件的 RabbitMQ 容器，并在后台运行。通过 `http://localhost:15672` 可以访问 RabbitMQ 管理控制台，使用用户名 `itcast` 和密码 `123321` 登录。同时，应用可以通过 `amqp://localhost:5672` 连接到这个 RabbitMQ 实例。