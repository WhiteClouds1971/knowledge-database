---
{"dg-publish":true,"permalink":"/TP02 开发库/VUE 一个事件绑定多个方法/","dgPassFrontmatter":true,"created":"2023-08-28T11:22:22.163+08:00","updated":"2024-06-01T10:50:14.253+08:00"}
---

## 一个事件绑定多个方法

```vue
<div
	@blur="
	onEditorBlur();
	onInput();
	"
/>
```