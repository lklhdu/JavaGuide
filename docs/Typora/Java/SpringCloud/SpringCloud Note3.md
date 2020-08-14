# SpringCloud Note3

## Hystrix

**服务故障的雪崩：**在微服务架构中，如果单个服务出现问题，由于服务和服务之间的依赖性，故障会传播，会对整个微服务系统造成灾难性的严重后果

**熔断：**当多次访问一个服务失败时，应熔断，标记该服务已停止工作，直接返回错误。直至该服务恢复正常后再重新建立连接。

## 在ribbon上使用熔断器

引入以下依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

在该工程的启动类上加上@EnableHystrix注解开启Hystrix

在服务中加上@HystrixCommand注解。该注解对该方法创建了熔断器的功能，并指定了fallbackMethod熔断方法（自定义）

### 参考资料

1. [方志朋-SpringCloud教程第4篇：Hystrix](https://www.fangzhipeng.com/springcloud/2018/08/04/sc-f4-hystrix.html)