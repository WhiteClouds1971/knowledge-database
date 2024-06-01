---
{"dg-publish":true,"permalink":"/TP311.5 软件工程/Git 本地代码托管到 Git平台/","created":"2023-07-11T16:10:44.242+08:00","updated":"2024-06-01T10:50:27.490+08:00"}
---

1. 初始化本地Git仓库
```shell
git init
```
2. 将 GitLab 远程仓库地址添加为远程地址
```shell
git remote add origin <远程仓库地址> #其中 `<远程仓库地址>` 是您在 GitLab 上创建的远程仓库的地址。
```
3. 添加并提交本地代码到本地仓库：
```shell
git add .
git commit -m "Initial commit"
# 不可以使用 git commit -am "Initial commit"代替
```
4. 将本地代码推送到 GitLab 远程仓库的 `develop` 分支：
```shell
git push -u origin master
```