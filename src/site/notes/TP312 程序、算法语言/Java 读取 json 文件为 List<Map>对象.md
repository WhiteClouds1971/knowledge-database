---
{"dg-publish":true,"permalink":"/tp-312/java-json-list-map/","dgPassFrontmatter":true,"created":"2023-08-29T14:09:49.428+08:00","updated":"2024-06-01T14:21:35.411+08:00"}
---

```kotlin
var translateProvinceMap = listOf<Map<String, String>>()
        // 对去省，市的中英文映射表
        translatedProvinceResource.inputStream.use {
            val mapper = ObjectMapper()
            val type = mapper.typeFactory.constructCollectionType(List::class.java, Map::class.java)
            translateProvinceMap = ObjectMapper().readValue(it, type)
        }
```