---
{"dg-publish":true,"permalink":"/011 Obsidian/插件/WebPage Html Export/","created":"2024-04-08T09:30:41.428+08:00","updated":"2024-06-01T10:49:15.256+08:00"}
---

插件 GitHub 仓库：[GitHub - KosmosisDire/obsidian-webpage-export: Export html from single files, canvas pages, or whole vaults. Direct access to the exported HTML files allows you to publish your digital garden anywhere. Focuses on flexibility, features, and style parity.](https://github.com/KosmosisDire/obsidian-webpage-export)
该插件是一个将笔记系统按照目录结构，导出为静态 Html 页面的插件。配合静态资源服务器可以将笔记快速发布到 web。效果图如下：

![Pasted image 20240408093358.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240408093358.png)

> tip：该插件目前依据处于开发中，在使用过程中存在 BUG。不过无伤大雅，整体效果不错。
# 安装

可以直接在插件系统中搜索 WebPage Html Export 安装发行版，但发行版目前功能不完善。建议使用 BATE 插件并添加 `https://github.com/KosmosisDire/obsidian-webpage-export` 插件地址体验最新预览版。

![Pasted image 20240408095312.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240408095312.png)
# 设置

具体详细设置介绍见：[7. Settings](https://docs.obsidianweb.net/getting-started/7.-settings.html)
# 问题

1. 在 web 展示中，所有的标题的 margin-top 都是 40 px 导致页面特别丑。可是将以下代码放入 custom header 中，更改标题的 margin 值。

```html
<html>  
<head>  
    <style>  
        body h1, h2, h3, h4, h5, h6 {  
            margin: 4px !important;  
        }  
    </style>  
</head>  
<body>  
  
</body>  
</html>
```

![Pasted image 20240408095011.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240408095011.png)