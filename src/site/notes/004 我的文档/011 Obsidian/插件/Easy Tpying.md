---
{"dg-publish":true,"permalink":"/004 我的文档/011 Obsidian/插件/Easy Tpying/","dgPassFrontmatter":true,"created":"2024-04-03T16:45:10.363+08:00","updated":"2024-06-01T10:49:12.359+08:00"}
---

>[Obsidian 插件：Easy Tpying 自动格式化你的中英文标点输入格式](https://pkmer.cn/Pkmer-Docs/10-obsidian/obsidian社区插件/easy-typing-obsidian/)
# 自定义输入转换

![Pasted image 20240403164638.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020240403164638.png)
# 自定义不格式化的正则

>[! tip] 详细语法参见：[easy-typing-obsidian/UserDefinedRegExp.md at master · Yaozhuwa/easy-typing-obsidian · GitHub](https://github.com/Yaozhuwa/easy-typing-obsidian/blob/master/UserDefinedRegExp.md)

**需求是：对“.”左右两边不进行空格处理**

搜先 Easy Typing 有三种空格策略分别用三个符号来表示，不要求空格（-），软空格（=），严格空格（+）。其次 Easy Typing 在自定义正则表达式的文本编辑区域，每一行字符串都为一个正则规则，其格式如下：
```
<正则表达式>|<左空格策略><右空格策略>
```

所以 `对“.”左右两边不进行空格处理` 的 Easy Typring正则表达式如下：
```
\.|--
```

