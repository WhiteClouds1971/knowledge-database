---
{"dg-publish":true,"permalink":"/TP311.5 软件工程/Pm2 部署node后端项目/","dgPassFrontmatter":true,"created":"2023-07-14T09:11:36.684+08:00","updated":"2024-06-01T10:50:48.094+08:00"}
---

1. 安装PM2
```php
npm install -g pm2
```
2. 在项目目录下创建配置文件pm2.config.json
```json
{
  "name": "meta-puppeteer",        // 应用程序的名称
  "version": "1.0.0",              // 应用程序的版本号
  "script": "index.js",            // 应用程序的入口文件
  "instances": "1",                // 启动的实例数量
  "watch": false,                  // 是否开启文件变动监视
  "max_size": "10M",               // 日志文件的最大大小
  "max_restarts": 5,               // 保留的旧日志文件数量
  "error_file" : "./logs/app-err.log",  // 错误日志文件路径
  "out_file"   : "./logs/app-out.log",  // 标准输出日志文件路径
  "env": {                         // 默认环境变量
    "PORT": "3980"                 // 环境变量设置
  },
  "env_local": {                   // 本地环境变量
  },
  "env_test": {                    // 测试环境变量
  },
  "env_prod": {                    // 生产环境变量
  }
}

```
3. 启动项目时指定配置文件以及运行环境
```php
pm2 start pm2.config.json --env local
```
4. 配置PM2开机自动启动项目
- 运行***pm2 startup***
- 执行命令行输出的命令（sudo env PATH=$PATH:/Users/cloudswhite/.nvm/versions/node/v16.16.0/bin /Users/cloudswhite/.nvm/versions/node/v16.16.0/lib/node_modules/pm2/bin/pm2 startup launchd -u cloudswhite --hp /Users/cloudswhite）