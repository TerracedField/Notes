# Linux

## 添加组 用户

[Linux - 增加用户、添加用户组_linux添加用户组命令-CSDN博客](https://blog.csdn.net/zhichaosong/article/details/82622558)

## 权限

[Linux权限详解（chmod、600、644、700、711、755、777、4755、6755、7755）_chmod700什么意思-CSDN博客](https://blog.csdn.net/u013197629/article/details/73608613)

```bash
u:user 文件属主
g:group 文件组
o:other 其它用户

rwx:读 写 执行
chmod o+x filename
file的其它用户加上可执行权限
```



### set位权限和粘滞位权限(rwx中的s/S/t/T)

[linux中SET位权限和粘滞位权限详解 - 夜半歌声断 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yellowzunzhi/p/12649823.html)

假如本来在该位上有x, 则这些特别标志 (suid, sgid, sticky) 显示为小写字母 (s, s, t).否则, 显示为大写字母 (S, S, T) 。 

粘滞位权限是针对目录的，对文件无效

## vim

按i切换成正常的输入模式

按esc之后按冒号输入wq保存退出

## 磁盘配额

### 主要参考

[Linux 磁盘管理-配额管理-配置用户对磁盘进行指定大小或者文件数量的使用权限_指定磁盘容量_xyz的博客-CSDN博客](https://blog.csdn.net/xyz/article/details/117350197)

### quota扫描

[11.3 Linux扫描文件系统并建立磁盘配额记录文件（quotacheck命令）-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/910555)

### quota查询磁盘配额

[Linux quota和repquota命令查询磁盘配额方法详解 (biancheng.net)](https://c.biancheng.net/view/909.html#:~:text=查询磁盘配额有两种方法： 使用 quota 命令,查询用户或用户组的配额； 使用 repquota 命令 查询整个分区的配额情况。)

## 挂载

[(65 封私信 / 80 条消息) 能否通俗易懂，深入浅出地解释一下linux中的挂载的概念？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/266907637)

[如何查看linux分区挂载在哪个目录？_linux查看目录挂载在哪个盘-CSDN博客](https://blog.csdn.net/xuxu_123_/article/details/130737385)

### 复杂的介绍

linux下面所有的文件、目录、设备都有一个路径，这个路径永远以/开头，用/分隔，如果一个路径是另一个路径的前缀，则这两个路径有逻辑上的父子关系。

但是并不是所有逻辑上的父子关系都必须要是同一个设备，决定不同路径对应到哪个设备的机制就叫做mount（挂载）。通过mount，可以设置当前的路径与设备的对应关系。

每个设备会设置一个挂载点，挂载点是一个空目录。一般来说必须有一个设备挂载在/这个根路径下面，叫做rootfs。其他挂载点可以是/tmp，/boot，/dev等等，通过在rootfs上面创建一个空目录然后用mount命令就可以将设备挂载到这个目录上。挂载之后，这个目录下的子路径，就会映射到被挂载的设备里面。

当访问一个路径时，会选择一个能最大匹配当前路径前缀的挂载点。比如说，有/var的挂载点，也有/var/run的挂载点的情况下，访问/var/run/test.pid，就会匹配到/var/run挂载点设备下面的/test.pid。

同一个设备可以有多个挂载点，同一个挂载点同时只能加载一个设备。访问非挂载点的路径的时候，按照前面所说，其实是访问最接近的一个挂载点，如果没有其他挂载点那么就是rootfs上的目录或者文件了。



实际上并不只有linux支持挂载点，Windows也是一样支持的。去控制面板/管理工具/计算机管理 里面，挑一个磁盘（比如D盘），然后给它分一个新的挂载点试试，比如C:\data

### 正常的介绍

[如何查看linux分区挂载在哪个目录？_linux查看目录挂载在哪个盘-CSDN博客](https://blog.csdn.net/xuxu_123_/article/details/130737385)

sda1挂载在“/boot/[efi](https://so.csdn.net/so/search?q=efi&spm=1001.2101.3001.7020)”目录下，后续在该目录（/boot/efi）下创建的文件，存储在sda1分区。

同一个设备可以有多个挂载点，同一个挂载点同时只能加载一个设备。



另外/dev (dev->device设备，一般是磁盘)


