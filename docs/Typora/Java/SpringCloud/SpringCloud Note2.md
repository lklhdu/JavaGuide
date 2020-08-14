# SpringCloud Note2

## ribbon+restTemplate

**服务间调用方式的一种**

**ribbon是一个负载均衡客户端，可以很好的控制http和tcp的一些行为**

1. 在`application.yml`文件中指定服务的**注册中心地址**

1. 在工程的启动类中,通过`@EnableDiscoveryClient`向服务中心注册；并且向程序的ioc注入一个`bean: restTemplate`;并通过`@LoadBalanced`注解表明这个restRemplate开启负载均衡的功能。

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   @EnableDiscoveryClient
   public class ServiceRibbonApplication {
   
       public static void main(String[] args) {
           SpringApplication.run( ServiceRibbonApplication.class, args );
       }
   
       @Bean
       @LoadBalanced
       RestTemplate restTemplate() {
           return new RestTemplate();
       }
   }
   ```

1. 该工程中的服务就可以通过已经注入ioc容器中的restTemplate来消费其他工程上的服务

### 参考资料

1. [方志朋-SpringCloud教程第2篇：Ribbon](https://www.fangzhipeng.com/springcloud/2018/08/02/sc-f2-ribbon.html)