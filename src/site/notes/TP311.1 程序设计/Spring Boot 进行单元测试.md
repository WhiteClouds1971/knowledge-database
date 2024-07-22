---
{"dg-publish":true,"permalink":"/TP311.1 程序设计/Spring Boot 进行单元测试/","dgPassFrontmatter":true,"created":"2024-07-12T15:48:09.418+08:00","updated":"2024-07-12T15:52:52.788+08:00"}
---

```kotlin
package com.swanlab.pythonlab.wallet.service  
  
import com.swanlab.pythonlab.PythonLabServiceApplication  
import com.swanlab.pythonlab.wallet.core.transaction.em.TransactionChannel  
import com.swanlab.pythonlab.wallet.feo.request.TransactionCriteriaRequest  
import org.junit.Test  
  
import org.junit.runner.RunWith  
import org.springframework.beans.factory.annotation.Autowired  
import org.springframework.boot.test.context.SpringBootTest  
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner  
  
@RunWith(SpringJUnit4ClassRunner::class)  
@SpringBootTest(  
    classes = [PythonLabServiceApplication::class],  
    webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT  
)  
class TransactionOrderServiceTest {  
    @Autowired  
    private lateinit var transactionOrderService: TransactionOrderService  
  
    @Test  
    fun criteriaBasedFind() {  
        transactionOrderService.criteriaBasedFind(  
            TransactionCriteriaRequest().apply {  
                this.number = ""  
                this.channel = TransactionChannel.WECHAT  
            }  
        )  
    }  
}
```