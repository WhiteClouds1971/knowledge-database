---
{"dg-publish":true,"permalink":"/004 我的项目/010 All in one 服务器/docker application/Alist/","dgPassFrontmatter":true,"created":"2024-06-22T10:27:46.826+08:00","updated":"2024-06-22T10:59:07.793+08:00"}
---

> 官网：[Introduction | AList文档](https://alist.nn.ci/zh/guide/)
# 下载安装

![Pasted image 20240622102832.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622102832.png)
# 设置用户名密码

打开 docker 命令行输入：

```zsh
# 随机生成一个密码
docker exec -it alist ./alist admin random
# 手动设置一个密码,`NEW_PASSWORD`是指你需要设置的密码
docker exec -it alist ./alist admin set NEW_PASSWORD
```

![Pasted image 20240622103011.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622103011.png)
# 添加存储
## 阿里云盘

![Pasted image 20240622103247.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622103247.png)

其中刷新令牌通过 [Get Aliyundrive Refresh Token | AList Docs](https://alist.nn.ci/tool/aliyundrive/request) 这个连接获取
## 本地存储

![Pasted image 20240622103827.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622103827.png)
## SFTP 和 FTP

**目前 SFTP 有问题，部分文件不可见。FTP 配置之后删不到。都不推荐使用。**
# 同意webdev 挂载 alist

将 alist 里聚合的多存储库文件通过 webdev 挂载出去

1. alist WEBDEV 的网址格式：

![Pasted image 20240622103700.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622103700.png)
2. 例子

![Pasted image 20240622105842.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622105842.png)

![Pasted image 20240622103908.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240622103908.png)
