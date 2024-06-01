---
{"dg-publish":true,"permalink":"/TP311.1 程序设计/BigDecimal如何比较数学相等/","created":"2024-04-28T14:48:51.908+08:00","updated":"2024-06-01T10:49:38.785+08:00"}
---

```java
BigDecimal("4800.000") == BigDecimal("4800");
BigDecimal("4800.000").equals(BigDecimal("4800");)
```

上面的两条语句的输出都是 false。BigDecimal 提供的 api 可能并不是太靠谱。建议使用第三方库进行操作。

```java
import cn.hutool.core.util.NumberUtil
NumberUtil.equals(BigDecimal("4800.000"),BigDecimal("4800");)
```

此时上述代码的输出就是 true