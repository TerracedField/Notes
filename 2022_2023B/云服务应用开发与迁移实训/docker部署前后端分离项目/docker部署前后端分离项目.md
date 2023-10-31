# 用docker部署前后端分离项目

姓名：杨锦烨

学号：222021321262043

班级：2021级 软件工程（普通）2班

[toc]

## 前后端项目的配置和打包

### 项目的配置

在CentOS终端输入ifconfig命令获取虚拟机IP，然后将制作完成的前后端项目中的相关IP配置信息设为虚拟机IP，将数据库配置信息设置为预设的用户名和密码。

如图：

![pic1](pics\pic1.png) 

（配置过多不逐一展示）

### 前端项目的打包

在IDE中打开前端项目，获取管理员身份后在控制台输入命令：

```bash
npm install
```

来初始化项目，加载相应的依赖，获得node_modules文件夹。

之后在控制台输入命令：

```bash
npm run build
```

将文件打包，获得一个dist文件夹里面存放了相关的前端文件。

![pic2](pics\pic2.png) 

### 后端项目的打包

后端项目在IDEA中使用maven打包，clean之后使用package即可打包：

![pic3](pics\pic3.png) 

打包结束后得到后端项目的jar包如下：

![pic4](pics\pic4.png) 

（如果本地没有相关，IDEA默认从apache官网拉取相关依赖，延迟太高等待时间会非常长，需要挂加速器）

## 前后端项目镜像的制作与容器的运行

### 准备工作

在CentOS中为前后端项目文件准备两个文件夹，将dist文件和jar包分别放入前后端文件中

![pic5](pics\pic5.png) 

### 前端镜像的制作

#### Nginx镜像的拉取

在CentOS控制台输入命令：

```bash
docker pull nginx
```

默认拉取最新的Nginx镜像到本地

#### Dockerfile文件

在前端文件夹中创建Dockerfile文件：

```bash
vim Dockerfile
```

之后进入编辑模式，编写内容为如图后保存并退出：

![pic7](pics\pic7.png) 

FROM nginx：因为项目是基于`Nginx`部署的，所以需要Nginx作为基础镜像

COPY ./dist /usr/share/nginx/html/：把dist文件上传到Nginx访问时默认启动的目录下

EXPOSE 80：暴露80端口（Nginx默认80）

#### 制作镜像

运行如下命令制作前端镜像：

```bash
docker build -t websocket-chat-ui .
```

如图命名为websocket-chat-ui的镜像制作完成：

![pic9](pics\pic9.png) 

####  容器的运行

输入如下命令运行容器：

```bash
docker run -d -p 8081:80 --name websocket-chat-ui websocket-chat-ui
```

暴露8081端口与容器内的80端口映射，并为容器取名为websocket-chat-ui

成功后如图：

![pic10](pics\pic10.png) 

### 后端镜像的制作

#### Java8镜像的拉取

在CentOS控制台输入命令：

```bash
docker pull Java:8
```

拉取Java8镜像到本地

#### Dockerfile文件

在后端文件夹中创建Dockerfile文件：

```bash
vim Dockerfile
```

之后进入编辑模式，编写内容为如图后保存并退出：

![pic11](pics\pic11.png) 

FROM java:8							：后端项目是基于Java8的
VOLUME /websocket-data	：数据卷
ADD *.jar app.jar					：把当前目录下的所有jar包上传到容器并重命名为app.jar
EXPOSE 8082						   ：暴露8082接口（后端项目在8082端口运行）
CMD ["java","-jar","/app.jar"]：运行jar包至Java框架

#### 制作镜像

运行如下命令制作后端镜像：

```bash
docker build -t websocket-chat .
```

制作好后如图：

![pic8](pics\pic8.png) 

#### 容器的运行

输入如下命令运行容器：

```bash
docker run -d -p 8082:8082--name websocket-chat websocket-chat
```

成功后如图：

![pic12](D:\Typora\notes\大二下学期\云服务应用开发与迁移实训\docker部署前后端分离项目\pics\pic12.png)

## MySQL部分

### MySQL镜像的拉取

在CentOS控制台中输入命令：

```bash
docker pull mysql:8.0.26
```

拉取成功后如图：

![pic13](pics\pic13.png) 

### 容器的运行

在CentOS控制台中输入命令：

```bash
docker run -d -p 3306:3306 --name websocket-mysql -v /mydata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=timeless123 mysql:8.0.26
```

初始化MySQL并设置用户名和密码

### 测试连接

启动MySQL容器后在IDEA中输入虚拟机IP和端口号，以及用户名和密码来测试连接：

![pic14](pics\pic14.png) 

### 数据库的构建

在主机与虚拟机中MySQL容器连接，并将提前编写好的SQL脚本运行，完成建库建表和基本用户信息的填充操作：

![pic15](pics\pic15.png)



![pic16](pics\pic16.png) 

## 项目演示

### 用户注册

在主机中输入虚拟机IP:8081进入主页面：

![pic17](D:\Typora\notes\大二下学期\云服务应用开发与迁移实训\docker部署前后端分离项目\pics\pic17.png)

点击用户注册：

![pic18](pics\pic18.png) 

注册提交后虚拟机内MySQL容器的数据也进行更新：

![pic19](pics\pic19.png) 

### 聊天功能

如杨锦烨向admin发送你好，admin：

![pic20](pics\pic20.png) 

双方都会收到消息：

![pic21](pics\pic21.png) 

消息也会同步进入数据库中：

![pic22](pics\pic22.png) 