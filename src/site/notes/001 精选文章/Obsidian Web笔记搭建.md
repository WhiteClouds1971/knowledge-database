---
{"dg-publish":true,"permalink":"/001 精选文章/Obsidian Web笔记搭建/","dgPassFrontmatter":true,"created":"2024-10-20T10:56:27.654+08:00","updated":"2024-10-20T15:16:50.634+08:00"}
---

> 将 Obsidian 里记录的笔记转换成 HTML 文件，发布到互联网上。

主要思路是通过一个名为`DigItal Garden` 的插件将笔记转换为 node 项目，推送到 GitHub 上，在通过阿里的云效进行自动构建。然后通过 Frp 进行内网穿透供公网用户访问。

因为之前`DigItal Garden` 的具体使用方法我没记录，我这里直接从云效构建开始。

我的笔记 Github 地址：[knowledge-database](https://github.com/WhiteClouds1971/knowledge-database)

云效的详细使用方法参见[[002 开发随笔/迁移云效调研#持续集成\|迁移云效调研#持续集成]]，这章节记录一下一些需要特别注意的地方。
# Nginx 的安装与配置

在开始构建部署之前，我们需要安装一下 Nginx 并为项目配置一个代理。
## 安装并设置自启动

```zsh
yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```
## 配置字符集

给 Nginx 配置 utf8 字符集，不然网页会乱码。

```zsh
vim /etc/nginx/nginx.conf

# charset utf-8;
```

![Pasted image 20241020150610.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020150610.png)
## 配置文件

```zsh
sudo vim /etc/nginx/conf.d/web-node-obsidian.conf
```

```text
# /etc/nginx/conf.d/web-node-obsidian.conf

server {
    server_name  192.168.10.51;
    client_max_body_size 500M;
    proxy_connect_timeout       6000;
    proxy_send_timeout          6000;
    proxy_read_timeout          6000;
    send_timeout                6000;
    sendfile on;
    location / {
        root /home/app/web-note-obsidian/dist/;
        index index.html  index.htm;
    }
    listen 80;
}
```
## 关闭SELinux

最后记得关闭 SELinux，不然访问不了网页的。参考：[[002 开发随笔/Linux初始化流程#永久关闭selinux\|Linux初始化流程#永久关闭selinux]]
# 持续集成
## 添加 GitHub 源

我们的笔记项目在 GitHub 上而不是在云效自己的代码仓库里。所以第一步就要为流水线添加 GitHub 源
### GitHub授权

一路确定即可

![Pasted image 20241020110757.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020110757.png)
### GitHub 仓库添加Webhook

>[, \_云效(Apsara Devops)-阿里云帮助中心](https://help.aliyun.com/zh/yunxiao/user-guide/code-source-trigger?spm=a2cl9.flow_devops2020_goldlog_detail.0.0.6e234c0ahG1jXV)

在选择完仓库和分支后需要在 GitHub 上的仓库添加一个 Webhook，在分支发生提交后，通知云效进行持续构建。

![Pasted image 20241020114646.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020114646.png)
## 构建

阿里云提供的北京云效构建集群，死活构建不过去。无奈之下，只能使用自己的构建集群自行构建了，私有构建集权的添加参考[[002 开发随笔/迁移云效调研#添加私有构建集群\|迁移云效调研#添加私有构建集群]]

![Pasted image 20241020124222.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020124222.png)
## 部署

将上一步构建上传到云效的构建物（dist 包）下载到部署主机上并解压。

部署命令：

```zsh
cd /home/app/web-note-obsidian
mkdir -p /home/app/web-note-obsidian/dist
rm -rf /home/app/web-note-obsidian/dist/*
sudo tar zxvf package.tgz -C dist
```
## 测试

至此，我们的笔记发生变更推送到 GitHub 上时就可以实现自动构建了。现在我们就可以通过内网方法笔记了，下一步就需要通过 Frp 把服务映射到公网上供所有人访问。

