# 分布式demo
## 新建parent服务
- 使用 springboot init
- 打包类型pom
- 版本信息：springboot:2.3.12.RELEASE;springcloud:Hoxton.SR12;springcloudali:2.2.6.RELEASE
## 新建子服务-1
- 通过上述parent工程名，右键新建modules
- 在弹出的浮层中左侧选择 New Module
- 在resource文件夹下新建application.yml，指定端口号，注意yaml语法，属性与值直接有空格
- 添加application启动类；添加@bean RestTemplate
- 添加controller类，注入 restTemplate ,通过getForObject调用另一个服务
## 新建子服务-2
- 通过上述parent工程名，右键新建modules
- 在弹出的浮层中左侧选择 New Module
- 在resource文件夹下新建application.yml，指定端口号，注意yaml语法，属性与值直接有空格
- 添加controller类，暴露服务
# springcloudali
## 整合
添加对ali&Spring原生cloud的依赖（会混用两者的组件），注意cloud与各组件版本的关系。依赖配置放到dependencyManagement中，因为
不继承parent的版本信息，所以使用management。
```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>${spring-cloud-alibaba.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

```
## Nacos-注册中心
Eureka是Netflix中的注册中心
### 功能
- 一致性协议（AP+CP）
- 健康检查
- 负责均衡策略
- 雪崩保护
- 自动注销实例
- 访问协议（http dns）
- 监听支持
- 多数据中心
- 跨注册中心同步
### docker服务启动
使用低配置aws实例，起来后资源卡死;本地（mac m2）使用个人镜像启动
```
docker run --name nacos-quick -e MODE=standalone -p 8849:8848 -d moese/nacos-server-m1:latest
```
### 微服务集成nacos
- 微服务添加nacos依赖
```
       <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
```
- 分别copy一份现有的服务调用和服务提供微服务
- 在配置文件中以下添加nacos配置
```
spring:
  application:
    name: order-service
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8849
        username: nacos
        password: nacos
        namespace: public
```
- 调用方 restTemplate添加@LoadBalanced依赖（否则识别不了被调用方的服务名）
- restTeplate调用方式由ip:port/接口改为服务名/接口
#### 同一微服务启动不同 端口 方法
- 在底部 services tab中选择需要的微服务，copy 微服配置，overrice configuration properties,新增 server.port 变量值
## Ribbon
客户端负责均衡工具，nacos已默认添加Ribbon依赖，无需单独添加。默认轮训策略，如果分区域部署，默认就近区域可用节点。
- RandomRule
- RoundRobinRule
- RetryRule
- WeightedRusponseTimeRule
- BestAvailableRule
### 修改默认负责策略
- 在非Application所在包新建configuration配置类
- 返回IRule 接口实现类
- 在Application启动类中配置负载注解
```
@RibbonClients(value = {@RibbonClient(value = "stock-service",configuration = RibbonRandomConfig.class)})

```
## Feign Netflix微服务调用组件
- 添加依赖，新版本springcloud移除ribbon默认负载类，需要添加loadbalancer依赖
```
       <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--openfeigh start-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
```
- Application添加注解：@EnableFeignClients
- 新建调用接口类,添加注解 name为被调用方nacos服务名称，path为调用接口path
```
@FeignClient(name="stock-service", path="/stock")
public interface StockFeignService {
    @RequestMapping("/order")
    public String stock() ;
}
```
### 日志配置
- 新建配置类,以下为全局配置类，
- 注意：该级别代表的是日志打印信息详细与否，因springboot有日志的对应级别，还需将其日志级别设置为合适的级别
```
logging:
  level:
    com.springcloud.feign: debug
```
```
@Configuration
public class FeignConfig {
    @Bean
    public Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```
### 超时时间
```
    #以下是局部配置，对被调用方 stock-service有效
feign:
  client:
    config:
      stock-service:
        logger-level:  BASIC
        connect-timeout: 2000
        read-timeout: 2000
```
### 拦截器
- 配置类
```
public class FeignInterceptorConfig implements RequestInterceptor {
    Logger logger = LoggerFactory.getLogger(this.getClass());
    @Override
    public void apply(RequestTemplate requestTemplate) {
        logger.info("interceptor");
        requestTemplate.header("xxx","xxx");

```
将上述拦截器的实现，添加都配置类中
```
    /**
     * 自定义拦截器
     * @return
     */
    @Bean
    public FeignInterceptorConfig feignAuthRequestInterceptor(){
        return new FeignInterceptorConfig();
    }
```
- 配置文件
```
    #以下是局部配置，对被调用方 stock-service有效
feign:
  client:
    config:
      stock-service:
        logger-level:  FULL
        connect-timeout: 2000
        read-timeout: 2000
        request-interceptors:
          com.springcloud.feign.interceptor.FeignInterceptorConfig
```
## Nacos-config配置中心
- 添加依赖
```
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
```
- 添加配置文件，注意：好像config只能 配置在bootstrap.properties 文件中才能读取到。注意config的application name:xxx,
- 与nacos配置中data id的对应关系。
- 配置文件data id默认是properties后缀，可通通过file extention配置非默认的data id 文件后缀
- 可通过shared-config配置自定义data id，去除Application name和data id的默认关系

```
spring.application.name=order-nacos-config
spring.cloud.nacos.config.server-addr=127.0.0.1:9849
```
## Sentinel （hystrix,已停更，springcloud netflix）
## Seata-分布式事务
事务模式：AT 、TCC、SAGA、XA；AT是阿里首推的模式。
三个角色：
- TC(Transaction Coordinator)事务协调者，维护全局和分支事务的状态，驱动全局事务提交或回滚
- TM(Transaction Manager)事务管理器，定义全局事务的范围：开始全局事务、提交或回滚全局事务
- RM(Resource Manager)资源管理器，管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚
## GateWay(早期版本：zuul)
- 引入,有冲突，需要排除web
```
   <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```
- 配置文件
```
server:
  port: 8090
spring:
  application:
    name: order-gateway
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8849
        username: nacos
        password: nacos
        namespace: public
    gateway:
      #路由规则
      routes:
        - id: order-route #路由唯一标识
          uri:
          predicates: lb://order-ribbon #lb:使用nacos本地负载策略
            - Path=/order-serv/**
          filters:
            - StripPrefix=1 #转发之前去掉第一层路径
     # discovery:
     #   locator:
       #   enabled: true #是否启动自动识别nacos服务

```
### 断言
使用断言，对当前请求进行匹配,可跟正则。
- 基于Datetime类型的断言
- 基于Header
  - Header=X-Request-Id, \d+
- Method
  - Method=GET
- Query 参数
   - Query=name,dly|ddd
- 自定义断言工厂
    - 约定为类需添加 RoutePredicateFactory为结尾
    - 继承AbstractRoutePredicateFactory
- 自定义过滤器
### 整合sentinel
- 添加依赖
```
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-sentinel-gateway</artifactId>
        </dependency>
```
## skywalking
国产开源框架，是分布式系统的应用程序性能监控工具，2017年加入Apache孵化器。包括分布式追踪、性能指标分析、服务依赖分析等。
