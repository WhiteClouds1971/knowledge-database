---
{"dg-publish":true,"permalink":"/tp-311-1/spring-js/","created":"2023-09-11T17:57:38.000+08:00","updated":"2024-06-01T10:49:54.922+08:00"}
---

## Spring

后端通过==response.contentType = "application/javascript"==指定返回的文件是一个JS文件

```kotlin
package com.dashenglab.briefcase.business

import org.springframework.beans.factory.annotation.Autowired
import org.springframework.web.bind.annotation.CrossOrigin
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestParam
import freemarker.template.Configuration
import jakarta.servlet.http.HttpServletResponse
import org.springframework.beans.factory.annotation.Value
import org.springframework.core.io.Resource
import org.springframework.http.MediaType
import org.springframework.web.bind.annotation.RestController

@RestController
@RequestMapping("/open/panels")
class OpenStaticHTMLController {
    @Autowired
    private lateinit var freeMarkerConfig: Configuration

    @Value("classpath:static/panel/index.js")
    private lateinit var panelJs: Resource

    /**
     * 第三方网站根据ID请求一个被FreeMarker渲染后的静态HTML字符串
     */
    @CrossOrigin("*")
    @GetMapping
    fun renderStaticHTML(@RequestParam caseId: String, response: HttpServletResponse) {
        response.contentType = "application/javascript"
        panelJs.inputStream.use {
            it.copyTo(response.outputStream)
        }
        response.flushBuffer()
    }
}
```

## 前端

通过<script/>标签加载的js文件在加载完成后会自动执行

```js
loadJs(src) {
        return new Promise((resolve, reject) => {
          const script = document.createElement('script');
          script.type = 'text/javascript';
          script.src = src;
          script.onload = () => {
            resolve();
          };
          script.onerror = () => {
            reject();
          };
          document.body.appendChild(script);
        });
      },
```