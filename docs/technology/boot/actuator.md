# springboot整合actuator

在springboot整合actuator遇到了一些问题，特意在此记录一下

Spring Boot Actuator 提供了生产环境中所需的监控和管理功能，例如健康检查、指标收集、环境信息等。通过 Actuator，你可以轻松地获取应用程序的运行状态，并进行故障排查

在Maven项目中添加依赖

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

但是在添加spring-boot-starter-actuator依赖后会报错

`Failed to start bean 'documentationPluginsBootstrapper'; nested exception is java.lang.NullPointerException`

首先给出的解决方案是更改路径匹配策略，但是没有用

```java
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```

此时发现我用的是Knife4j 的 OpenAPI 2.x 版本，OpenAPI 2.x 版本是为 Spring Boot 2.5.x 及以下设计的，与 Spring Boot 2.7.x 存在兼容性问题，所以便将OpenAPI 2.x 升级到了3.X版本。升级至3.X版本后一些地方也需要更改（包括原有的一些注解的写法和SwaggerConfig配置类），创建新的配置类 `OpenApiConfig`（替换原有 `SwaggerConfig`）

```java
package com.als.Beans.Blogs.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OpenApiConfig {
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("API文档")
                        .description("API文档描述")
                        .version("1.0")
                );
    }
}
```

此时便可正常启动项目，通过http://host:port/context-path/actuator访问
