---
{"dg-publish":true,"permalink":"/TP01 开发组件/Selenium 部署/","dgPassFrontmatter":true,"created":"2024-10-28T16:44:36.470+08:00","updated":"2024-10-31T15:44:40.913+08:00"}
---


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
# 拉取镜像

```zsh
docker pull selenium/standalone-chrome
```

我折腾了很久，挂梯子、换镜像源，反正这个镜像一直拉不下来。最后还是通过别人的电脑拉下来的，然后通过`docker save`命令导成压缩包发给我，压缩包可以在这里下载：

[File Browser](https://fcloud.whiteclouds.work/files/appstoragehub/obsidian-large%20-attachments/standlone-chrome-amd64.zip)

下载之后，上传压缩包到服务器通过`load`命令导入镜像：

```zsh
docker load < standlone-chrome-amd64.zip
```

![Pasted image 20241029092057.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241029092057.png)
# 启动容器

```zsh
docker run -d --restart=always --name selenium-standalone-chrome -p 4446:4444 -p 7900:7900 --shm-size="2g" selenium/standalone-chrome:latest
```

访问 ip: 4446 即可看到 dashboard：

![Pasted image 20241029093247.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020241029093247.png)

sessions:代表创建了几个会话。max.concurrentcy: 代表你这个容器同时可以处理几个对话。  图中这种情况只能 同时模拟一个网页，也就是同时只能处理一个会话。
# 代码调用
## Java

```kotlin
val options: ChromeOptions = ChromeOptions()  
options.addArguments("--remote-allow-origins=*")
options.addArguments("--ignore-certificate-errors")
val driver = RemoteWebDriver(URL("http://IP:4446/wd/hub"),options)
try{
  driver.get("www.baidu.com")
  Thread.sleep(10000) // 等待10秒
}finally{
  driver.quit()
}
```

注意使用完之后一定要执行`driver.quit()`关闭连接。