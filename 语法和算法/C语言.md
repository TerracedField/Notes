# C语言

[toc]

## bool值

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

## malloc

在\#include <stdlib.h>库中

C语言中，利用函数malloc来申请内存，传入参数为申请内存的字节数，返回值为申请到的内存的首地址，是什么类型的地址，就要强转成什么类型，如下:

```c
int *p = (int*)malloc( sizeof( int)* n );
```

### malloc字符数组

```c
char* reverseLeftWords(char* s, int n){
	int Ssize = strlen(s);
	char* ans = (char*)malloc(Ssize*sizeof(char) + 1);
	for(int i = 0;i < Ssize-n;i++){
		ans[i] = s[i+n];
	}
	for(int i = Ssize - n;i < Ssize;i++){
		ans[i] = s[i - Ssize + n];
	}
	ans[Ssize] = '\0';//注意最后补'\0'
	return ans;
}
```

需要注意多申请一位，最后补'\0'，用来标记字符串结尾，不然有时候leetcode调用strlen来判题，会报错

## string.h

```c
char* s = "abcdefg";
int Ssize = strlen(s);
```

### strlen

对于自己malloc申请的字符空间，最后一位没有补'\0'的话用strlen函数在不同平台上可能有些问题，leetcode就不太行，经常出问题

## 按位与&

### 按位与结果

| 按位与 | 结果 |
| ------ | ---- |
| 1&1    | 1    |
| 1&0    | 0    |
| 0&1    | 0    |
| 0&0    | 0    |

### 按位与判断奇偶

```c++
if(a & 1){
    printf("奇数");
}else{
    printf("偶数");
}
```

## 前缀和

前缀和可以简单理解为「数列的前n项的和」

[前缀和 & 差分 - OI Wiki (oi-wiki.org)](https://oi-wiki.org/basic/prefix-sum/)

## 排序

### 计数排序

```c
#include <stdio.h>
#include <stdlib.h>
void CountingSort(int *ini_arr, int *sorted_arr, int n) {
	//ini_arr为原始待整理的数组，sorted_arr为会被整理好之后的数组填充的数组
	
	//count_arr为计数数组，里面的下标对应存放ini_arr中每个整数（这里可以看出来并不适用于正整数排序）
	//里面的值对应该下标对应的数在ini_arr的个数
	int* count_arr = (int*)malloc(10000*sizeof(int));//假设计数排序的计数数组支持0-9999的整数
	for(int i = 0;i < 10000;i++){
		count_arr[i] = 0;//计数数组初始化为0
	}
	for(int i = 0;i < n;i++){
		count_arr[ini_arr[i]]++;//对原始数组每个数出现次数计数并记入计数数组
	}
	for(int i = 1;i < 10000;i++){
		count_arr[i]+=count_arr[i-1];//对计数数组每一项计算前缀和并记录，以此推出每个数在新数组中的下标
								   	 //因为这样保证了每个数对应的前缀和是递增的，方便排序
	}
	for (int j = n; j > 0; j--){//从原始数组的最后一位开始，依次向新数组赋值
		sorted_arr[--count_arr[ini_arr[j - 1]]] = ini_arr[j - 1];
		//右值的ini_arr[j - 1]为从被遍历的原始数组里面提出来的值
		//每次提出来一个值，通过count_arr[ini_arr[j - 1]]记录的该值对应的前缀和来找到在新数组里面的下标
		//每次--count_arr[ini_arr[j - 1]]的--是为了将该值对应的前缀和-1，保证下标不重复，
		//并且保证在原始数组后面的数在新数组一定会排在同等值的前面（为了排序的稳定性）
	}
	free(count_arr);
}

int main(){
	int ini_arr[10] = {5,6,8,1,4,9,3,2,7,0};
	int sorted_arr[10];
	CountingSort(ini_arr,sorted_arr,10);
	for(int i = 0;i < 10;i++){
		printf("%d\n",sorted_arr[i]);
	}
	
	return 0;
}
```

## 遍历字符串的方式

```c
for(int i = 0; s[i]; i++)；
```



这是一个常见的字符串遍历方式，使用索引 i 从 0 开始循环递增，条件为 s[i]，即判断当前字符是否为字符串的终止符 '\0'。当遍历到字符串的终止符时，循环结束。

## INF

int是32位有符号整型，左右都只能是1左移/右移31位

INF：

```c
(1<<31)-1
// 输出2147483647
```

在limits.h/climits中，定义了INT_MAX,INT_MIN，可以直接使用

## 取模

注意 C取模的特殊性，当被除数为负数时取模结果为负数，需要纠正

```c
int modulus = (chushu % k + k) % k;
```

## 二分查找模板

```c
int binarySearch(int* arr,int arrSize){
    int l = -1;//l是最左边的左边
    int r = arrSize;//r是最右边的右边
    int mid;
    while(r - l > 1){
        mid = l + (r - l)/2;//不要写(r + l)/2，防止计算溢出
        if(二分查找条件){
            r = mid;
        }else{
            l = mid;
        }
    }
        
        return r;//如果需要返回左边界，那么是l，右边界是r
}
```

## & | ^ ~的应用

### &位与

&：位与，2真才真

[《C语言零基础》（13）位运算 & 的应用 (zsxq.com)](https://articles.zsxq.com/id_h18br7u14eoo.html)

### |位或

|：位或，1真就真

[《C语言零基础》（14）位运算 | 的应用 (zsxq.com)](https://articles.zsxq.com/id_xiawzw94db8q.html)

### ^异或

^：异或，仅1真才真

[《C语言零基础》（15）位运算 ^ 的应用 (zsxq.com)](https://articles.zsxq.com/id_j1aibvhagr03.html)

### ~按位取反

~：按位取反

[《C语言零基础》（16）位运算 ~ 的应用 (zsxq.com)](https://articles.zsxq.com/id_srwwc835ihu9.html)

#### 按位取反得相反数

```c
int x = 18;
printf("%d\n",~x+1);
```

输出-18，也就是说-x可以变成~x+1，那么加号就是负负得正，可以做很多题

## 左移<<

[《C语言零基础》（17）位运算 ＜＜ 的应用 (zsxq.com)](https://articles.zsxq.com/id_uxl45q623qvn.html)

## 右移>>

[《C语言零基础》（18）位运算 >> 的应用 (zsxq.com)](https://articles.zsxq.com/id_cqpbqdfcuwsh.html)

## 补码

```c
printf("%d\n,~1");
```

输出是-2，按道理来说应该是0

是因为int 是32位有符号整型，它最前面的一位是符号位：

`0`000000 0000000 0000000 0000001

按位取反之后变成：

`1`111111 1111111 1111111 1111110

有符号整型的负数是按它的补码来存储的



符号位为0代表它为正数，为1代表它为负数

**正数的补码是它本身，负数的补码为负数对应的正数按位取反后加1，且符号位也改为1**



输出为-2的原因是1111111 1111111 1111111 1111110为-2的补码

-2对应的正数2的二进制0000000 0000000 0000000 0000010按位取反：

1111111 1111111 1111111 1111101

加1：

1111111 1111111 1111111 1111110



补码的`补`：一个32位ku有符号整型数的补码加上它本身位2e32

## 排序

### 快速排序

[快速排序法（详解）_李小白~的博客-CSDN博客](https://blog.csdn.net/qq_40941722/article/details/94396010)

![P1](pics\P1.png) 

不用担心到这一步在中间3这个位置有问题，如果3这里大于基数6的话，右指针还是会继续找下一个比基数6小的数，直到遇到左指针，而左指针此时还指向上一个比基数6小的数，稳得。
