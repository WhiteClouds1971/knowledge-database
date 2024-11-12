---
{"dg-publish":true,"permalink":"/001 精选文章/Vite+Vue3项目搭建/","dgPassFrontmatter":true,"created":"2024-11-08T09:24:28.894+08:00","updated":"2024-11-12T13:57:20.566+08:00"}
---

最近和小伙伴线下面杀游玩的时候，有些操作实在难以实现，便有了开发一款移动端 Web 面杀辅助工具的想法。对于前端方面，我之前写过一段时间的桌面端 Web 应用。对于移动端几乎没有什么经验，也不知道最后能写成什么样。不过本着 先实现、在抽离、在抽象 的原则，先启动再说。

> GitHub 仓库：[WhiteClouds1971/mstools · GitHub](https://github.com/WhiteClouds1971/mstools)

>项目演示网址：[MsTools](https://mstools.whiteclouds.work/)

本项前端使用 Vite、Vue3、vantUI 开发，目前暂无后端支持。
# git
## 初始化本地仓库

```zsh
git init
git branch -m master
git checkout -b develop
git flow init
```
## 添加.gitignore

将下面内容添加到.gitignore 文件中：

```
# Logs  
logs  
*.log  
npm-debug.log*  
yarn-debug.log*  
yarn-error.log*  
pnpm-debug.log*  
lerna-debug.log*  
  
node_modules  
dist  
dist-ssr  
*.local  
  
# Editor directories and files  
.vscode/*  
!.vscode/extensions.json  
.idea  
.DS_Store  
*.suo  
*.ntvs*  
*.njsproj  
*.sln  
*.sw?  
!public/static/vditor/dist
```
## 关联 Github 仓库

```zsh
git remote add origin git@github.com:****/mstools.git
git push --set-upstream origin develop
git checkout master
git push --set-upstream origin master
```

![Pasted image 20241108095740.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108095740.png)

# Prettier

> 对于一个多人协同开发的项目而言，**统一**的代码格式化风格是非常重要的。
## 安装

```zsh
npm install --save-dev prettier
```
## .prettierrc.json

在根目录下创建.prettierrc.json 文件并将下面内容复制进去：

```zsh
{  
  "printWidth": 80,  
  "trailingComma": "es5",  
  "tabWidth": 2,  
  "semi": true,  
  "singleQuote": true,  
  "vueIndentScriptAndStyle": true,  
  "arrowParens": "avoid",  
  "bracketSpacing": true,  
  "htmlWhitespaceSensitivity": "ignore"  
}
```
## 配置 WebStrom

![Pasted image 20241108125831.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108125831.png)
# Vue

```zsh
npm create vite@latest mstools
cd mstools
npm install
npm run dev
```
## 修改 icon 和 title

![Pasted image 20241108104118.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108104118.png)
## 配置路径别名和绑定服务端口

![Pasted image 20241108105519.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108105519.png)
## Vite 环境变量


## 路由配置

### 安装 vue route

```zsh
npm install vue-router 
```
### 封装路由

![Pasted image 20241108110530.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108110530.png)

下面简单介绍一下上面的 index.js、routes.js 和 main.js

index.js 主要是创建了一个 Router 对象。导入 routes.js 中将个个模块的路由聚合在一起的路由数组。

routes.js 主要的功能是将我之后每个模块开发的路由统一聚合成一个数组，这个文件我们后续开发过程中还会继续用到。

main.js 里就是使用 use 函数挂载 route 对象。
## 环境变量

![Pasted image 20241108130135.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108130135.png)

Vite 的环境变量类似于 springboot 的配置文件。通用配置写到 .env 文件中，不通环境的配置写到对应的 .evn.** 文件中。

然后在 vue 文件中可以通过以下方式使用环境变量：

```vue
import.meta.env.APP_BASE_API
```
## 反向代理

目前我们的项目没有后端支持，不过先在这里配置一下。

编辑 vite.config.js 文件如下

![Pasted image 20241108130845.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108130845.png)

## 修改 App.vue 文件

vite 脚手架自动帮我们创建的 App.vue 文件只是一个演示文件，并不能满足我们的实际开发需求。所以我们将 App.vue 文件调整如下：

```vue
<template>  
  <router-view></router-view>  
</template>  
<script setup>  
</script>  
<style scoped></style>
```

![Pasted image 20241108111954.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108111954.png)
# Vant UI

在 UI 这方面，我原本是打算从 ElementUI 和 AntUI 里挑一个出来用的。但是这两个毕竟是桌面端 UI 组件库，不太适合移动端开发。搜索了一阵发现有一个 VantUI 挺符合我的需求的，就它了。

>VantUI 官网：[Vant 4 - A lightweight, customizable Vue UI library for mobile web apps.](https://vant-ui.github.io/vant/#/zh-CN/home)
## 安装

```zsh
npm i vant
```
## 自动按需组件注册

vant 提供了局部注册、全局注册、按需注册等多种组件注册方案，我们这里只介绍主流的自动按需注册方案。

>[Vant 4 - A lightweight, customizable Vue UI library for mobile web apps.](https://vant-ui.github.io/vant/#/zh-CN/quickstart#fang-fa-er.-an-xu-yin-ru-zu-jian-yang-shi)

### 安装插件

```zsh
npm i @vant/auto-import-resolver unplugin-vue-components unplugin-auto-import -D
```

在基于 Rsbuild、Vite、webpack 或 vue-cli 的项目中使用 Vant 时，可以使用 [unplugin-vue-components](https://github.com/unplugin/unplugin-vue-components) 插件，它可以自动引入组件。

Vant 官方基于 `unplugin-vue-components` 提供了自动导入样式的解析器 [@vant/auto-import-resolver](https://github.com/youzan/vant/tree/main/packages/vant-auto-import-resolver)，两者可以配合使用。
## 配置 vite.config.js 文件

完成配置后的 vite.config.js 文件：

```js
import path from 'path';

import {defineConfig} from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite';
import Components from 'unplugin-vue-components/vite';
import {VantResolver} from '@vant/auto-import-resolver';

// https://vite.dev/config/
export default defineConfig(({mode}) => {

    return {
        plugins: [
            vue(),
            AutoImport({
                resolvers: [VantResolver()],
            }),
            Components({
                resolvers: [VantResolver()],
            }),
        ],
        resolve: {
            // https://cn.vitejs.dev/config/#resolve-alias
            alias: {
                // 设置路径
                '~': path.resolve(__dirname, './'),
                // 设置别名
                '@': path.resolve(__dirname, './src'),
            },
        },
        server: {
            port: 9453,
            host: '0.0.0.0',
        },
    }
})
```
# Less

```zsh
npm install less --save-dev
```
# Iconfont

将从 iconfont 上下载的文件复制到项目中：

![Pasted image 20241108195436.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108195436.png)

在 main.js 中导入 iconfont.css

![Pasted image 20241108200526.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108200526.png)
# 测试主页开发

至此，现在的项目便能够支持我们开发一些简单的业务功能了。我们这里开发一下简单的主页，然后给他部署的服务器上测试一下。

1. 在 src/page 下创建一个 Index.vue

![Pasted image 20241108132839.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108132839.png)

2. 在 router/index.js 里添加相关路由

![Pasted image 20241108132919.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108132919.png)

3. 访问网址

![Pasted image 20241108132943.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241108132943.png)
# 线上环境部署

因为我们这项目就是自己开发着玩的，没有什么太重要的东西。因此我就不分生产，测试环境了。我只统一部署了一个 uat 环境即可。

具体里部署教程可以参考 [[001 精选文章/Obsidian Web笔记搭建\|Obsidian Web笔记搭建]]这篇文件。

部署测试完成之后，之后我们就可以进入正式的业务开发了。

