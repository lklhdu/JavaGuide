# SpringCloud学习

推荐学习`Greenwich`版

## SpringCloud和SpringBoot版本对应关系

![image-20200723104502636](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200723104507-655988.png)

来源未知网站:joy:

## 1.服务注册与发现

服务注册与发现的组件：`Eureka`

一个`module`工程作为`Eureka Server`，另一个`module`工程作为`Eureka Client`

`Eureka Server`需要在配置文件中注明自己的服务注册中心的地址

## 2.服务间的调用

在微服务架构中，服务与服务的通讯是基于http restful的。

Spring cloud有两种服务调用方式，一种是`ribbon+restTemplate`，另一种是`feign。`

## 2.1Ribbon

ribbon是一个负载均衡客户端，可以很好的控制http和tcp的一些行为。

restTemplate则提供了多种便捷访问远程Http服务的方法。

这篇文章中的`module`工程有：

* `eureka-server`
* 通过修改`service-hi` 端口注册了两个`service-hi`实例
* `service-ribbon` 作为服务消费者

**此时的架构**

![image-20200723140425414](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200723140427-569383.png)

### service-ribbon模块

引入`ribbon`依赖，在启动类中向`ioc`容器注入一个`bean：restTemplate`，并通过`@LoadBalanced`注解开启负载均衡

## 2.2Feign

Feign默认集成了Ribbon，并和Eureka结合，默认实现了负载均衡的效果。

* Feign 采用的是基于接口的注解
* Feign 整合了ribbon，具有负载均衡的能力
* 整合了Hystrix，具有熔断的能力

与`ribbon+restTemplate`相比，`Feign`消费服务的方式要相对简单些

需要定义一个`Feign`接口，并通过注释来指定要调用的服务

## 3.路由网关

**Zuul** 主要功能是路由转发和过滤器

### 路由转发

引入zuul依赖，在启动类上加上相关注解开启功能，然后修改配置文件`application.yml`

首先同样需要指定服务注册中心地址，然后加上路由配置的相关内容`zuul.routes.XXX`

### 过滤器

可以自定义过滤器，做一些安全验证

```java
@Component
public class MyFilter extends ZuulFilter {
 	//重写ZuulFilter中的函数   
}
```

**一个简单的微服务系统架构**

![image-20200723144852760](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200723144854-628332.png)

## 4.分布式配置中心

**作用**：随着线上项目变的日益庞大，每个项目都散落着各种配置文件，如果采用分布式的开发模式，需要的配置文件随着服务增加而不断增多。某一个基础服务信息变更，都会引起一系列的更新和重启。配置中心便是用来解决此类问题。

Spring Cloud Config项目是一个解决分布式系统的配置管理方案。

它包含了Client和Server两个部分，并使用git或svn存放配置文件，默认情况下使用git

* server提供配置文件的存储、以接口的形式将配置文件的内容提供出去
* client通过接口获取数据、并依据此数据初始化自己的应用。

![image-20200723150954784](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200723150955-285238.png)

### config-server模块

1. 引入`spring-cloud-config-server`依赖
1. 在启动类上加`@EnableConfigServer`注解开启配置服务器功能
1. 修改配置文件，添加`git`仓库的信息

### config-client模块

1. 引入`spring-cloud-starter-config`依赖
1. 修改配置文件**bootstrap.properties**，添加配置服务中心的相关信息
1. 成功配置后可以从配置中心读取相关的信息

## 5.高可用的分布式配置中心

可以考虑将配置中心集群化，做成一个微服务，从而达到高可用

![image-20200723151620264](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200723151622-743297.png)

