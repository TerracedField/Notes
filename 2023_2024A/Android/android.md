# Android studio

[toc]

环境：

1. 虚拟机PIXEL 4
2. API 29(ANDROID 10.0)

## 项目目录分析

### 切换目录到project

![P1](D:\Typora\notes\2023_2024A\Android\pics\P1.png) 

左上角点project切换

![P2](D:\Typora\notes\2023_2024A\Android\pics\P2.png) 

### .gradle

Android studio自动生成

### .idea

Android studio自动生成

### app

存放项目主要文件

![P3](D:\Typora\notes\2023_2024A\Android\pics\P3.png) 

#### build

自动生成

#### libs

放jar包

#### src-androidTest

放测试用例的

#### src-main

##### java

主要的程序

##### res

资源目录，放图片布局、字符串

![P6](D:\Typora\notes\2023_2024A\Android\pics\P6.png) 

drawable：图片

layout：布局

values：字符串、颜色

xhdpi：分辨率

##### androidManifest.xml

注册活动的，所有活动都要注册

![P4](D:\Typora\notes\2023_2024A\Android\pics\P4.png) 

这里就是注册了mainActivity

其中<intent-filter>指定为其为项目主活动，已启动项目第一个活动就是main-activity



#### src-test

也是测试

### gradle

存放配置文件

### .gitignore

版本控制

### build gradle

全局gradle脚本

### gradle properties

全局配置文件

### gradlew

linux/mac用的

### gradlew.bat

win用的

### local.properties

指定sdk位置的

### settings.gradle

项目中引用模块的组合

## Activity

Activity是安卓一个基类

```java
setContentView(R.layout.activity_main);
```

重写父类方法，用这个函数调用了layout里面的样式

![P5](D:\Typora\notes\2023_2024A\Android\pics\P5.png) 

### 创建活动

![P5](D:\Typora\notes\2023_2024A\Android\pics\P9.png)

### 注册活动和注册为主活动

![P5](D:\Typora\notes\2023_2024A\Android\pics\P10.png) 

如图，两个活动ActivityLifeCycle和MainActivity被注册到了AndroidManifest.xml里面，添加了<intent-filter>的为主活动

### 调用layout布局文件

```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_life_cycle);
}
```

调用res下的layout下的activity_life_cycle，

注意super.onCreate(savedInstanceState);为创建活动自动生成不用管

一个activity就是通过setContentView(R.layout.activity_life_cycle);来与layout下的样式文件进行绑定的

## layout

在layout布局文件中写如下

```xml
<Button
    android:id="@+id/Button01"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="取消"></Button>
```

指定button的id为Button01

宽和parent一样，高度刚好够包住文字，文本为取消

## AndroidManifest.xml

引用XXX就是@XXX/XXX_ID

如这里的APPname

![P7](D:\Typora\notes\2023_2024A\Android\pics\P7.png) 





![P8](D:\Typora\notes\2023_2024A\Android\pics\P8.png) 