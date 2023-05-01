1、操作系统内核环境查看  
- `uname -r`
- `6.1.23-36.46.amzn2023.x86_64`

2、操作系统版本查看
- `cat /etc/os-release`
# docker基本命令
1、卸载旧版本
- `yum remove docker`

2、安装yum utils
- `yum install -y yum-utils`

3、设置镜像
- `sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

以上是学习视频中摘录的命令，针对aws linux貌似有问题，直接使用下述命令

4、安装docker
- `yum install docker `
- `docker version`
- `docker info`

5、启动docker
- `systemctl start docker`
- `sudo docker run hello-world`

6、常用命令

- 列出当前已有镜像
- `docker images -aq`
- 查询命令帮助文件
- `docker xx命令 --help`
- 搜索指定信息镜像
- `sudo docker search mysql --filter=STARS=6000`
- 拉取指定版本镜像
- `sudo docker pull mysql:8.0`
- 删除指定id镜像
- `docker rmi -f 镜像id`
- 交互式运行某镜像
- `docker run -it xxx /bin/bash`
- 容器不停止退出
- ctrl+P+Q
- 列出当前运行中镜像
- `docker ps -aq`
- 删除容器
- ·docker rm -f 容器id·
- 启动容器
- `docker start|stop|kill 容器id`
- 进入容器
- `sudo docker exec -it 3829e4dc7190 /bin/bash`
- 查看日志
- ·docker logs -f -t --tail 10 5578e2c7435c·
- 查看容器进程
- ·sudo docker top 5578e2c7435c·
- 查看容器信息
- ·docker inspect 5578e2c7435c·
- 从容器内拷贝文件到宿主机(在容器中执行)
- `docker cp 容器id:/home/test.java /home`
## docker mysql images
- 启动mysql镜像:-v /root/data:/var/lib/mysql /root/data/:这是宿主机的数据存放路径（你也可以自定义）
- `docker run -it --name some-mysql -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v /root/data:/var/lib/mysql mysql:5.6`
- 进入mysql容器
- `sudo docker exec -it 3829e4dc7190 /bin/bash`
- 登录数据库
- `mysql -h localhost -P 3306 -u root -p`
数据库相关操作

`select database();`
`create database test;`
`use test`
## docker redis images
- `sudo docker run --name redisNeedPw -p 6379:6379 -d --restart=always  redis:latest redis-server --appendonly yes --requirepass "123456"`

