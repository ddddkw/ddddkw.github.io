# 微服务整合nacos的过程

Windows或者linux上安装nacos的过程就不在赘述了，这里直接快进到项目中如何注册到nacos服务中

pom文件中需要添加对应的依赖

```java
 <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
```

项目配置文件中首先需要添加nacos的配置内容

```java
    nacos:
      // nacos的账号和密码
      username: ${nacos_username:nacos}
      password: ${nacos_passwd:nacos}
      discovery:
        server-addr: ${nacos_addr:127.0.0.1:8848} // nacos服务地址
        # 如果是public命名空间 namespace不填写,因为他是保留空间, 没有命名空间Id, 若是别的命名空间则加上
        namespace: ${nacos_namespace:}
```

启动类中需要添加注解：`@EnableDiscoveryClient`

此时启动服务的话，便可在nacos服务列表中看到对应的服务
