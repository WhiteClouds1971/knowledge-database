---
{"dg-publish":true,"permalink":"/tp-311-1/spring-jpa/","dgPassFrontmatter":true,"created":"2023-08-03T15:29:22.350+08:00","updated":"2024-06-01T10:49:58.526+08:00"}
---

# Spring JPA - 映射 MySQL 的 Text 数据类型

在 Spring 中，使用 `@Lob` (Large Object) 注解可以将 MySQL 中的 `text` 数据类型映射到 Java 实体类。

`text` 数据类型通常用于存储较长的文本数据，可以容纳较大的字符或文本内容。在 MySQL 中，`text` 类型有多个变种，如 `TEXT`、`MEDIUMTEXT`、`LONGTEXT` 等，它们具有不同的最大长度。

在 Spring 的 JPA (Java Persistence API) 中，使用 `@Lob` 注解可以将 Java 实体类中的字段映射到数据库的 `text` 数据类型。
# 示例用法

```java
import javax.persistence.*;

@Entity
@Table(name = "your_table_name")
public class YourEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Lob // 使用 @Lob 注解将字段映射到数据库的 text 类型
    @Column(name = "your_text_column", columnDefinition = "TEXT") // 根据实际情况指定列定义
    private String yourTextData;

    // Getter 和 Setter 方法
}
