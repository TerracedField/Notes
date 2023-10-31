# Nginx作业2

姓名：杨锦烨

学号：222021321262043

班级：2021级 软件工程（普通）2班

[toc]

## 部署Portainer服务

首先，从docker hub上拉一个Portainer镜像

```bash
docker pull portainer/portainer:latest
```

使用Docker部署Portainer服务。可以通过以下命令来运行Portainer容器：

```bash
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```

![pic1](Pics\pic1.png) 

## 文件服务

文件服务选alist：

```bash
docker run -d --restart=always -v /home/lers/alist:/opt/alist/data -p 5244:5244 -e PUID=0 -e PGID=0 -e UMASK=022 --name="alist" xhofe/alist:latest
```

## 打包若依项目前后端

### 数据库准备

#### MySQL

拉取一个MySQL镜像

```bash
docker pull mysql
```

运行MySQL容器

```bash
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=yjy123456 mysql
```

#### redis

拉取一个redis镜像

```bash
docker pull redis
```

运行redis容器

```bash
docker run -itd --name redis -p 6379:6379 redis
```

### 打包前后端项目

在IDEA中用maven-package打包，在前端目录路径下用npm run build命令打包。

![pic5](Pics\pic5.png) 

## 配置Nginx代理

### Dockerfile

```dockerfile
From nginx:latest
WORKDIR /home/nginx/html/
ADD ./dist.tar ./
COPY nginx.conf /etc/nginx/
```

### 配置文件

创建一个名为`nginx.conf`的Nginx配置文件：

 ```nginx
   http {
       upstream myruoyi {
             server 192.168.177.1:8080;
             server 192.168.177.1:8081;
       }
   
       server {
             listen 80;
             server_name  localhost;
         
           location / {
             root   /home/nginx/html/;#跟dockerfile一致
 			try_files $uri $uri/ /index.html;
             index  index.html index.htm;
           }
   
           location / {
               proxy_pass http://myruoyi; # 跟上面upstream一致
               proxy_http_version 1.1;
               proxy_set_header Upgrade $http_upgrade;
               proxy_set_header Connection "upgrade";
               proxy_set_header Host $host;
               proxy_set_header X-Real-IP $remote_addr;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
               proxy_set_header X-Forwarded-Proto $scheme;
               proxy_set_header X-Forwarded-Host $host;
               proxy_set_header X-Forwarded-Port $server_port;
               proxy_set_header X-Forwarded-Server $host;
               proxy_set_header X-Forwarded-Ssl on;
               proxy_set_header X-Forwarded-Proto $scheme;
               proxy_cookie_path / "/; HTTPOnly; Secure";
               proxy_cookie_domain $host;
               proxy_redirect off;
               proxy_read_timeout 300;
               proxy_send_timeout 300;
               proxy_set_header X-Real-IP $remote_addr;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
               proxy_set_header X-Forwarded-Proto $scheme;
               proxy_set_header Upgrade $http_upgrade;
               proxy_set_header Connection "upgrade";
               proxy_set_header Host $http_host;
           }
       }
   }
 ```

![pic2](Pics\pic2.png) 

## 创建Nginx容器并挂载配置文件

创建一个名为`nginx`的Nginx容器，并将配置文件挂载到容器内部的`/etc/nginx/nginx.conf`路径。可以使用以下命令来运行Nginx容器：

```bash
docker run -d -p 80:80 --name nginx -v /path/to/nginx.conf:/etc/nginx/nginx.conf nginx
```

![pic3](Pics\pic3.png) 

完成以上步骤后，Nginx将根据配置文件进行代理和负载均衡。

当访问`http://<ip_address>/docker/`时，将被代理到Portainer服务；

当访问`http://<ip_address>/file/`时，将指向文件存放目录。

当访问`http://<ip_address>/`时，将根据负载均衡配置转发到指定的若依项目，并实现会话保持功能。

## 尝试访问

![pic4](Pics\pic4.png) 