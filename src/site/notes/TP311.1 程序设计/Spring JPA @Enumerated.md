---
{"dg-publish":true,"permalink":"/TP311.1 程序设计/Spring JPA @Enumerated/","dgPassFrontmatter":true,"created":"2024-07-09T16:36:58.113+08:00","updated":"2024-07-09T17:34:13.698+08:00"}
---

```java
@Column(name = "sex") 
@Enumerated(EnumType.ORDINAL)//性别字段持久化为0，1 
private Sex sex; 

@Column(name = "type") 
@Enumerated(EnumType.STRING)//枚举字符串 
private Type type;
```