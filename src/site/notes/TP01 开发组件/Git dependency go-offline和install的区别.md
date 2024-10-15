---
{"dg-publish":true,"permalink":"/TP01 开发组件/Git dependency go-offline和install的区别/","dgPassFrontmatter":true,"created":"2023-10-20T11:12:41.093+08:00","updated":"2024-06-01T10:50:32.384+08:00"}
---

**Maven 构建命令：`mvn -B dependency:go-offline` 和 `mvn install` 的区别**

1. `mvn -B dependency:go-offline`：
   - 目的：下载项目依赖项到本地仓库，以确保后续构建时不依赖外部仓库的可用性。
   - 行为：下载依赖项到本地 Maven 仓库，但不执行实际项目构建。通常用于预下载依赖项。

2. `mvn install`：
   - 目的：构建项目并将构建结果（通常是 JAR 文件）安装到本地 Maven 仓库，以供其他项目引用。
   - 行为：执行项目构建，生成构建结果，并将其安装到本地仓库。

通常，你可以首先运行 `mvn -B dependency:go-offline` 下载依赖项，然后运行 `mvn install` 构建项目并安装到本地仓库，以确保项目构建时依赖项可用并可供其他项目引用。
