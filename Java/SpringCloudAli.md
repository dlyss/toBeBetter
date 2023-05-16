# 分布式demo
## 新建parent服务
- 使用 springboot init
- 打包类型pom
## 新建子服务-1
- 通过上述parent工程名，右键新建modules
- 在resource文件夹下新建application.yml，指定端口号，注意yaml语法，属性与值直接有空格
- 添加application启动类；添加@bean RestTemplate
- 添加controller类，注入 restTemplate ,通过getForObject调用另一个服务
## 新建子服务-2
- 通过上述parent工程名，右键新建modules
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
### 集成
- 微服务添加nacos依赖
```
       <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
```
- 分别copy一份现有的服务调用和服务提供微服务
- 在配置文件中添加nacos配置
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

