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
