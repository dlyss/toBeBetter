# SpringBoot Tips
1、官网：spring.io

2、配置类：@Configuration

3、spring-boot-autoconfiguration下的meta-info中的 spring.factories配置了自动注入的bean 

4、条件注解@ConditionalOnBean(value=xxx.class)//判断是否存在某个class或名称的类，满足条件时，才生效

5、属性绑定
- 解析classpath下application.properties下的kv值。使用@Value（“${xxx}”）方式获得；亦可通过@PropertySource(value="classpath:xxx.properties")注解在启动Application类中引入
- 可以封装属性类，通过@ConfigurationProperties注解，其他需要的地方注入该属性类即可
- 亦可通过@EnableConfigurationProperties(xxx.class)注解，在配置类中使用
- 使用@ConfigurationPropertiesScan(prefix = "xxx.xxx")，扫描属性配置类

6、profiles机制
- 通过@Profile("xx")注解
- 通过配置不同环境的属性文件：xxx-dev.yml、xxx-prod.yml;启动时，指定属性配置文件：spring.profiles.active=xxx-dev.yml

7、https://www.bilibili.com/video/BV1ta411Q7EQ/?p=24&spm_id_from=pageDriver&vd_source=db1a06bcf7a3419e6b9d5eabd5cd3e78 p24

8、yaml赋值

