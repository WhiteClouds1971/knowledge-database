---
{"dg-publish":true,"permalink":"/tp-311-1/json-json-property/","dgPassFrontmatter":true,"created":"2024-05-21T16:52:30.030+08:00","updated":"2024-06-01T10:49:45.871+08:00"}
---

```java
import com.fasterxml.jackson.annotation.JsonProperty;

@JsonProperty("apiVersion")  
private String apiVersion = "v1";
```

`@JsonProperty` 是Java中的一个注解，通常在Jackson等库中使用，用来指示一个字段或方法在序列化或反序列化时应该使用特定的名称。