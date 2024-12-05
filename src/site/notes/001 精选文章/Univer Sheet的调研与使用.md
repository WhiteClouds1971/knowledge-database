---
{"dg-publish":true,"permalink":"/001 精选文章/Univer Sheet的调研与使用/","dgPassFrontmatter":true,"created":"2024-11-27T09:49:18.146+08:00","updated":"2024-11-29T14:02:45.668+08:00"}
---

> [Univer Sheets | Univer Sheets](https://docs.univer.ai/zh-CN/guides/sheets)

Univer 是一个用于构建生产力应用程序的全栈框架，目前可以支持在网页上使用 Excel、Doc，PPT 这三个常用的办公工具。

这篇文章主要是结合公司的实际研发需求记录一下 Univer Sheet 的调研与使用。我们需要实现教师在实验指导的文件章节里上传一个 Excel，每个学生使用上传的 Excel 模板编辑出专属于学生的 Excel 文件并回传到后端。
# 使用模板创建 demo

我们需要用的 Excel 的导入和导出，因此我们使用 univer 的高级版功能。

```zsh
bash -c "$(curl -fsSL https://get.univer.ai)"
cd univer-server && bash run.sh start-demo-ui
```

注意这步可能会遇到各种各样的问题，我在本地 mac 上部署的很顺利。但是将测试环境的服务器上部署的时候，遇到了：docker compose 版本过低、镜像拉不下来、docker compose 的网段冲突和服务端口号冲突，基本把坑全踩了一遍👍。

另外，你可以通过`univer-server`目录下的`run.sh`来控制Univer后端服务的停止/启动/重启。

- 停止：`cd univer-server && bash run.sh stop`
- 启动：`cd univer-server && bash run.sh start`
- 重启：`cd univer-server && bash run.sh restart`
- 卸载：`cd univer-server && bash run.sh uninstall`，**注意**，卸载操作将会删除你在体验过程中所创建的文档及其中包含的图片等所有数据，请谨慎操作。

然后启动访问 http://localhost:3010 ，注意高级版右上角就有了自动保存的功能。

最后需要知道的是，我们访问的是官方提供的一个演示 demo 的前端网址，这个对我们的实际用处不大，我们后续真正需要用到的是通过 bash run.sh start 启动的后端服务，这个服务的端口号是: 8000。

![Pasted image 20241129112311.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241129112311.png)
# univer pro的项目集成

[GitHub - dream-num/univer-pro-sheet-start-kit: A starter kit for Univer Pro Features](https://github.com/dream-num/univer-pro-sheet-start-kit) 这个是 univer 官方给的演示 demo 源码，我们可以参考这个项目和官网文档把 univer pro集成到我们自己的项目里，这里记录一下大概过程。

