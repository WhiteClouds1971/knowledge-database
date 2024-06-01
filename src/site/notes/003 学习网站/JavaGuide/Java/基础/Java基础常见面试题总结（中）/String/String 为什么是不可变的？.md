---
{"dg-publish":true,"permalink":"/003/java-guide/java//java/string/string/","dgPassFrontmatter":true,"created":"2024-05-06T10:13:05.855+08:00","updated":"2024-06-01T10:47:58.951+08:00"}
---

`String` 类中使用 `final` 关键字修饰字符数组来保存字符串，~~所以 `String` 对象是不可变的。~~

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
	private final char value[];
	//...
}	
```

🐛 修正：

我们知道被 `final` 关键字修饰的类不能被继承，修饰的方法不能被重写，修饰的变量是基本数据类型则值不能改变，修饰的变量是引用类型则不能再指向其他对象。因此，`final` 关键字修饰的数组保存字符串并不是 `String` 不可变的根本原因，因为这个数组保存的字符串是可变的（`final` 修饰引用类型变量的情况）。
{ #80c6dd}


`String` 真正不可变有下面几点原因：
{ #9528da}


1. 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。
2. `String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。

补充：

在 Java 9 之后，`String`、`StringBuilder` 与 `StringBuffer` 的实现改用 `byte` 数组存储字符串。

```java
public final class String implements java.io.Serializable,Comparable<String>, CharSequence {
	    // @Stable 注解表示变量最多被修改一次，称为“稳定的”。
	    @Stable
	    private final byte[] value;
}
	
abstract class AbstractStringBuilder implements Appendable, CharSequence {
	    byte[] value;	
}
```

**Java 9 为何要将 `String` 的底层实现由 `char[]` 改成了 `byte[]` ?**
{ #f5d754}


> 新版的 String 其实支持两个编码方案：Latin-1 和 UTF-16。如果字符串中包含的汉字没有超过 Latin-1 可表示范围内的字符，那就会使用 Latin-1 作为编码方案。Latin-1 编码方案下，`byte` 占一个字节(8 位)，`char` 占用 2 个字节（16），`byte` 相较 `char` 节省一半的内存空间。
> 
> JDK 官方就说了绝大部分字符串对象只包含 Latin-1 可表示的字符。
> 
> ![Pasted image 20240506102848.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240506102848.png)
>
> 如果字符串中包含的汉字超过 Latin-1 可表示范围内的字符，`byte` 和 `char` 所占用的空间是一样的。
> 
> 这是官方的介绍：[https://openjdk.java.net/jeps/254](https://openjdk.java.net/jeps/254) 。

