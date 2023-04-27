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
列出当前已有镜像
- `docker images -aq`
查询命令帮助文件
- `docker xx命令 --help`
搜索指定信息镜像
- `sudo docker search mysql --filter=STARS=6000`
拉取指定版本镜像
- `sudo docker pull mysql:8.0`
删除指定id镜像
- `docker rmi -f 镜像id`
交互式运行某镜像
- `docker run -it xxx /bin/bash`
列出当前运行中镜像
- `docker ps -aq`
-  
