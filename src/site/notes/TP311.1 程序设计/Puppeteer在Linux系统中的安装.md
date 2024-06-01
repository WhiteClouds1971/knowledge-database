---
{"dg-publish":true,"permalink":"/TP311.1 程序设计/Puppeteer在Linux系统中的安装/","dgPassFrontmatter":true,"created":"2023-12-05T15:01:45.343+08:00","updated":"2024-06-01T10:49:48.852+08:00"}
---

项目有一个需求：将Web页面打印成PDF，为了保证功能的稳定可靠。决定单独起一个安装Puppeteer的Node项目，为导出PDF或者图片提供打印服务。

在Mac完成开发测试后，将Puppeteer的Node项目部署到Linux服务器上的时候，遇到了的坑，记录如下。
## 安装环境

1. Linux: Alibaba Cloud Linux release 3
2. Node: v16.18.1
3. Npm: 8.19.2
4. Pm2: 5.3.0
## 安装

```js
// 在创建一个浏览器对象的时候需要禁用沙盒模式
browser = await puppeteer.launch({
			headless: "new", args: ['--no-sandbox', '--disable-setuid-sandbox'],
		})
```

```shell
npm install
# 执行puppeteer的初始化脚本
node node_modules/puppeteer/install.js
```
## 解决缺少依赖库

此时执行打印相关代码Puppeteer会报缺少项目库的错误，一种方案是根据报错提示安装对应的依赖库。另一种方案是把chrome在系统里安装一下，解决所有依赖库。

```shell
sudo yum install chromium
```

至此便可以把PDF打印出来了，但是打开PDF一看，中文全部乱码🥰
## 安装相关中文字体

为了解决中文乱码的问题，我们需要为Linux系统安装相关的中文字体。
1. 安装fontconfig库，
2. 创建chinese文件夹并cd该目录。
3. 将整合好的Fonts.zip字体文件夹上传到服务器该目录（Fonts.zip早不到的话可以直接将Win系统的C:\Windows\Fonts整个压缩）。
4. 解压缩整理一下目录
5. 执行mkfontscale和fc-cache -fv 使字体生效

```shell
yum -y install fontconfig
mkdir -p /usr/share/fonts/chinese
cd /usr/share/fonts/chinese
rz Fonts.zip
unzip Fonts.zip
mv Fonts/* ./
rm -r Fonts
rm Fonts.zip
mkfontscale 
# 如果提示 mkfontscale: command not found，需自行安装 yum install mkfontscale mkfontdir
fc-cache -fv 
#如果提示 fc-cache: command not found，则需要安装 yum install fontconfig
```

最后我们就可以快快乐乐的打印PDF啦😛