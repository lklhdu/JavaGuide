# Spring注解相关

1.  @Value

   通过@Value注解可以注入配置文件中的内容

   ```xml
   /* application.yml */
   server:
   	port: 8090
   ```

   ```java
   @Service
   public class HelloWorldServiceImpl implements HelloWorldService {
   
       @Autowired
       private MyConfig myConfig;
   
       @Value(value = "${server.port}")
       private String port;
   ```

1. @RequestMapping和@GetMapping

    @GetMapping用于将HTTP get请求映射到特定处理程序的**方法注解**。具体来说，@GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)的缩写

1. @RequestParam

   `@RequestParam` 用于从request请求中接收参数

   http://localhost:8080/springmvc/hello/101?param1=10&param2=20

   根据上面的URL，可以通过下面的方式来获取参数

   ```java
   public String getDetails(
       @RequestParam(value="param1", required=true) String param1,
       @RequestParam(value="param2", required=false) String param2){
       ...
   }
   ```

   @RequestParam 支持下面四种参数

   * defaultValue 如果本次请求没有携带这个参数，或者参数为空，那么就会启用默认
   * name 绑定本次参数的名称，要跟URL上面的一样
   * required 这个参数是不是必须的
   * value 跟name一样的作用，是name属性的一个别名
   
1.  @EnableDiscoveryClient与@EnableEurekaClient

    * 如果选用的注册中心是eureka，那么就推荐@EnableEurekaClient
    * 如果是其他的注册中心（zookeeper、consul），那么推荐使用@EnableDiscoveryClient

### 参考资料

1. [Spring boot之@Value注解的使用总结](https://blog.csdn.net/hunan961/article/details/79206291)
1. [@RequestMapping和@GetMapping @PostMapping 区别](https://blog.csdn.net/magi1201/article/details/82226289)
1. [@RequestParam，@PathParam，@PathVariable等注解区别](https://blog.csdn.net/u011410529/article/details/66974974)