---
{"dg-publish":true,"permalink":"/003 学习网站/JavaGuide/Java/基础/Java基础常见面试题总结(下)/异常/Checked Exception和Unchecked Exception有什么区别？/","created":"2024-05-08T10:32:26.187+08:00","updated":"2024-06-01T10:47:06.844+08:00"}
---

**Checked Exception** 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

比如下面这段 IO 操作的代码：

![Pasted image 20240508103648.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240508103648.png)

除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、`ClassNotFoundException`、`SQLException`...。

**Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

`RuntimeException` 及其子类都统称为非受检查异常，常见的有（建议记下来，日常开发中会经常用到）：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ClassCastException`（类型转换错误）
- `ArithmeticException`（算术错误）
- `SecurityException` （安全错误比如权限不够）
- `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)
- ……

![Pasted image 20240508103748.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240508103748.png)