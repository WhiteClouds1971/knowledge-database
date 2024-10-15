---
{"dg-publish":true,"permalink":"/TP01 开发组件/Maven 指定配置文件启动/","dgPassFrontmatter":true,"created":"2024-07-03T11:51:25.540+08:00","updated":"2024-10-15T17:22:01.739+08:00"}
---

```zsh
mvn package -Ptest -DskipTests
java -jar target/python-lab-service-v1.1.1.jar
```