---
{"dg-publish":true,"permalink":"/TP393 计算机网络/跨域（CSRF）/","dgPassFrontmatter":true,"created":"2023-09-06T11:07:21.113+08:00","updated":"2024-06-01T13:13:17.102+08:00"}
---

>source:
>1. [浅谈CSRF攻击方式 - hyddd - 博客园](https://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html)
>2. [前端安全系列（二）：如何防止CSRF攻击？ - 美团技术团队](https://tech.meituan.com/2018/10/11/fe-security-csrf.html)

## 跨域攻击的原理
{ #16b02b}


![Pasted image 20230906111013.png](/img/user/$/$Sys999%20Attachment/Pasted%20image%2020230906111013.png)

从上图可以看出，要完成一次CSRF攻击，受害者必须依次完成两个步骤：
1. 登录受信任网站A，并在本地生成Cookie。
2. 在不登出A的情况下，访问危险网站B。
3. browser在发送请求时会自动带上Cookie（==CSRF攻击是源于WEB的隐式身份验证机制！WEB的身份验证机制虽然可以保证一个请求是来自于某个用户的浏览器，但却无法保证该请求是用户批准发送的！==）
看到这里，你也许会说：“如果我不满足以上两个条件中的一个，我就不会受到CSRF的攻击”。是的，确实如此，但你不能保证以下情况不会发生：
1. 你不能保证你登录了一个网站后，不再打开一个tab页面并访问另外的网站。
2. 你不能保证你关闭浏览器了后，你本地的Cookie立刻过期，你上次的会话已经结束。（事实上，关闭浏览器不能结束一个会话，但大多数人都会错误的认为关闭浏览器就等于退出登录/结束会话了......）
3. 上图中所谓的攻击网站，可能是一个存在其他漏洞的可信任的经常被人访问的网站。

## 跨越攻击的列子

银行网站A，它以GET请求来完成银行转账的操作，如：http://www.mybank.com/Transfer.php?toBankId=11&money=1000

```php
// 银行的请求代码
<?php　　　　
session_start();　　　　
if (isset($_POST['toBankId'] &&　isset($_POST['money'])){　　　　    buy_stocks($_POST['toBankId'],　$_POST['money']);　　　　
}　　
?>
```

```html
// 恶意网站发送的请求
<html>　　
<head>　　　　
<script type="text/javascript">　　　　　　
function steal(){
iframe = document.frames["steal"];　　     　　         iframe.document.Submit("transfer");　　　　　　
}　　　　
</script>　　
</head>　　
<body onload="steal()">　　　　
<iframe name="steal" display="none">　　　　　　<form method="POST" name="transfer"　action="http://www.myBank.com/Transfer.php">　　　　　　　　<input type="hidden" name="toBankId" value="11">　　　　　　　　<input type="hidden" name="money" value="1000">　　　　　　
</form>　　　　
</iframe>　　
</body>
</html>
```

## CSRF的防御

CSRF通常从第三方网站发起，被攻击的网站无法防止攻击发生，只能通过增强自己网站针对CSRF的防护能力来提升安全性。

上文中讲了CSRF的两个特点：
1. CSRF（通常）发生在第三方域名。
2. CSRF攻击者不能获取到Cookie等信息，只是使用。

针对这两点，我们可以专门制定防护策略，如下：
- 阻止不明外域的访问
1. 同源检测
2. Samesite Cookie
- 提交时要求附加本域才能获取的信息
3. CSRF Token
4. 双重Cookie验证

详细防御措施介绍见[前端安全系列（二）：如何防止CSRF攻击？ - 美团技术团队](https://tech.meituan.com/2018/10/11/fe-security-csrf.html)