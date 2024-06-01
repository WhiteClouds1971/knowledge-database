---
{"dg-publish":true,"permalink":"/TP311.5 软件工程/Git 分支管理/","dgPassFrontmatter":true,"created":"2023-07-11T16:32:07.185+08:00","updated":"2024-06-01T10:50:30.271+08:00"}
---

- 创建分支
```php
git branch <branch-name>
```
- 切换到分支
```php
git checkout <branch-name>
```
- 创建并切换到分支
```php
git checkout -b <branch-name>
# 创建完分支后必须commit一次代码或者合并其他分支改分支才能关联到一个commitId。这个分支才能创建成功
```
- 查看分支
```php
git branch -a
```
- 合并分支
```php
git merge <branch-name>
```
- 重命名分支
```PHP
git branch -m <old-name> <new-name>
```
- 删除分支
```php
git branch -d <branch-name>
```
- 推送分支到远程仓库
```php
git push -u origin <branch-name>
```
