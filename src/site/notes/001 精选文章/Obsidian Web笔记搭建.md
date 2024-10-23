---
{"dg-publish":true,"permalink":"/001 精选文章/Obsidian Web笔记搭建/","dgPassFrontmatter":true,"created":"2024-10-20T10:56:27.654+08:00","updated":"2024-10-23T15:42:35.479+08:00"}
---

> 将 Obsidian 里记录的笔记转换成 HTML 文件，发布到互联网上。

主要思路是通过一个名为`DigItal Garden` 的插件将笔记转换为 node 项目，推送到 GitHub 上，在通过阿里的云效进行自动构建。然后通过 Frp 进行内网穿透供公网用户访问。

因为之前`DigItal Garden` 的具体使用方法我没记录，我这里直接从云效构建开始。

我的笔记 Github 地址：[knowledge-database](https://github.com/WhiteClouds1971/knowledge-database)

云效的详细使用方法参见[[002 开发随笔/迁移云效调研#持续集成\|迁移云效调研#持续集成]]，这章节记录一下一些需要特别注意的地方。
# Nginx 的安装与配置

在开始构建部署之前，我们需要安装一下 Nginx 并为项目配置一下代理。
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

![Pasted image 20241023135644.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023135644.png)
## 配置文件

```zsh
sudo vim /etc/nginx/conf.d/web-node-obsidian.conf
```

```text
# /etc/nginx/conf.d/web-node-obsidian.conf

server {
    server_name note.whiteclouds.work;
    client_max_body_size 500M;
    proxy_connect_timeout       6000;
    proxy_send_timeout          6000;
    proxy_read_timeout          6000;
    send_timeout                6000;
    sendfile on;
    location / {
        root /home/app/web-note-obsidian/dist/;
        index index.html index.htm;
    }
    listen 80;
}
```
## 查看 Nginx 日志

```zsh
tail -f /var/log/nginx/
```

# SELinux 和防火墙

最后记得关闭 SELinux，不然访问不了网页的。参考：


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/002/linux/#se-linux" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



# SELinux 和防火墙

> 每当你访问不了服务的的时候，查看一下 SELinux 和防火墙是不是关闭了🙂
## SELinux
### 获取当前 selinux 状态

```zsh
getenforce
```

Enforcing 为开启，Permissive 为关闭
### 临时关闭selinux

```zsh
setenforce 0
```
### 永久关闭selinux

```zsh
sudo vi /etc/sysconfig/selinux
```

SELINUX=enforcing 替换为SELINUX=disabled

![Pasted image 20241020151350.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020151350.png)
## 关闭防火墙

```zsh
systemctl stop firewalld.service
systemctl disable firewalld.service
systemctl status firewalld
```

![Pasted image 20241023145321.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023145321.png)

</div></div>

# 持续集成
## 添加 GitHub 源

我们的笔记项目在 GitHub 上而不是在云效自己的代码仓库里。所以第一步就要为流水线添加 GitHub 源
### GitHub授权

一路确定即可

![Pasted image 20241023135944.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023135944.png)
### GitHub 仓库添加Webhook

>[, \_云效(Apsara Devops)-阿里云帮助中心](https://help.aliyun.com/zh/yunxiao/user-guide/code-source-trigger?spm=a2cl9.flow_devops2020_goldlog_detail.0.0.6e234c0ahG1jXV)

在选择完仓库和分支后需要在 GitHub 上的仓库添加一个 Webhook，在分支发生提交后，通知云效进行持续构建。

![Pasted image 20241023140213.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023140213.png)
## 构建

阿里云提供的北京云效构建集群，死活构建不过去。无奈之下，只能使用自己的构建集群自行构建了，私有构建集权的添加参考[[002 开发随笔/迁移云效调研#添加私有构建集群\|迁移云效调研#添加私有构建集群]]
## 部署

将上一步构建上传到云效的构建物（dist 包）下载到部署主机上并解压。

部署命令：

```zsh
cd /home/app/web-note-obsidian
sudo rm -rf /home/app/web-note-obsidian/dist/
sudo mkdir -p /home/app/web-note-obsidian/dist
sudo tar zxvf package.tgz -C dist
```
## 测试

至此，我们的笔记发生变更推送到 GitHub 上时就可以实现自动构建了。现在我们就可以通过内网方法笔记了，下一步就需要通过 Frp 把服务映射到公网上供所有人访问。

![Pasted image 20241020161628.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020161628.png)
# 域名

接下来我们需要为笔记设置一个域名，首先找一个域名服务商注册购买一个域名。

然后我们为域名添加两个 DNS 解析`note.xxxx`和`frp.xxxx`：

![Pasted image 20241023141213.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023141213.png)

**主机名称和记录值：**

![Pasted image 20241023141407.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023141407.png)
# IPC 备案

在 DNS 映射后，还需要对网站的一级域名进行备案。如果没有备案，直接访问 80,443 端口一般都是会被云服务商阻断的。

其次我们只需要备案一级域名就可以了，剩下的二级域名不需要备案也可使用。比如我们备案了 a.com 那么 b.a.com；aa.a.com 都是可以使用 80 等端口的。

备案的流程现在一般都是通过云服务商提供的服务进行备案。按照云服务商的要求填写相关信息后，等待 20 天左右就会收到工信部的备案审批通知。
# Frp 内网穿透和反向代理

关于 Frp 的详细介绍和教程我之前看过一些，但是没有做过笔记。大家上网找一下教程看一下吧。我这里记录一下具体配置过程。
## Frps 服务端配置
### 下载压缩包

```zsh
mkdir -p /home/app/frp
cd /home/app/frp
wget https://github.com/fatedier/frp/releases/download/v0.61.0/frp_0.61.0_linux_amd64.tar.gz
tar -zxvf frp_0.61.0_linux_amd64.tar.gz
```
### 配置服务端配置文件（frps.toml）

```zsh
cd frp_0.61.0_linux_amd64.tar.gz
vim frps.toml

[common]
bind_port = 7000
dashboard_port = 7500
token = ""
dashboard_user = ""
dashboard_pwd = ""
vhost_http_port = 80
vhost_https_port = 443
subdomain_host = xxxx.com # 设置用于子域名的主域名
```

![Pasted image 20241020163749.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020163749.png)

打马赛克的地方大家自己填写并记住即可，没有过多要求。
### 设置 Frp 开机自启动

Frp 的开机启动要稍微麻烦一点，我们需要自己编写一下启动脚本。然后通过 `systemctl` 命令启动。可以参考一下 [[004 我的文档/010 All in one 服务器/8. openvpn的搭建#设置 openservice 自启动\|8. openvpn的搭建#设置 openservice 自启动]]
#### 创建启动文件

```zsh
sudo vim /etc/systemd/system/frps.service
```

复制以下内容：

```text
[Unit]
Description=FRP Server Service
After=network.target

[Service]
User=root
Group=root
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/home/app/frp/frp_0.61.0_linux_amd64/frp/frps -c /home/app/frp/frp_0.61.0_linux_amd64/frp/frps.toml
[Install]
WantedBy=multi-user.target
```
#### 启动 frp 服务端

```zsh
sudo systemctl daemon-reload # 重载配置
sudo systemctl start frps # 启动
sudo systemctl status frps # 确认状态
sudo systemctl enable frps # 自启
```
### 云服务器的端口放行

如果你使用的中转服务器是云服务器，一般都是要在服务商提供的 Web 管理界面放行上面配置的 7000 和 7500 这两个端口：

![Pasted image 20241023141605.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023141605.png)

访问 http://IP:7500 就可以查看到服务端 GUI 了：

![Pasted image 20241022143457.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241022143457.png)
## Frpc 客户端配置

frp 客户端一般推荐配置在网关上，来转发整个内网的流量。我是在一个虚拟机上搭建一个 openwrt 软路由作为网关，如果你对软路由搭建感兴趣可以参考 [[004 我的文档/010 All in one 服务器/7. 软路由\|7. 软路由]]。所以我用 openwrt 内置的 frp 客户端服务进行配置，因此这章节只有一些参考性，如果需要在 Linux 上配置客户端，自行百度一下。
### 服务器连接配置

![Pasted image 20241023141849.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023141849.png)
### 具体服务配置

![Pasted image 20241023154233.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241023154233.png)

现在我们在公网访问 `note.whiteclouds.work` 就可以看到笔记了：
# Https 协议配置
