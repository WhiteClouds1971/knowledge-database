---
{"dg-publish":true,"permalink":"/002 开发随笔/Linux初始化流程/","dgPassFrontmatter":true,"created":"2024-09-30T10:09:10.911+08:00","updated":"2024-10-28T17:02:34.761+08:00"}
---

在我们安装完 Linux 操作系统之后，我们需要对 OS 进行一些通用配置，来方便我们的使用。于本文记录一下通用的流程。
# 修改 IP
## 修改配置文件
### 查看网口

```zsh
ip addr
```

![Pasted image 20240930101552.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240930101552.png)
### 编辑配置文件

eth0替换为你网卡名称

```zsh
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

![Pasted image 20240930112443.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240930112443.png)
## 重启网络服务

```zsh
service network restart
```
# SSH 设置

通过以下命令使用用户名密码进行 ssh 连接：

```zsh
ssh -p port username@ip 
```

在输入密码就可以连上云服务器了。
## SSH 公钥免密登录
### 生成密钥对

在云服务器上执行以下命令：

```zsh
ssh-keygen
```

![Pasted image 20240930103551.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240930103551.png)
### 复制公钥

将 local 机器上的 ~/.ssh/id_rsa.pub 里的内容复制到服务里的~/.ssh/authorized_keys 里

```zsh
pbcopy < ~/.ssh/id_rsa.pub
```

![Pasted image 20240602155101.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240602155101.png)
### 修改配置文件

```zsh
chmod 700 ~/.ssh

vi /etc/ssh/sshd_config
PasswordAuthentication yes　　　　　　# 口令登录
RSAAuthentication yes　　　　　　　　　# RSA认证
PubkeyAuthentication yes　　　　　　　# 公钥登录

systemctl restart sshd
```
# 更换 YUM 软件包源

>CentOS Linux 7 的生命周期（EOL）于 2024 年 6 月 30 日终止。了解红帽帮助您轻松迁移的选项，包括支持第三方 Linux 迁移的红帽企业 Linux。

```zsh
sudo curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sudo yum clean all
sudo yum makecache
```
## 安装 epel-release

```zsh
yum install epel-release -y
```
# rzsz 配置

我们上面在客户端和服务器之间传输文件都是通过 sftp 软件实现的，相对不方便。可以使用 lrzsz 库在命令行里直接上传和下载文件

```zsh
yum install -y lrzsz
```
# 命令行配置
## ZSH
### 下载包

```zsh
yum install -y zsh
```
### 设置为默认终端

```zsh
# ~/.bash_profile 添加如下内容
export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
```
### 验证

```zsh
source ~/.bash_profile
echo $SHELL
/usr/bin/zsh # 注意必须要看到输出这个。不然会导致登录不了的。
```

如果输出为/usr/bin/zsh 则代表安装成功
## 安装 Oh My sh

参见：[Linux 终端美化 - Oh My Zsh - 简书](https://www.jianshu.com/p/b8a80dd59414)
### 下载并安装

```zsh
yum install -y git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

如果下载不下来就去 [install.sh](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh) 手动下载并上传服务器后执行该文件

```zsh
sh install.sh
```
### 配置插件和主题
#### 将下面配置复制到服务器的 ~/.zshrc 文件中

```
source ~/.bash_profile


#zsh
export ZSH="$HOME/.oh-my-zsh"
#ZSH_THEME="robbyrussell"
ZSH_THEME="taw-ys-conda"

plugins=(
git
zsh-autosuggestions
zsh-syntax-highlighting
)

source $ZSH/oh-my-zsh.sh
```
#### 将 下面文件上传到到服务器~/.oh-my-zsh/themes 文件夹中

![[taw-ys-conda.zsh-theme]]
#### 修复 zsh 插件不存在

如果登录 shell报：
`[oh-my-zsh] plugin 'zsh-autosuggestions' not found`
`[oh-my-zsh] plugin 'zsh-syntax-highlighting' not found`
执行

```zsh
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting
```
#### 重新加载终端

```zsh
source ~/.zshrc
```

![Pasted image 20240602163113.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240602163113.png)
# 扩展仓库（EPEL）

```zsh
sudo yum install -y epel-release
```
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
# 开发软件配置
## 常用

```zsh
yum install -y vim wegt unzip docker
```
## Node

```zsh
yum install -y nodejs npm
node -v
```
## npm 换源设置

>一些文章还是写着旧的淘宝 [NPM 镜像](https://so.csdn.net/so/search?q=NPM%20%E9%95%9C%E5%83%8F&spm=1001.2101.3001.7020) `registry.npm.taobao.org`，但它已于 2022 年 05 月 31 日 废弃，读者需要更换为新的 `registry.npmmirror.com` 源。

```zsh
npm config set registry=https://registry.npmmirror.com
npm config get registry
```
## Nginx


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/001/obsidian-web/#nginx" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


</div></div>
