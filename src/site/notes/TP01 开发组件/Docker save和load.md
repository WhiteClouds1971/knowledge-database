---
{"dg-publish":true,"permalink":"/TP01 开发组件/Docker save和load/","dgPassFrontmatter":true,"created":"2024-11-28T11:37:20.482+08:00","updated":"2024-11-28T16:18:41.879+08:00"}
---

# docker save

单独导出一个镜像的压缩包：

```zsh
docker save [imageId]/image:tag > image.zip
```

多个镜像一起导出为压缩包

```zsh
docker save -o image.zip image1:tag imageId2 imageId3
```

注意如果使用 imageId 来导出压缩包的话，那么 docker load 进去的镜像都是没有名字的，还需要手动指定镜像名称：

```zsh
docker tag [镜像id] [新镜像名称]:[新镜像标签]
```
# docker load

压缩包导入本地镜像仓库

```zsh
docker load < image.zip
```
