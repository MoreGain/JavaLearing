# Docker 入门

### 一、概述

###### 虚拟化

资源管理技术，

###### Docker

容器技术

###### 容器与虚拟机的比较

### 二、Docker 组件

######  服务端与客户端

###### 镜像与容器

###### 注册中心

### 三、安装与启动

###### 安装

###### 国内镜像

###### 启动与停止

启动 docker

```bash
systemctl start docker
```

重启 docker

```bash
systemctl restart docker
```

停止 docker

```bash
systemctl stop docker
```

查看 docker 状态

```bash
systemctl status docker
```

设置 docker 开机启动

```bash
systemctl enable docker
```

查看 docker 概要信息

```bash
docker info
```

查看 docker 帮助文档

```bash
docker --help
```



### 四、常用命令

###### 镜像相关

查看镜像

```base
docker images
```

搜索镜像

```dockerfile
docker search <centos>
```

拉取镜像

```dockerfile
docker pull <centos:7>
```

删除镜像

```dockerfile
docker rmi <centos>
docker rmi `docker images -q`
```

###### 容器相关

查看运行中的容器

```dockerfile
docker ps
docker ps -a
```

创建与启动容器

```dockerfile
docker run [-i][-t][--name=name][]
# 交互式创建容器，通过exit方式退出容器会停止容器
docker run -it --name=mycentos centos:7 /bin/bash
# 守护式创建容器
docker run -di --name=mycentos2 centos:7
# 进入守护式容器，此时通过exit方式退出容器不会停止容器
docker exec -it mycentos2 /bin/bash
```

停止启动容器

```dockerfile
# 启动
docker start <ID/name>
# 停止
docker stop <ID/name>
```

文件拷贝

```dockerfile
docker cp mycentos2:/usr/local/a.txt 
docker cp 
```

目录挂载

```dockerfile
# 
docker run -di -v /usr/local/test:/usr/local/test --name=mycentos3 centos:7
```

查看容器IP

```dockerfile
docker inspect [--format='{{.NetworkSetting.IPAddress}}'] mycentos
```

删除容器

```dockerfile
# 无法删除运行中的容器，删除前须停止容器
docker rm mycentos3
```

### 五、应用部署

###### MySQL 部署

拉取镜像

创建容器

###### Tomcat 部署

拉取镜像

创建容器

```dockerfile
docker run -di --name=mytomcat -p 9000:8080 -v /usr/local/webapps:/usr/local/tomcat/webapps tomcat:7-jre7
```

###### Nginx 部署

拉取镜像

```dockerfile
docker pull nginx
```

创建容器

```dockerfile
docker run -di --name=mynginx -p 80:80 nginx
```

###### Redis 部署

拉取镜像

```dockerfile
docker pull redis
```

创建镜像

```dockerfile
docker run -di --name=myredis -p 6379:6379 redis
```

### 六、迁移与备份

###### 容器保存为镜像

将容器保存为镜像

```dockerfile
docker commit mynginx mynginx_i
```

###### 镜像备份

将镜像保存为文件

```dockerfile
docker save -o mynginx.tar mynginx
```

根据镜像恢复文件

```dockerfile
docker load -i mynginx.tar
```

###### 镜像恢复与迁移

### 七、Dockerfile

###### Dockerfile

由一系列命令和参数构成的脚本，这些命令应用于基础镜像并最终创建一个新的镜像

###### 常用命令

FROM image_name:tag

MAINTAINER user_name

ENV key value

RUN command

ADD source_dir/file dest_dir/file

###### 构建 Dockerfile 文件

### 八、Docker 私有仓库

###### 私有仓库搭建与配置

拉取私有仓库镜像

```dockerfile
docker pull registry
```

创建私有仓库容器

```dockerfile
docker run -di --name=myregistry -p 5000:5000 registry
```

修改配置，信任私有仓库

```dockerfile
# 修改daemon.json
vi /etc/docker/daemon.json
# 添加内容 {"insecure-registries":["ip:port"]}
# 重启docker服务时配置生效
systemctl restart docker
```

###### 镜像上传至私有仓库

打标签

```dockerfile
docker tag mycentos ip:port/mycentos_s
```

上传镜像

```dockerfile
docker push ip:port/mycnetos_s
```

