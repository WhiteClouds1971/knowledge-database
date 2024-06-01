---
{"dg-publish":true,"permalink":"/tp-316-8/linux-8081/","dgPassFrontmatter":true,"created":"2024-03-26T09:16:39.595+08:00","updated":"2024-06-01T10:51:11.123+08:00"}
---

# 杀死占用8081端口的应用

如果需要终止占用8081端口的应用，可以按照以下步骤进行：
```
lsof -i :8081
kill -9 12345
```