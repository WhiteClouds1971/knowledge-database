---
{"dg-publish":true,"permalink":"/TP311.1 程序设计/Spring @Value的两种用法/","created":"2024-04-15T11:19:03.487+08:00","updated":"2024-06-01T10:49:52.028+08:00"}
---

```Java
import org.springframework.beans.factory.annotation.Value

// 引用资源文件夹下的文件不加${}
@Value("classpath:init/task_process.json")  
private lateinit var initProcessJson: Resource

// 引用配置
@Value("\${default.holder}")  
private lateinit var defaultHolder: String
```