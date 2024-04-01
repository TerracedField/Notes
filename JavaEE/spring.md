# Spring

[toc]

## 注解

不用new，spring可以自动装配，如下：

![P1](pics\P3.png) 

但是需要在自动装配的类上面加上注解，不然扫描不到，mapper也要@Mapper，诸如此类。

![P1](pics\P4.png) 

## maven

### 仓库路径设置

可以直接IDEA设置里面搜maven，然后改路径，不用放在C盘。

### 依赖

依赖可以在配置完之后用这样的方式同一控制版本

![P1](pics\P1.png)  

或者直接在每个依赖下面加版本

![P1](pics\P2.png) 

但是你tm不能不加版本，12月20日，被折磨两个小时，你终于学会了加版本

## mybatis

[Spring整合MyBatis——超详细_mybatis-spring文件-CSDN博客](https://blog.csdn.net/qq_42662759/article/details/116757078?spm=1001.2014.3001.5501)

这篇基本就没问题，注意resource下面建目录的时候注意下，不要打点，打/

[[MyBatis\] Invalid bound statement (not found)解决方案 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/646132189)

### multipartfile存文件

[MyBatis + MySQL + MultipartFile保存文件二进制数据到表里面_mysql multipartfile-CSDN博客](https://blog.csdn.net/hp961218/article/details/95505159)

然后取出来的时候存为byte[]，然后转file返回

[文件和byte数组之间相互转换_file转byte数组-CSDN博客](https://blog.csdn.net/zongzhankui/article/details/78300745)

## controller

controller接受APIfox传来的参数时可以自动装箱为一个参数与APIfox参数相同的对象（前提是有这个类，并且接受类型设置为这个类）

也可以这样接受：

```java
public String uploadResource(@RequestParam("resource_id")int resource_id,@RequestParam("donor_user_id")int donor_user_id,@RequestParam("text_file")MultipartFile text_file) throws JsonProcessingException {
		
    return "jjj";
}
```

