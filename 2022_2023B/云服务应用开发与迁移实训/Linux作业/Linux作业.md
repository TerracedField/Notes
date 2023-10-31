# Linux作业

姓名：杨锦烨

学号：222021321262043

班级：2021级 软件工程（普通）2班

[toc]

## Q1

题目要求：通过yum安装mysql服务，要求服务开机自动启动

### 下载并安装mysql

下载安装用的Yum Repository：

```bash
[root@localhost ~]# wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

安装MySQL：

```bash
[root@localhost ~]# yum -y install mysql57-community-release-el7-10.noarch.rpm
```

```bash
[root@localhost ~]# yum -y install mysql-community-server --nogpgcheck
```

在CentOS中默认安装有MariaDB，安装完成后就会覆盖掉之前的mariadb。加上--nogpgcheck跳过公钥检查防止出错。

### MySQL数据库设置

启动MySQL：

```bash
[root@localhost ~]# systemctl start  mysqld.service
```

通过命令可以在日志文件中找出密码：

```bash
[root@localhost ~]# grep "password" /var/log/mysqld.log
```

进入数据库：

```bash
[root@localhost ~]# mysql -uroot -p
```

 输入初始密码，此时不能做任何事情，因为MySQL默认必须修改密码之后才能操作数据库：

```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'XXX';
```

注意XXX为新的密码

### 设置MySQL开机自启动

执行语句把MySQL服务设置成自启动：

```bash
[root@localhost ~]# systemctl enable mysqld
```

也可以在vi /etc/rc.local文件中添加 service mysqld start。

重启后查看MySQL状态：

![pic2](pics\pic2.png) 

设置成功。

## Q2

题目要求：利用mysql自带备份工具mysqldump 对mysql数据库进行定期备份和压缩操作，每天凌晨2:30开始自动备份和压缩数据，备份文件名为：完整时间_mysql.sql.gz（例如20230203122343_mysql.sql.gz），将备份文件保存到/opt/mysql.back目录下，每次执行备份操作后删除30天之前的所有备份文件

### 定期备份脚本

脚本1：定期备份和压缩MySQL数据库（默认备份所有数据库）

```shell
#!/bin/bash

# 获取当前日期和时间
current_date=$(date +%Y%m%d%H%M%S)

# 备份目录
backup_dir="/opt/mysql.back"

# 备份文件名
backup_file="${current_date}_mysql.sql.gz"

# 数据库用户名和密码
db_user="root"
db_password="TJyjy7221592_"

# 备份数据库
mysqldump -u ${db_user} -p ${db_password} --all-databases | gzip > "${backup_file}"

# 删除30天之前的备份文件
find  -type f -name '*_mysql.sql.gz' -mtime +30 -exec rm {} \; 
```

（设置路径到/opt/mysql.back文件下一直报错，就直接设定为备份到同级目录了）

### 设置可执行权限

将脚本1保存为`mysql_backup.sh`。然后，将这两个脚本设置为可执行文件：

```bash
chmod +x mysql_backup.sh
```

### 测试备份

执行脚本1后，发现同级目录下出现备份文件：

![pic3](pics\pic4.png)

### 设置定时任务

要实现每天凌晨2:30自动执行备份操作，可以使用`crontab`来设置定时任务：

```bash
crontab -e
```

![pic3](pics\pic3.png) 

输入以上内容后保存并退出，每天凌晨2:30脚本`mysql_backup.sh`将被自动执行。

## Q3

### 还原备份数据脚本

脚本2：还原备份数据

```shell
#!/bin/bash

# 从命令行读文件名
backup_file_name=$1

# 备份文件路径
backup_file="${backup_file_name}"

# 数据库用户名和密码
db_user="root"
db_password="TJyjy7221592_"

# 还原备份数据
gunzip < ${backup_file} | mysql -u ${db_user} -p${db_password}
```

###  设置可执行权限

将脚本2保存为`mysql_restore.sh`后输入如下命令：

```bash
chmod +x mysql_restore.sh
```

### 还原命令

在控制台输入如下命令来还原：

```bash
./mysql_source.sh 20230712115343_mysql.sql.gz
```
