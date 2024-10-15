---
{"dg-publish":true,"permalink":"/TP05 操作系统/SSH公钥登录/","dgPassFrontmatter":true,"created":"2024-06-01T18:04:02.920+08:00","updated":"2024-10-15T17:24:05.905+08:00"}
---

每次登录远程主机都需要输入密码是很不方便的，如果想要省去这一步骤，可以利用密钥对进行连接，还可以提高安全性。

1. **在本机生成密钥对**：

```zsh
$ ssh-keygen
```

然后根据提示一步步的按enter键即可（其中有一个提示是要求设置私钥口令passphrase，不设置则为空，这里看心情吧，如果不放心私钥的安全可以设置一下），执行结束以后会在 /home/当前用户目录下生成一个 .ssh 文件夹,其中包含私钥文件 id_rsa 和公钥文件 id_rsa.pub。

2. **通过 FTP 工具将~/.ssh/id_rsa.pub 公钥复制到远程主机上**

![Pasted image 20240601182013.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240601182013.png)

3. **将公钥添加到authorized_keys中**

```zsh

```
