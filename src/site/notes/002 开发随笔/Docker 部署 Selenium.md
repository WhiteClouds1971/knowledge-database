---
{"dg-publish":true,"permalink":"/002 开发随笔/Docker 部署 Selenium/","dgPassFrontmatter":true,"created":"2024-10-28T16:44:36.470+08:00","updated":"2024-10-28T17:06:04.870+08:00"}
---

>参考：[当selenium遇上docker | 晚花行乐](https://www.lfhacks.com/tech/selenium-docker/)

现在有一个项目需要使用 Selenium 来打印 PDF，为了给多个服务提供打印功能，决定单独部署一Selenium 中间间，具体部署流程记录如下。
# 修改 Docker 镜像源

执行执行 docker pull 命令我这边网络错误，所有先给 docker 添加几个中国源。

>[! tip] 注意：自 2024-06-06 开始，国内的 Docker Hub 镜像加速器相继停止服务，可选择为 Docker daemon 配置代理或自建镜像加速服务。

具体哪些镜像源可用可参考：[国内的 Docker Hub 镜像加速器，由国内教育机构与各大云服务商提供的镜像加速服务 | Dockerized 实践 https://github.com/y0ngb1n/dockerized · GitHub](https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6#docker-hub-%E9%95%9C%E5%83%8F%E5%8A%A0%E9%80%9F%E5%99%A8%E5%88%97%E8%A1%A8)

修改完成后可以通过 `docker info` 命令划到最底下查看是否成功

```zsh
docker info
```

![Pasted image 20241028165957.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241028165957.png)
## Mac

```
"registry-mirrors": [
  "https://registry.docker-cn.com"
]
```

![Pasted image 20241028165019.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241028165019.png)