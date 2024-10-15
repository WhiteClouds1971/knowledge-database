---
{"dg-publish":true,"permalink":"/TP02 开发库/Spring Security 允许接口跨域请求/","dgPassFrontmatter":true,"created":"2023-09-11T15:10:24.852+08:00","updated":"2024-06-01T10:50:08.554+08:00"}
---

#跨域
# 允许接口跨域请求

\介绍如何允许接口进行跨域请求，以便其他域名的客户端可以访问您的接口资源。

# 使用@CrossOrigin注解

在Spring Boot框架中，您可以使用`@CrossOrigin`注解来配置CORS（跨源资源共享）策略。以下是一个示例，演示如何使用`@CrossOrigin`注解允许所有来源的跨域请求：

```kotlin
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import javax.servlet.http.HttpServletResponse;

@CrossOrigin(origins = "*")
@GetMapping("/api/open/statics")
fun renderStaticHTML(@RequestParam caseId: String, response: HttpServletResponse) {
    // ...
}
```
{ #2e6c14}

