# springmvc 父项目
## 创建
- 在idea里直接new maven项目，一步步走下来后，将src文件夹删除
- 在pom中添加基本依赖
```
<dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
```
### 创建子项目
- 在父项目右键创建module
- 选择new module，不选任何依赖、框架，直接创建一个简单的maven项目
- 在新创建的子项目中右键选择 add framework support
- 在列表中选择 web application，注意对应的版本号
- 至此，一个原生springmvc空工程建立完毕
### lombok
- 引入
```
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.16</version>
        </dependency>
```
- 使用
```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    String name;
    int age;

}

```
### 乱码问题
```
<!--注册声明过滤器-->
 <filter>
  <filter-name>characterEncodingFilter</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<!--指定项目的编码格式-->
  <init-param>
   <param-name>encoding</param-name>
   <param-value>utf-8</param-value>
  </init-param>
<!--强制请求对象的编码格式 使用字符集encoding-->
  <init-param>
   <param-name>forceRequestEncoding</param-name>
   <param-value>true</param-value>
  </init-param>
<!--强制响应对象使用encoding的字符集编码-->
  <init-param>
   <param-name>forceResponseEncoding</param-name>
   <param-value>true</param-value>
  </init-param>
 </filter>
 
 <filter-mapping>
<!--强制所有请求先通过过滤器处理-->
  <filter-name>characterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
 </filter-mapping>
 ```
 ### fastjson乱码
 - @RestController 修饰的类，返回的都是json格式
 ```
 <bean id="stringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter">
        <constructor-arg value="UTF-8" index="0"/>
        <property name="supportedMediaTypes">
            <list>
                <value>text/plain;charset=UTF-8</value>
            </list>
        </property>
    </bean>

    <!-- fastJson配置 -->
    <bean id="fastJsonHttpMessageConverter" class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
        <property name="supportedMediaTypes">
            <list>
                <value>text/json;charset=UTF-8</value>
                <value>text/html;charset=UTF-8</value>
            </list>
        </property>
    </bean>
    <!-- 请求处理器 -->

    <mvc:annotation-driven>
        <mvc:message-converters>
            <ref bean="fastJsonHttpMessageConverter"/>
            <ref bean="stringHttpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>
```
