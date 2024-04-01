[toc]

## 实验1

P100

### 要求

1. 为 Linux 添加一块 SCSI 磁盘/dev/sdb，容量为 250MB。
2. 并在该磁盘上创建一个分区 sdb1，大小为 150MB，标识为 Linux native 分区。 
3. 在分区 sdb1 上创建 ext4 文件系统，然后挂载到 Linux 的挂载点/mnt/usr。

### 命令

查看磁盘：

 ```bash
 fdisk -l
 ```

创建分区，假设新磁盘为 `/dev/sdb`：

```bash
fdisk /dev/sdb
```

sda1:系统第1块 SATA 接口的硬盘第 1个分区sdb5:系统第2块 SATA 接口的硬盘第 5 个分区

格式化 sdb1 分区为 ext4 文件系统:

```bash
mkfs.ext4 /dev/sdb1
```

挂载到 Linux 的挂载点/mnt/usr

首先创建usr文件夹，然后挂载

```bash
mkdir /mnt/usr
mount /dev/sdb1 /mnt/usr
```

永久挂载分区：

在`/etc/fstab`中加入

```
/dev/sdb1   /mnt/usr   ext4   defaults   0 0
```

## 实验2

P68

P77实战

### 要求

1． 新增加一个组名为 student，密码为 123。 

2． 新增加一个用户名为 zhouxingchi，其附属组为 student，用户密码为 123456，家目录为 /mnt/usr/zxc。 

3． 新建一个目录/usgroup a) 该目录的拥有者为 zhouxingchi，拥有组为 student； b) 该目录下所有文件只有属主可以删除，其他用户即使有修改权限也无法删除。

### 命令

新增student组 

```bash
groupadd student
```

设置组密码

```
gpasswd student
```

新增zhouxingchi用户

```
 useradd zhouxingchi
```

设置用户密码

```
passwd zhouxingchi
```

设置附属组为 student

```
usermod -G student zhouxingchi
```

设置家目录

```
usermod -d /mnt/usr/zxc zhouxingchi
```

设置usgroup目录的拥有者为 zhouxingchi

```
chown zhouxingchi usgroup
```

设置usgroup目录的拥有组为 student

```
chgrp student usgroup
```

先给其它用户加上 rwx 权限 

```
chomod o+r+w+x usgroup
```

设置粘滞位权限

```
chmod o+t usgroup
```

### 其它

rwx权限：可读可写可执行

```
u:user 文件属主
g:group 文件组
o:other 其它用户

rwx:读 写 执行
chmod o+x filename
file的其它用户加上可执行权限
```



[linux中SET位权限和粘滞位权限详解 - 夜半歌声断 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yellowzunzhi/p/12649823.html)

假如本来在该位上有x, 则这些特别标志 (suid, sgid, sticky) 显示为小写字母 (s, s, t).否则, 显示为大写字母 (S, S, T) 。 

粘滞位权限是针对目录的，对文件无效



umask

比如该用户 touch 或 gedit 创建一个文件，则**其默认权限为 -rw-r-r–**，如果该用户创建一个**可执行文件**（比如编译生成的程序），则其**默认权限为      	-rwx r-x r-x**。也就是说，由于 umask 的设定，创建的文件默认是不具有 g 的 w 权限和 o 的 w 权限的，除非用 chmod 更改权限。

对于创建文件file默认权限为 **rw-rw-rw-**（666）umask 002，则实际file权限为664：

默认的666为 110	110	110，与umask取反111	111	101位与，得110	110	100，即664，即rw rw r

## 实验3

### 要求

在前面实验的基础上，对用户 zhouxingchi 实行磁盘限额， 要求其在家目录下使用容量 不超过 100K，新建文件的个数不超过 10 个。

### 命令

因为 zhouxingchi 的家目录挂载在了根目录下，所以这里重新挂载 /，让文件系统支持 磁盘配额。用法：mount -o usrquota，grpquota 磁盘名

重新挂载根文件系统，并启用用户配额（usrquota）和组配额（grpquota）功能。

```
mount -o remount,usrquota,grpquota /
```

建立磁盘配额的配置文件

```
quotacheck -avugm
```

设置 zhouxingchi 的磁盘限额

```
edquota -u zhouxingchi
```

然后进入vim编写相应的软硬限制

启动配额

```
quotaon -ugv /
```





