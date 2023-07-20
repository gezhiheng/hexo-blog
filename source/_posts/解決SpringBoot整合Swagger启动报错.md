---
title: 解决SpringBoot整合Swagger启动报错
date: 2023-07-19 16:30:39
tags: ['Spring', 'Swagger']
---
高版本的SpringBoot在整合Swagger时候，有可能会报空指针异常的问题
```
2023-07-19 15:15:04 [main] ERROR org.springframework.boot.SpringApplication - Application run failed
org.springframework.context.ApplicationContextException: Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException
```
## 解决方案
配置WebMvcConfigurer.java
```
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport;


@Configuration
public class WebMvcConfigurer extends WebMvcConfigurationSupport {

    /**
     * 发现如果继承了WebMvcConfigurationSupport，则在yml中配置的相关内容会失效。 需要重新指定静态资源
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**").addResourceLocations(
                "classpath:/static/");
        registry.addResourceHandler("swagger-ui.html", "doc.html").addResourceLocations(
                "classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars/**").addResourceLocations(
                "classpath:/META-INF/resources/webjars/");
        super.addResourceHandlers(registry);
    }

}
```
或者配置文件添加spring.mvc.pathmatch.matching-strategy=ant_path_matcher
```
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```
## 重新启动
重启项目并访问测试
```
http://localhost:8080/swagger-ui.html
```
