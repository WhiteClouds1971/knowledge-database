---
{"dg-publish":true,"permalink":"/TP05 操作系统/Linux 复制ssh公钥到剪贴板/","dgPassFrontmatter":true,"created":"2023-09-15T11:39:31.048+08:00","updated":"2024-06-01T10:51:08.254+08:00"}
---

复制ssh公钥到剪贴板

```shell
pbcopy < ~/.ssh/id_rsa.pub
```