## 实验4

P94

### 要求

1. 为Linux新添加一块SCSI磁盘/dev/sdc，容量为1000MB。在该磁盘上创建三个分区sdc1、sdc2、sdc3，大小为128MB，标识为Linux native分区。
2. 在三个分区上创建物理卷；将三个物理卷加入VolGroup00卷组；在VolGroup00卷组上创建LogVolHome逻辑卷，大小为300M。
3. 在LogVolHome逻辑卷上创建ext4文件系统，并挂载到/mnt/mntpoint下。
4. 对逻辑卷LogVolHome上的文件系统扩容

a)    为Linux新添加一块SCSI磁盘/dev/sdd，容量为256MB。在该磁盘上创建一个分区sdd1，大小为256MB，分区类型为8e。

b)    在分区sdd1上创建物理卷。

c)    将新创建的物理卷扩展到VolGroup00卷组。

d)    对VolGroup00卷组中的LogVolHome逻辑卷进行扩容。

e)    对LogVolHome逻辑卷上的文件系统进行扩容。

### 命令

这是一个相当复杂的操作，涉及到多个步骤和命令。以下是完成这些任务的步骤：

第一部分 - 关于磁盘sdc的操作：

1. **创建分区：**
   使用 `fdisk` 命令对新磁盘sdc进行分区：
   ```bash
   fdisk /dev/sdc
   ```
   在fdisk中进行以下操作：
   - 创建三个分区sdc1、sdc2、sdc3，每个大小为128MB，类型为Linux native分区类型。

2. **创建物理卷：**
   使用`pvcreate`命令为每个分区创建物理卷：
   ```bash
   pvcreate /dev/sdc1
   pvcreate /dev/sdc2
   pvcreate /dev/sdc3
   ```

3. **加入VolGroup00卷组：**
   使用`vgextend`命令将三个物理卷加入VolGroup00卷组：
   ```bash
   vgextend VolGroup00 /dev/sdc1 /dev/sdc2 /dev/sdc3
   ```

4. **创建逻辑卷：**
   使用`lvcreate`命令在VolGroup00卷组上创建LogVolHome逻辑卷，大小为300M：
   ```bash
   lvcreate -n LogVolHome -L 300M VolGroup00
   ```

5. **创建并挂载文件系统：**
   使用`mkfs.ext4`命令对LogVolHome逻辑卷上创建ext4文件系统：
   ```bash
   mkfs.ext4 /dev/VolGroup00/LogVolHome
   ```
   然后挂载到/mnt/mntpoint下：
   ```bash
   mkdir /mnt/mntpoint
   mount /dev/VolGroup00/LogVolHome /mnt/mntpoint
   ```

6. **扩展逻辑卷：**
   使用`lvextend`命令将LogVolHome逻辑卷扩展到新添加的磁盘sdd上。

关于磁盘sdd的操作：

a) **创建分区：**
   使用 `fdisk` 命令对新磁盘sdd进行分区：
   ```bash
   fdisk /dev/sdd
   ```
   在fdisk中创建一个分区sdd1，大小为256MB，分区类型为8e。

b) **创建物理卷：**
   使用`pvcreate`命令为分区sdd1创建物理卷：
   ```bash
   pvcreate /dev/sdd1
   ```

c) **将物理卷扩展到VolGroup00卷组：**
   使用`vgextend`命令将新创建的物理卷扩展到VolGroup00卷组：
   ```bash
   vgextend VolGroup00 /dev/sdd1
   ```

d) **扩展逻辑卷：**
   使用`lvextend`命令对VolGroup00卷组中的LogVolHome逻辑卷进行扩容。

e) **扩展文件系统：**
   使用`resize2fs`命令对LogVolHome逻辑卷上的ext4文件系统进行扩容。







使用`lvextend`命令可以扩展逻辑卷的大小。以下是`lvextend`命令的基本用法：

```bash
lvextend -L +<增加的大小> <逻辑卷路径>
```

例如，如果您想要将逻辑卷`/dev/VolGroup00/LogVolHome`的大小增加100MB，您可以运行以下命令：

```bash
lvextend -L +100M /dev/VolGroup00/LogVolHome
```

如果您想将逻辑卷扩展到特定的绝对大小，而不是增加一个特定的量，您可以使用以下格式：

