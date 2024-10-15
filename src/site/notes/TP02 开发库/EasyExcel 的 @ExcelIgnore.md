---
{"dg-publish":true,"permalink":"/TP02 开发库/EasyExcel 的 @ExcelIgnore/","dgPassFrontmatter":true,"created":"2024-04-09T10:00:52.943+08:00","updated":"2024-06-01T10:49:42.387+08:00"}
---

#Excel #导入 #记录

> [!bug] 
> 在使用 EasyExcel 的模板类导入时，需要为模板类中不需要映射的字段增加@ExcelIgnore 注解。否则在导入的时候会识别报错

```kotlin
class ProgressReportExcel {  
    @ExcelProperty("Purchasing Document")  
    var poNumber: String? = null  
  
    @ExcelIgnore
    var poNumberChanged = false
}
```
