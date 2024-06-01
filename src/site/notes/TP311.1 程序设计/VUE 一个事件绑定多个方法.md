---
{"dg-publish":true,"permalink":"/tp-311-1/vue/","created":"2023-08-28T11:22:22.163+08:00","updated":"2024-06-01T10:50:14.253+08:00"}
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