```bash
lvextend -L <新的大小> <逻辑卷路径>
```

例如，如果您想将逻辑卷`/dev/VolGroup00/LogVolHome`扩展到500MB，您可以运行以下命令：

```bash
lvextend -L 500M /dev/VolGroup00/LogVolHome
```

完成扩展后，您可能需要使用`resize2fs`命令来扩展逻辑卷上的文件系统，使其能够利用新的空间。

```
resize2fs /dev/VolGroup00/LogVolHome
```



## 实验5

P148写了crontab -e后编辑什么

### 要求

1. 查看定时任务进程，确保重启后能自动运行；
2. 对 root 用户，设置如下的定时任务：
    a) 每天下午 4：00 定时删除/tmp 目录下所有不属于 root 的文件
    b) 每月 15 日凌晨 12：00 重启系统；
    c) 每天早上 8：00 将/var/log/secure 文件内容发送给 admin@swu.edu.cn；
    d) 每隔 2 小时将命令 netstat –a 的输出发送给 admin@swu.edu.cn；
    e) 每天 7-17 点开放 sshd 服务
3. 查看 root 用户已经配置好的定时任务；
4. 不允许普通用户 zhouxingchi 设置定时任务。

### 命令

查看当前定时任务：

```
 crontab -l
```

编辑定时任务：

```
crontab -e
```



重启后自动运行cron

```bash
systemctl enable cron
```





关闭启动服务

systemctl start ＜ServiceName＞

systemctl stop ＜ServiceName＞



系统启动时就启动XX服务

systemctl enable ＜ServiceName＞  	在启动系统时启用名为 ServiceName 的服务

systemctl disable ＜ServiceName＞	 在启动系统时停用名为 ServiceName 的服务



每个任务描述行的格式如下： 

minute	hour	day-of-month	month-of-year	day-of-week	[username]	commands 

普通用户不能设置username

5 1 * * 7 			每周日凌晨 1:05

0 	4,8-18,22	 * * * 			每天 4:00，22:00 以及 8～18 的每个整点

## 实验6

P131 RPM使用 **看包名！！！！！！！！**

P135 YUM

P137 YUM配置

P138 仓库配置

### rpm

rpm -i ＜.rpm file name＞ 安装指定的.rpm 文件 

rpm -U ＜.rpm file name＞ 用指定的.rpm 文件升级同名包 

rpm -e ＜package-name＞ 删除指定的软件包 

rpm -q ＜package-name＞ 查询指定的软件包在系统中是否安装

### yum

yum check-update  检查可更新的所有软件包 

yum update  下载更新系统已安装的所有软件包 

yum upgrade  大规模的版本升级，与 yum update 不同的是，连旧的被淘汰的包也升级

yum install ＜packages＞  安装新软件包 

yum update ＜packages＞  更新指定的软件包

yum remove ＜packages＞  移除指定的软件包 

yum localinstall ＜rpmfile＞  安装本地的 RPM 包（与 rpm -i 命令的不同在于同时安装依赖的包）

yum localupdate ＜rpmfile＞  更新本地的 RPM 包（与 rpm -U 命令的不同在于同时安装依赖的包）

yum list 列出资源库中所有可以安装或更新的 rpm 包，以及已经安装的 rpm 包  

### yum源配置

先挂载装了rpm包的光盘到/mnt/cdrom下

```
name=043yjy
baseurl=file:///mnt/cdrom
enabled=1 # 用于指定是否使用本仓库
gpgcheck=0 #  校验软件包的 GPG 签名
```





## 实验8

P321 vsftpd配置文件

### 其它



**每次配置完了要systemctl restart vsftpd**



/etc/vsftpd/vsftpd.conf

```
//允许匿名登录
anonymous_enable=YES 
//允许本地用户登录
local_enable=YES 
//开放本地用户的写权限
write_enable=YES 
#允许匿名用户上传
anon_upload_enable=YES    
#允许匿名用户建目录
anon_mkdir_write_enable=YES
#允许匿名用户其他操作
anon_other_write_enable=YES
#tcp_wrapper
tcp_wrappers=YES
```

启动 vsftpd 并确保其开机启动 

```
systemctl start vsftpd 
```

```
systemctl enable vsftpd 
```



TCP Wrapper

只允许 IP 地址为 192.168.98.1（Windows 主机）和 192.168.98.33（Linux 主机）的 ftp 客 户端访问 vsfptd 服务器

