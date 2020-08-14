# SpringCloud Note1

## Eureka

**服务注册与发现的组件**

需要一个Eureka Server 以及 一或多个Eureka Client

### Eureka Server

引入以下依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka-server</artifactId>
</dependency>
```

**启动服务注册中心**

在该工程的启动类上加上注解@EnableEurekaServer

**修改配置文件application.yml**

### Eureka client

引入以下依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

**通过注解@EnableEurekaClient 表明自己是一个eurekaclient**

除了该注解外，还需要在配置文件中注明**服务注册中心**的地址

博客的文章和源代码引入的依赖有出入，这篇笔记以源代码为准

### 参考资料 

1. [方志朋-SpringCloud教程第1篇：Eureka（F版本）](https://www.fangzhipeng.com/springcloud/2018/08/01/sc-f1-eureka.html)