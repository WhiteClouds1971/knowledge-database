---
{"dg-publish":true,"permalink":"/tp-311-1/spring-mvc-handler-interceptor/","created":"2023-09-05T09:31:32.209+08:00","updated":"2024-06-01T10:50:03.181+08:00"}
---

# HandlerInterceptor 简介

HandlerInterceptor 是 Spring 框架中的一个关键组件，用于拦截请求（Controller）的处理流程。它常用于执行一些在请求处理之前或之后需要进行的操作，如身份验证、日志记录、权限控制等。HandlerInterceptor 主要用于拦截请求，而不是修改请求或响应本身。

## HandlerInterceptor 的作用

1. **身份验证**：验证用户身份，确保只有合法用户能够访问特定资源。

2. **日志记录**：记录请求的详细信息，如请求参数、请求路径、处理时间等，用于调试和监控。

3. **权限控制**：检查用户是否有访问特定资源的权限，以确保只有授权用户可以执行某些操作。

4. **缓存控制**：检查缓存，如果有缓存数据，可以直接返回缓存结果，提高性能。

## HandlerInterceptor 的用法

使用 HandlerInterceptor 需要实现 `HandlerInterceptor` 接口，并实现以下三个方法：

1. `preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)`：在请求到达处理方法之前被调用，返回 `true` 表示继续处理，返回 `false` 表示拦截请求。handler 参数表示当前请求所要执行的==处理方法==，通常是一个控制器（Controller）中的处理方法。

2. `postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)`：在处理方法执行之后，视图渲染之前被调用。可修改请求或响应。

3. `afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)`：在请求完成之后被调用，无论处理方法是否抛出异常都会执行。通常用于清理资源或记录日志。

## 配置 HandlerInterceptor

要使用 HandlerInterceptor，需在 Spring 配置文件中配置拦截器，并指定拦截的路径规则。通常，需创建一个类实现 `WebMvcConfigurer` 接口，重写 `addInterceptors` 方法配置拦截器。

```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class MyInterceptor implements HandlerInterceptor {

    // 在请求到达处理方法之前执行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 这里可以进行身份验证，例如检查用户是否登录

        // 如果用户未登录，可以重定向到登录页面
        if (!isUserLoggedIn(request)) {
            response.sendRedirect("/login");
            return false; // 拦截请求
        }

        // 记录请求开始时间
        request.setAttribute("startTime", System.currentTimeMillis());

        // 继续处理请求
        return true;
    }

    // 在处理方法执行之后，视图渲染之前执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
        // 这里可以记录日志，修改响应，或其他后处理操作

        // 记录请求处理时间
        long startTime = (long) request.getAttribute("startTime");
        long endTime = System.currentTimeMillis();
        long executionTime = endTime - startTime;
        System.out.println("Request Execution Time: " + executionTime + " ms");
    }

    // 在请求完成之后执行，无论处理方法是否抛出异常都会执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
            Exception ex) throws Exception {
        // 这里可以进行资源清理，如关闭数据库连接等
    }

    // 检查用户是否已登录（示例方法，需根据实际情况实现）
    private boolean isUserLoggedIn(HttpServletRequest request) {
        // 这里可以根据会话或其他方式判断用户是否已登录
        // 如果已登录，返回 true；否则，返回 false
        return true; // 假设用户已登录
    }
}


@Configuration
public class MyInterceptorConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new MyInterceptor())
                .addPathPatterns("/secured/**") // 拦截的路径
                .excludePathPatterns("/public/**"); // 排除的路径
    }
}
```
