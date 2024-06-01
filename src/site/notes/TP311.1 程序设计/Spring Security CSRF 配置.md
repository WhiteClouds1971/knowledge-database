---
{"dg-publish":true,"permalink":"/tp-311-1/spring-security-csrf/","created":"2023-09-12T10:57:11.378+08:00","updated":"2024-06-01T10:50:11.135+08:00"}
---

# Spring Security CSRF 配置

Spring Security 默认情况下只对 `POST` 请求进行 CSRF（跨站请求伪造）保护，而不会对 `GET` 请求进行保护。这是因为 `POST` 请求通常用于执行可能有副作用的操作，而 `GET` 请求通常用于只读操作。如果需要对 `GET` 请求进行 CSRF 保护，您可以通过配置来实现。

## 配置示例

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf()
                .requireCsrfProtectionMatcher(new RequestMatcher() {
                    @Override
                    public boolean matches(HttpServletRequest request) {
                        // 自定义匹配逻辑，决定哪些请求需要进行 CSRF 保护
                        return !request.getMethod().equals("GET");
                    }
                })
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());
        // 其他配置
    }
}
```