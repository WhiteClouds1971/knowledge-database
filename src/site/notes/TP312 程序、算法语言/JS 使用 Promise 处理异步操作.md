---
{"dg-publish":true,"permalink":"/TP312 程序、算法语言/JS 使用 Promise 处理异步操作/","dgPassFrontmatter":true,"created":"2023-08-29T09:37:45.546+08:00","updated":"2024-06-01T10:51:00.564+08:00"}
---

# 使用 Promise 处理异步操作

[[Promises\|Promises]] 是 JavaScript 中用于处理异步操作的方法，它们使异步代码更易于编写和管理。

## 创建一个 Promise 对象

```javascript
const myPromise = new Promise((resolve, reject) => {
  // 异步操作
  setTimeout(() => {
    const randomNumber = Math.random();
    if (randomNumber > 0.5) {
      resolve(randomNumber); // 成功时调用 resolve
    } else {
      reject('Random number is too small'); // 失败时调用 reject
    }
  }, 1000); // 模拟一个异步操作，1秒后执行
});
```
