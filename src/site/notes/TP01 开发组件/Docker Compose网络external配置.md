---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker Compose网络external配置/","dgPassFrontmatter":true,"created":"2024-11-29T10:17:08.311+08:00","updated":"2024-11-29T10:24:11.639+08:00"}
---

# Docker Compose 网络 `external` 配置项

在 Docker Compose 文件中，`external` 配置项用于指定网络是否为 **外部网络**。当设置 `external: true` 时，Docker Compose 会将该网络视为已经存在的网络，而不是尝试创建一个新的网络。

>[! tip] 注意如果指定的外部网络不存在，docker 不会自动创建网络。如果指定的外部网络不存在，Docker 会报错，提示找不到网络。
## 配置示例

```yaml
networks:
  univer:
    external: true
    name: univer-prod
```

**`external: true`**：告诉 Docker Compose，这个网络是外部的，即它不由 Compose 来管理或创建，而是一个已经存在的网络。通常是由其他服务或手动创建的。
## 作用

### 1. 共享外部网络

你可以让多个 Docker Compose 项目共享同一个外部网络，这样多个服务可以通过相同的网络进行通信。例如，不同的应用程序或服务可以在不同的 Compose 项目中使用相同的外部网络。
### 2. 避免 Docker Compose 创建新网络

默认情况下，Docker Compose 会为每个项目自动创建网络。如果你希望 Docker Compose 使用已存在的网络，而不是创建一个新的网络，可以通过设置 `external: true` 来控制。