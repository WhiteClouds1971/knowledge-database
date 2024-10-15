---
{"dg-publish":true,"permalink":"/TP02 开发库/Spring @Value的两种用法/","dgPassFrontmatter":true,"created":"2024-04-15T11:19:03.487+08:00","updated":"2024-08-05T09:50:49.241+08:00"}
---

```Java
import org.springframework.beans.factory.annotation.Value

// 引用资源文件夹下的文件不加${}
@Value("classpath:init/task_process.json")  
private lateinit var initProcessJson: Resource

// 引用配置
@Value("\${default.holder:defaultValue}")  
private lateinit var defaultHolder: String
```