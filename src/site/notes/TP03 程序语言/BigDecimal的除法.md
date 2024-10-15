---
{"dg-publish":true,"permalink":"/TP03 程序语言/BigDecimal的除法/","dgPassFrontmatter":true,"created":"2024-08-09T15:50:22.247+08:00","updated":"2024-10-15T17:23:03.417+08:00"}
---

kotlin 的 BigDecimal 的运算符重载 `/` 会根据值自动切换是不是整除。在以后的代码中能用 div 就不要使用 `/`

```java
import cn.hutool.core.util.NumberUtil

BigDecimal("3")/BigDecimal("2") // value is 2

BigDecimal("3.0")/BigDecimal("2") // value is 1.5
BigDecimal("3.0").divide(BigDecimal("2")) // value is 1.5
NumberUtil.div(BigDecimal("3.0"),BigDecimal("2")) // value is 1.5
```