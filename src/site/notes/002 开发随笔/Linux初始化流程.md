---
{"dg-publish":true,"permalink":"/002 开发随笔/Linux初始化流程/","dgPassFrontmatter":true,"created":"2024-09-30T10:09:10.911+08:00","updated":"2024-10-15T17:20:43.141+08:00"}
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
$ pbcopy < ~/.ssh/id_rsa.pub
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
yum install lrzsz
```
# 命令行配置
## ZSH
### 下载包

```zsh
yum install zsh
```
### 设置为默认终端

```zsh
# ~/.bash_profile 添加如下内容
export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
```
### 验证

```zsh
[root@ser230473264763 ~]# source ~/.bash_profile
[root@ser230473264763]~# echo $SHELL
/usr/bin/zsh
```

如果输出为/usr/bin/zsh 则代表安装成功
## 安装 Oh My sh

参见：[Linux 终端美化 - Oh My Zsh - 简书](https://www.jianshu.com/p/b8a80dd59414)
### 下载并安装

```zsh
yum install git
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

如果下载不下来就去 [install.sh](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh) 手动下载并上传服务器后执行该文件

```zsh
sh install.sh
```
### 配置插件和主题
#### 将下面配置复制到服务器的 ～/.zshrc 文件中

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
#### 重新加载终端

```zsh
source ~/.zshrc
```

![Pasted image 20240602163113.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240602163113.png)
# 开发环境配置
## Node 环境
### NVM
#### 下载安装

```zsh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
echo "source ~/nvm/nvm.sh" >> ~/.bashrc
source ~/.bashrc
```