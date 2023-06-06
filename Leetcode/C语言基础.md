# C语言基础

[toc]

## C语言中的bool值

从C99标准开始，C语言已经有了布尔型，类型名字为“**_Bool**”。

只要编译器支持C99，就可以直接使用布尔型。另外，C99为了让**C**和**C++**兼容，增加了一个头文件**stdbool.h**，里面定义了**bool**、**true**、**false**，可以像C++一样的定义布尔类型。

参考网址：[C语言的布尔类型](https://blog.csdn.net/daheiantian/article/details/6241893)

## 除法

```c
printf("%d\n",121/10);
```

输出“12”，是地板除，直接省去余数。

## 字符串比较

直接>,<,=是比较字符串首地址。也没有.equals()这种东西。

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char arr1[] = "abcdef";
	char arr2[] = "abc";
	int ret=strcmp(arr1, arr2);
	if (ret > 0)
	{
		printf("arr1>arr2");
	}
	else if (ret < 0)
	{
		printf("arr1<arr2");
	}
	else {
		printf("arr1=arr2");
	}
	return 0;
}
```

比较的是从第一位起的ASCII码值，没有比较长度的意思。某一位开始第一个参数的ASCII码大于第二个参数，那么返回1，小于返回-1，两个字符串相等则返回0。

所以正确的比较两个字符串是否相等应该是：

```c
strcmp(s, "IV")==0;
```

比较s和"IV"是否相等

[【C语言】正确比较两个字符串_c语言字符串比较](https://blog.csdn.net/weixin_53564801/article/details/123751749)

## 获取字符串长度

### sizeof()

基本用法网址：[C语言 --- sizeof() 使用详解](https://blog.csdn.net/zhouml_msn/article/details/103361539)

字符串方面的基本用法：

```c
int i=10;
sizeof(i);//表达式

char str[]="hello world";
sizeof(str);

sizeof(double);//数据类型
```

在使用sizeof()求字符串长度时，会将 ‘\0’ 也计算到字符串长度中。例如"abcd"用sizeof()求长度会计算得到5。
注意：char str[100]=""; sizeof(str)的值是100。



### strlen函数

在string.h中提供了计算字符串长度的函数。

语法：

```c
size_t strlen(const char *str);
```

在使用strlen函数时，需要添加string.h头文件，该函数会将字符串长度计算出，不包含 ‘\0’。



## 类型转换

例子：

```c
#include <stdio.h>
int main(){
    int sum = 103;  //总数
    int count = 7;  //数目
    double average;  //平均数
    average = (double) sum / count;
    printf("Average is %lf!\n", average);

    return 0;
}
```

sum 和 count 都是 int 类型，如果不进行干预，那么`sum / count`的运算结果也是 int 类型，小数部分将被丢弃；虽然是 average 是 double 类型，可以接收小数部分，但是心有余力不足，小数部分提前就被抛弃了，它只能接收到整数部分，这就导致除法运算的结果严重失真。

### 强制类型转换

```c
(float) a;  //将变量 a 转换为 float 类型
(int)(x+y);  //把表达式 x+y 的结果转换为 int 整型
(float) 100;  //将数值 100（默认为int类型）转换为 float 类型
```

### 自动类型转换

例子中这句如果不强制类型转换就会自动类型转换，将两个地板除得到的int给转换成double

```c
average = sum / count;
```

就是自动类型转换，将两个整型

## 格式化输入

### scanf正则用法

```c
scanf("%[^\n]",a);
```

`^`表示"非"，`[^\n]`表示读入换行字符就结束读入。这是scanf的正则用法，我们都知道scanf不能接收空格符，一接受到空格就结束读入，所以不能像gets()等函数一样接受一行字符串，但是使用`%[^\n]`就可以读取一行，直到碰到`’\n’`才结束读入。

### scanf吃回车的问题

scanf在一次输入结束后，不会舍弃最后的回车符（即回车符会残留在数据缓冲区中），两个连续的scanf只要scanf输入的是`%c %s %[^\n]`这类可以接受回车的都会出现这种吃回车的情况。

解决方案：

1. 输入时写成`scanf(" %[^\n]",letter);`  这里`%`前面有个空格吃掉回车这样就把缓冲区中的回车当成第一个字符，读取后丢掉;
2. scanf函数后面加个`fflush(stdin);` 会清除某些编译器中的输入缓冲区。

## 格式化输出

[格式化输出总结](https://blog.csdn.net/chenmozhe22/article/details/109738852)

## 二重指针和二维数组

### 字符型二维数组

二重字符指针通常用来表示一个字符串数组，也就是指向多个字符串的指针数组。

如果要获取二重字符指针指向了多少个字符串，可以通过循环遍历指针数组，并计算数组元素的个数来实现。代码示例如下：

```c
char *str_arr[] = {"hello", "world", "c", "programming", NULL}; // 字符串数组，最后一个元素为NULL

int count = 0; // 字符串个数

char **p = str_arr; // 二重字符指针

while (*p != NULL) {//二重指针遍历各个字符串的用法
    count++;
    p++;
}

printf("指向了%d个字符串\n", count);
```

**废案**，原来是想用这个解leetcode上面一道题（实际上题目给了字符串个数），但是这种方法求字符串数组包含的字符串数量的条件必须是最后为NULL。

但是可以看下二重指针遍历各个字符串的用法。

另外，似乎不能这样初始化二维数组

```c
char **str_arr = {"hello", "world", "c", "programming", NULL};
```

### 整型二维数组

在C语言中，如果已经定义了一个二维数组，可以通过以下方法求得第一维的大小：

```c
int arr[10][20]; // 定义一个二维数组

int size = sizeof(arr) / sizeof(arr[0]);
```

在上面的代码中，`sizeof(arr)`返回整个数组的大小，`sizeof(arr[0])`返回数组中第一个元素的大小，即第一维的大小。因此，通过将整个数组的大小除以第一个元素的大小，可以得到第一维的大小。

在上面的例子中，`arr`是一个10行20列的二维数组，因此`sizeof(arr)`返回的是2000（10 * 20 * 4），`sizeof(arr[0])`返回的是80（20 * 4），因此`size`的值为10。
