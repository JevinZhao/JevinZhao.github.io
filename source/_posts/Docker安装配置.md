---
title: Docker安装配置
date: 2021-06-10 22:19:07
tags: 项目
categories: Docker
---
## Docker安装配置

### 安装Docker

> 参考：[Docker 安装文档](https://docs.docker.com/engine/install/centos/)

#### 1. 删除老版本

```shell
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 2. 安装工具包并设置存储库

```shell
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

#### 3. 安装docker引擎

```shell
sudo yum install docker-ce docker-ce-cli containerd.io
```

### Docker使用

#### 1. 启动docker

```shell
sudo systemctl start docker
```

#### 2. 设置开机启动docker

1. 检查docker版本

```shell
docker -v
```

1. 查看docker已有镜像

```shell
sudo docker images
```

1. 设置docker开机启动

```shell
sudo systemctl enable docker
```

#### 3. 设置国内镜像仓库

> 参考：[阿里云镜像加速服务](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

```shell
# 创建文件
sudo mkdir -p /etc/docker
# 修改配置, 设置镜像
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://vw9qapdy.mirror.aliyuncs.com"]
}
EOF
# 重启后台线程
sudo systemctl daemon-reload
# 重启docker
sudo systemctl restart docker
```

### MySQL of Docker

#### 1. docker安装mysql

```shell
sudo docker pull mysql:5.7
```

#### 2. docker启动mysql

```shell
sudo docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```

参数：

- -p 3306:3306：将容器的3306端口映射到主机的3306端口
- --name：给容器命名
- -v /mydata/mysql/log:/var/log/mysql：将配置文件挂载到主机/mydata/..
- -e MYSQL_ROOT_PASSWORD=root：初始化root用户的密码为root

查看docker启动的容器:

```shell
docker ps
```

#### 3. 配置mysql

1. 进入挂载的mysql配置目录

```shell
cd /mydata/mysql/conf
```

​    2.修改配置文件 `my.cnf`

```shell
vi my.cnf
```

拷贝以下内容：

```shell
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve

# Esc
# :wq
```

1. docker重启mysql使配置生效

```shell
docker restart mysql
```

### Redis of Docker

#### 1. docker拉取redis镜像

```shell
docker pull redis
```

#### 2. docker启动redis

1. 创建redis配置文件目录

```shell
mkdir -p /mydata/redis/conf

touch /mydata/redis/conf/redis.conf
```

1. 启动redis容器

```shell
docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

#### 3..测试redis

```shell
# 进入redis客户端
docker exec -it redis redis-cli
# 设置值
set a b
# 取值
get a
# 退出
exit
# 重启 
docker restart redis
# 进入客户端，查看设置的值，发现没有
docker exec -it redis redis-cli
get a
```

#### 4. 配置redis持久化

> 更多redis配置参考：[redis配置](https://raw.githubusercontent.com/redis/redis/6.0/redis.conf)

```shell
# 采用aof的持久化方式
echo "appendonly yes"  >> /mydata/redis/conf/redis.conf
# 或者采用
vi /mydata/redis/conf/redis.conf
# i
复制 appendonly yes
# Esc
# :wq 
# 重启生效
docker restart redis
```

### 容器随docker启动自动运行

```shell
# mysql
docker update mysql --restart=always

# redis
docker update redis --restart=always
```

