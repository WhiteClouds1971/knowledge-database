---
{"dg-publish":true,"permalink":"/TP02 开发库/Redis Ression 在 Spring boot中的集成/","dgPassFrontmatter":true,"created":"2024-07-10T11:13:12.695+08:00","updated":"2024-07-10T11:30:49.344+08:00"}
---

# 引入依赖

```xml
<dependency>  
    <groupId>org.redisson</groupId>  
    <artifactId>redisson-spring-boot-starter</artifactId>  
    <version>3.22.1</version>  
    <exclusions>        
      <exclusion>            
        <groupId>org.redisson</groupId>  
        <artifactId>redisson-spring-data-31</artifactId>  
      </exclusion>    
    </exclusions>
</dependency>  
<dependency>  
    <groupId>org.redisson</groupId>  
    <artifactId>redisson-spring-data-26</artifactId>  
    <version>3.20.0</version>  
</dependency>
```
# 引入 RedissonClient

```kotlin
@Autowired
private lateinit var redissonClient: RedissonClient
```

一般来说，项目里配置了 redis 相关的信息。直接导入依赖即可直接使用 redissonClient 不需要其他额外配置