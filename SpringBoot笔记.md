# SpringBoot Tips
1、官网：spring.io

2、配置类：@Configuration

3、spring-boot-autoconfiguration下的meta-info中的 spring.factories配置了自动注入的bean 

4、条件注解@ConditionalOnBean(value=xxx.class)//判断是否存在某个class或名称的类，满足条件时，才生效

5、属性绑定，
- 解析classpath下application.properties下的kv值。使用@Value（“${xxx}”）方式获得；亦可通过@PropertySource("xxxpath")注解在启动Application类中引入
- 可以封装属性类，通过@ConfigurationProperties注解，其他需要的地方注入该属性类即可



