# Nginx作业1

姓名：杨锦烨

学号：222021321262043

班级：2021级 软件工程（普通）2班

[toc]

## 创建网络

在CentOS控制台输入如下命令：

```bash
docker network create --subnet=172.18.0.0/16 --gateway=172.18.0.1 mynet
```

创建过程中可能会报错：

```
Error response from daemon: Pool overlaps with other one on this address space
```

说明指定的子网前缀已被占用，在CentOS控制台输入如下命令：

```bash
docker network ls
```

查看当前网卡ID，逐个使用如下命令：

```bash
docker network inspect 网卡ID
```

删除与指定子网前缀重复的网络即可：

```bash
docker network rm 网卡ID
```



![pic1](Pics\pic1.png) 

## 启动MySQL容器并连接到mynet网络

首先拉取一个最新的MySQL镜像：

```bash
docker pull mysql:latest
```

然后通过如下命令来启动MySQL容器并连接到mynet网络：

```bash
docker run -d --name mysql-container --network=mynet --ip=172.18.0.10 -e MYSQL_ROOT_PASSWORD=123456 mysql:latest
```

## 制作镜像

```bash
docker commit mysql-container ry:1.0
```

制作的镜像如图：

![pic3](Pics\pic3.png) 

## 启动容器并设置宿主机端口和内网IP

```bash
docker run -d -p 8080:3306 --name ry-container --network=mynet --ip=172.18.0.10 ry:1.0
```

以上所有命令流程如图：

![pic2](Pics\pic2.png) 

