---
{"dg-publish":true,"permalink":"/tp-311-1/spring-resource/","created":"2023-08-29T11:19:25.206+08:00","updated":"2024-06-01T10:50:05.931+08:00"}
---

# Spring Resource 接口

在 Spring 框架中，`Resource` 是一个接口，用于对各种资源（如文件、类路径、URL 等）进行抽象和统一访问。它提供了一种用于获取资源的标准化方式，无论资源实际存储在何处，都可以通过 `Resource` 接口来进行访问。

`Resource` 接口提供了一系列方法来读取资源的内容、获取资源的元数据等。在 Spring 中，`Resource` 接口的实现类包括了对文件系统、类路径、URL 等不同类型资源的支持。

## 示例

以下是一个简单的示例，展示如何在 Spring 中使用 `Resource` 接口：

```java
import org.springframework.core.io.Resource;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.FileSystemResource;

public class ResourceExample {
    public static void main(String[] args) throws IOException {
        // 通过类路径获取资源
        Resource classPathResource = new ClassPathResource("config.properties");
        if (classPathResource.exists()) {
            InputStream inputStream = classPathResource.getInputStream();
            // 在这里可以读取 inputStream 中的内容
            [[inputStream.close()\|inputStream.close()]];
        }

        // 通过文件系统路径获取资源
        Resource fileSystemResource = new FileSystemResource("C:/path/to/file.txt");
        if (fileSystemResource.exists()) {
            InputStream inputStream = fileSystemResource.getInputStream();
            // 在这里可以读取 inputStream 中的内容
            inputStream.close();
        }
    }
}