编辑vsftpd的配置文件`/etc/vsftpd/vsftpd.conf`：

```
tcp_wrappers=YES
```

编辑TCP Wrapper的配置文件 `/etc/hosts.allow`：

```
vsftpd: 192.168.98.1, 192.168.98.33
```



拒绝 192.168.2.0/24 访问

编辑vsftpd的配置文件`/etc/vsftpd/vsftpd.conf`：

```
tcp_wrappers=YES
```
编辑TCP Wrapper的配置文件 `/etc/hosts.allow`：
```
vsftpd: 192.168.2.0/24: DENY
```



配置ftp的yum

```
[ftp]
name=FTP YUM Repository
baseurl=ftp://your_server_ip/yum
enabled=1
gpgcheck=0
```



## 实验10

看实验报告



GRUB方式：

1. 在 GRUB 启动菜单界面中按〈E〉键可以进入菜单项编辑界面
2. ro 改成 rw，使得可以读写
3. 加上init=/bin/sh，进入故障排除（告诉Linux内核在启动时使用`/bin/sh`作为系统的初始化进程）
4. mount -o remount,rw/（重新挂载根分区）
5. 用passwd命令改密码



用Linux安装光盘进入急救模式

1. BIOS中光盘启动调到第一位
2. 进入急救模式
3. 发现系统挂载到了/mnt/sysimage
4. 用 chroot 来将指定/mnt/sysimage换为根目录
5. 用passwd命令改密码

## 防火墙

开防火墙

```
systemctl start firewalld
```

关防火墙

```
systemctl stop firewalld
```

开机自启动

```bash
systemctl start firewalld
systemctl enable firewalld
```

开机不自启动

```bash
systemctl stop firewalld
systemctl disable firewalld
```



## 读shell

- `command > file.txt`：将命令的输出重定向到`file.txt`文件中，覆盖文件中的内容。
- `command >> file.txt`：将命令的输出附加到`file.txt`文件的末尾，不覆盖文件中的内容。
- `command < file.txt`：将`file.txt`文件的内容作为命令的输入。
- `command1 | command2`：将`command1`的输出作为`command2`的输入。





`$#`表示传入脚本的参数数量。



`*0)`：这是case语句的一个分支，表示当变量 `i` 的值以0结尾时执行以下操作。



cat >> $1

使用`cat`命令将标准输入追加到文件`$1`中。也就是将用户输入的内容追加到文件中。用户需要手动输入一段文字。因为脚本中的 `cat >> $1`命令会将用户输入的内容追加到文件 `$1` 中。所以在这种情况下，用户需要手动输入要追加的内容，然后按下Ctrl+D来结束输入，这样输入的内容就会被追加到文件中



在Unix和Linux系统中，按下Ctrl+D键组合通常用于表示输入的结束



for i in \* 的`*`代表通配符，匹配当前目录下的所有文件和目录名。因此，`for i in *` 表示对当前目录下的所有文件和目录进行迭代。

```
d=`pwd`
for i in *
do
if test –d $d/$i 
then
cd $d/$i 
while echo “$i:”
trap “exit” 2
read x
do eval $x; done
fi
done
```



1. `d=`pwd``：这一行将当前目录的路径保存在变量 `d` 中。
2. `for i in *`：这是一个for循环，它会遍历当前目录下的所有项目，并将每个项目的名称赋值给变量 `i`。
3. `if test –d $d/$i`：这行代码检查当前项目是否是一个目录。
4. `then`：如果是目录，则执行以下操作。
5. `cd $d/$i`：进入该目录。
6. `while echo “$i:”`：这一行开始一个while循环，并输出当前目录的名称。
7. `trap “exit” 2`：设置一个信号处理器，当接收到信号2（通常是Ctrl+C）时，执行 `exit` 命令退出循环。
8. `read x`：从用户输入读取一行命令，并将其保存在变量 `x` 中。
9. `do eval $x; done`：执行用户输入的命令，然后继续等待用户输入下一条命令。这个过程会一直循环，直到接收到信号2（通常是Ctrl+C）时退出循环。

因此，这段脚本的作用是进入当前目录下的每个子目录，然后允许用户在每个子目录中输入命令来对该目录进行操作，直到用户手动中断或退出。











