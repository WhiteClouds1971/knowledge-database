---
{"dg-publish":true,"permalink":"/003/java-guide/java//java/object/hash-code/","created":"2024-04-13T10:37:24.966+08:00","updated":"2024-06-01T10:47:47.578+08:00"}
---

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

![Pasted image 20240413103823.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240413103823.png)
<p style="text-align:center; font-size:small;">hashCode() 方法</p>

`hashCode()` 定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是：`Object` 的 `hashCode()` 方法是本地方法，也就是用 C 语言或 C++ 实现的。

>⚠️ 注意：该方法在 **Oracle OpenJDK8** 中默认是 "使用线程局部状态来实现 Marsaglia's xor-shift 随机数生成", 并不是 "地址" 或者 "地址转换而来", 不同 JDK/JVM 值可能不同。在 **Oracle OpenJDK8** 中有六种生成方式 (其中第五种是返回地址), 通过添加 VM 参数: -XX:hashCode=4 启用第五种。
{ #58f793}


```java
public native int hashCode();
```

散列表存储的是键值对(key-value)，它的特点是：**能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）**