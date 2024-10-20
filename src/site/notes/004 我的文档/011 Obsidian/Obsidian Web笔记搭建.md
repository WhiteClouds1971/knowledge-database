---
{"dg-publish":true,"permalink":"/004 我的文档/011 Obsidian/Obsidian Web笔记搭建/","dgPassFrontmatter":true,"created":"2024-10-20T10:56:27.654+08:00","updated":"2024-10-20T11:16:36.664+08:00"}
---

> 将 Obsidian 里记录的笔记转换成 HTML 文件，发布到互联网上。

主要思路是通过一个名为`DigItal Garden` 的插件将笔记转换为 node 项目，推送到 GitHub 上，在通过阿里的云效进行自动构建。然后 Frp 进行内网穿透供公网用户访问。

因为之前`DigItal Garden` 的具体使用方法我没记录，我这里直接从云效构建开始。
# 持续集成

云效的详细使用方法参见[[002 开发随笔/迁移云效调研#持续集成\|迁移云效调研#持续集成]]，这章节记录一下一些需要特别注意的地方。
## 添加 GitHub 源

我们的笔记项目在 GitHub 上而不是在云效自己的代码仓库里。所以第一步就要为流水线添加 GitHub 源
### 授权

一路确定即可

![Pasted image 20241020110757.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020110757.png)
### GitHub 仓库添加Webhook

在选择完仓库和分支后需要在 GitHub 上的仓库添加一个 Webhook，在分支发生提交后，通知云效进行持续构建。

![Pasted image 20241020111530.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241020111530.png)

