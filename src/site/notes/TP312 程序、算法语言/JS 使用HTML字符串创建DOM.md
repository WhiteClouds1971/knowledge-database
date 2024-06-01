---
{"dg-publish":true,"permalink":"/tp-312/js-html-dom/","dgPassFrontmatter":true,"created":"2023-09-11T15:06:47.791+08:00","updated":"2024-06-01T10:51:03.131+08:00"}
---

# 使用HTML字符串创建DOM

这是一个示例的Markdown笔记，演示了如何使用JavaScript将HTML字符串转换为DOM，并以纯文本形式表示它。

## 创建DOM

要创建DOM，我们可以使用JavaScript的DOMParser对象来解析HTML字符串并生成DOM元素。

```javascript
// 创建一个DOMParser对象
var parser = new DOMParser();

// 要解析的HTML字符串
var htmlString = "<div style='width: 100%; height: 200px; border: black 1px solid'>11111</div>";

// 使用DOMParser解析HTML字符串并创建DOM元素
var doc = parser.parseFromString(htmlString, "text/html");

// 获取新创建的DOM元素
var divElement = doc.body.firstChild;

// 插入DOM
document.body.appendChild(divElement)
```
