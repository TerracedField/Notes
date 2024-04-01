## dubbo的优势

1. 可以直接在页面对各种服务进行负载均衡的权重管理

![pic13](pics\pic13.png) 

2. 可以把同样的服务启动多个，如借书服务1，借书服务2，借书服务3，服务2挂了，还有服务1，3撑住

## dubbo操作

1. 先启动zookeeper（两个，zkServer.cmd和zkCli.cmd都要按）后启动dubbo（老师发的dubbo-admin文件里面，在idea直接启动）
2. dubbo可能对jdk版本有要求，要JAVA8.212
3. dubbo启动后用户名密码都是root

## 一些概念

提供者：提供某种服务，如图书馆管理系统，提供借书服务，主要是用mapper对数据库进行操作

消费者：差不多就是controller，读网页路径来的，调用提供者的服务

负载均衡：将流量导向压力不大的服务器

## 遇到的问题

### 网页传参

[url如何拼接参数格式& ？ 用&和? =拼接_url拼接参数-CSDN博客](https://blog.csdn.net/Mygongzi/article/details/123531547)，需要注意如这种情况：

![pic14](pics\pic14.png) 

可能需要改成：

```java
package borrowConsumer.controller;

import api.BorrowBookService;
import org.springframework.web.bind.annotation.*;


@RestController
public class BorrowServiceController {

    @com.alibaba.dubbo.config.annotation.Reference
    BorrowBookService borrowBookService;
    @RequestMapping("/borrow")
    public String borrow(@RequestParam("user_id") Integer user_id,@RequestParam("book_id") Integer book_id) {
        return borrowBookService.borrowBook(user_id,book_id);
    }
}
```

这种要用Integer接收参数

### dubbo配置

主要参照王伟杰网课，消费者和提供者配置不一样，提供者有两个端口，一个是注册在dubbo的，一个是启动端口

### 端口

要用APIfox或者直接网页访问，要看消费者启动端口

### session

mapper操作数据库一定要引入session对应的那个包

```java
//利用MyBatisUtil工具类获取数据库的连接
session = MyBatisUtil.openSession();
//获取到Mapper的接口
userMapper = session.getMapper(UserMapper.class);
bookMapper = session.getMapper(BookMapper.class);
recordMapper = session.getMapper(RecordMapper.class);
```

数据库操作完了要记得commit和close

```java
session.commit();
session.close();
```

不commit就没操作上，不close可能会有奇怪的问题，可能MySQL那边刷新不起

### 消费者传参给提供者

消费者传参给提供者用对象传可能会出现问题