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
