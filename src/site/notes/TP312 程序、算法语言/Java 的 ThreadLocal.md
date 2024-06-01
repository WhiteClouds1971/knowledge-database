---
{"dg-publish":true,"permalink":"/tp-312/java-thread-local/","created":"2023-09-04T13:36:58.432+08:00","updated":"2024-06-01T10:50:52.143+08:00"}
---

私有变量 authApplication 是一个 ThreadLocal<String?> 对象，用于在多线程环境中存储线程本地的可空字符串。该对象被命名为 "request_application"。

```Kotlin
private var authApplication: ThreadLocal<String?> = NamedThreadLocal("request_application")
```

上述代码使用了 Kotlin 中的 ThreadLocal 来存储一个名为 authApplication 的线程本地变量，用于存储字符串类型的值。每个线程都会有其自己的 authApplication 实例，因此在不同的线程中，authApplication 的值确实==是不同的对象==。这确保了在不同线程之间不会共享相同的 authApplication 实例，从而防止了线程之间的干扰。

这是因为 ThreadLocal 是基于线程的，每个线程都有自己的 ThreadLocal 变量副本。当您调用 setApplicationId 方法时，它会在当前线程的 authApplication 上设置值，不会影响其他线程的 authApplication。

总之，上述代码可以确保在不同的线程中，authApplication 的值都是不同的对象，因此不会发生线程之间的干扰。
