# Python

[toc]

## 函数

### 指定函数参数和返回类型

[python对函数的参数和返回值进行指定类型和检查_pycharm查看方法的返回值-CSDN博客](https://blog.csdn.net/sgyuanshi/article/details/96192887)

```python
def test(a: int, b: str) -> str:
    print(a, b)
    return "ok"


if __name__ == '__main__':
    test(10, "hello")
```

### Optional

Optional 类型提示用于标记函数的参数或返回值可以是指定的类型，也可以是 None。

[Python 如何使用 Optional 类型提示|极客教程 (geek-docs.com)](https://geek-docs.com/python/python-ask-answer/256_python_how_should_i_use_the_optional_type_hint.html)

## 面向对象

[Python 面向对象 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/python-object.html)

### 调用类内函数

```python
class Solution:
    def rangeSumBST(self, root: Optional[TreeNode], low: int, high: int) -> int:
        if root is None:
            return 0
        x = root.val
        if x > high:  # 右子树没有节点在范围内，只需递归左子树
            return self.rangeSumBST(root.left, low, high)
        if x < low:  # 左子树没有节点在范围内，只需递归右子树
            return self.rangeSumBST(root.right, low, high)
        return x + self.rangeSumBST(root.left, low, high) + \
                   self.rangeSumBST(root.right, low, high)
```



## for i in range

[Python for i in range ()用法详解_for i in range怎么用-CSDN博客](https://blog.csdn.net/qq_45937199/article/details/111463106)

### 从后往前遍历list

[Python - 反向遍历序列(列表、字符串、元组等)的五种方式 - Rocin - 博客园 (cnblogs.com)](https://www.cnblogs.com/allen2333/p/11621201.html)



```python
lists = [0, 1, 2, 3, 4, 5]
# 输出 5, 4, 3, 2, 1, 0
for i in range(5, -1, -1):
    print(lists[i])
 
# 输出5, 4, 3
for i in range(5, 2, -1):
    print(lists[i])
```

### zip()同时遍历两个

```python
nums = [1, 3, -1, -3, 5, 3, 6, 7]
mono_queue = [[nums[0], 0]]

for i, num in zip(range(1, len(nums)), nums[1:]):
    if num > nums[len(nums) - 1]:
        mono_queue.append([num, i])
```

## 空值None

### 判断列表为空

```python
# 1
list = []
if len(list) == 0:
    print('list is empty')
# 2
list = []
if not list:
    print('list is empty')
# 3
EmptyList = []
list = []
if list==EmptyList:
    print('list is empty')
```

直接

```python
list == None
```

没得用

## 数据结构

### str

#### 翻转字符串

[Python 字符串翻转 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python-string-reverse.html)

### 集合

[Python3 集合 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-set.html)

```python
# 1
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
# 2
thisset = set(("Google", "Runoob", "Taobao"))
thisset.add("Facebook")
print(thisset) # {'Taobao', 'Facebook', 'Google', 'Runoob'}
```

[Python3 集合 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-set.html)

#### 集合去重

```python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # 这里演示的是去重功能
{'orange', 'banana', 'pear', 'apple'}
```

#### 判断是否在集合

```python
>>> 'orange' in basket                 # 快速判断元素是否在集合内
True
>>> 'crabgrass' in basket
False
```

#### 集合删除

```python
>>> thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.remove("Taobao")
>>> print(thisset)
{'Google', 'Runoob'}
>>> thisset.remove("Facebook")   # 不存在会发生错误
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Facebook'
```

#### 集合运算符

`|` 是集合的并集运算符

`^` 是集合的对称差运算符

### 字典（哈希表）

[Python 字典(Dictionary) | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/python-dictionary.html)

```python
dc = {}
dc['s'] = 5
print(dc)
dc['s'] += 1
print(dc)
dc.pop('s')
print(dc)
```

```
{'s': 5}
{'s': 6}
{}
```

#### 遍历字典

```python
dict = {
    '小明':129,
    '小兰':148,
    '小红':89
}


for key in dict.keys():
    print (key)
 
---------------结果---------------------
小明
小兰
小红


for value in dict.values():
    print (value)
------------------------
129
148
89
```



#### 字典排序

```python
# 单独打印出排序后的value值
new_sys1 = sorted(sys.values())
print(new_sys1)

# 打印出根据value排序后的键值对的具体值
new_sys2 = sorted(sys.items(),  key=lambda d: d[1], reverse=False)
print(new_sys2)
```

```python
def dictionairy():  
 
    # 声明字典
    key_value ={}     
 
    # 初始化
    key_value[2] = 56       
    key_value[1] = 2 
    key_value[5] = 12 
    key_value[4] = 24
    key_value[6] = 18      
    key_value[3] = 323 
 
 
    print ("按值(value)排序:")   
    print(sorted(key_value.items(), key = lambda kv:(kv[1], kv[0])))     
   
def main(): 
    dictionairy()             
      
if __name__=="__main__":       
    main()
```



[Python 按键(key)或值(value)对字典进行排序 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python-sort-dictionaries-by-key-or-value.html)

### queue

[Python Queue模块 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/208997626)

### deque

[Python collections模块之deque()详解_python collections.deque-CSDN博客](https://blog.csdn.net/chl183/article/details/106958004)

```python
q = collections.deque()
q.append(1) # 从右边加元素
q.appendleft(1) # 从左边加元素
q.pop() # 从右边弹出元素
q.popleft() # 从左边弹出元素
```

### list

[Python3 列表 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-list.html#:~:text=Python3 列表 1 访问列表中的值 与字符串的索引一样，列表索引从 0 开始，第二个索引是 1,operator 模块 ）： ... 8 Python列表函数%26方法 Python包含以下函数%3A )

#### sort

[Python List sort()方法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/att-list-sort.html)

#### 按第二个sort

[Python 根据第二个元素（整数值）对元组列表进行排序|极客笔记 (deepinout.com)](https://deepinout.com/python/python-qa/126_python_sort_a_list_of_tuples_by_2nd_item_integer_value.html)



#### 切片

[[Python\]切片完全指南(语法篇) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/79541418)

##### 翻转list

```python
ls = ls[::-1]
```



#### 初始化list

[Python数组初始化固定长度、求和、简单文本处理_python 初始化固定长度list-CSDN博客](https://blog.csdn.net/qq_41602595/article/details/88688644)

```python
# 0为数组内初始数据，10位数据长度
list = [0]*10 # 这种少用！！！！！！！！！！！！！！可能有问题
# 结果：[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
list2 = [0 for i in range(10)]
# 结果：[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
a = [list()] * 5
a[0].append(1)
# 结果：[[1], [1], [1], [1], [1]]
a = [list() for i in range(5)]
a[0].append(1)
# 结果：[[1], [], [], [], []]
```

#### 用列表初始化列表


```python
list = [[0 for i in range(4)] for i in range(4)]
# [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

#### list部分求和

```python
ls = [4, 5, 9, 4]
print(sum(ls[0:2])) # 9
```

#### 删除列表元素

[Python list列表删除元素（4种方法）_python怎么从列表里删除元素-CSDN博客](https://blog.csdn.net/yelitoudu/article/details/117952380)

#### 遍历list时删除元素

[Python遍历列表时删除元素-CSDN博客](https://blog.csdn.net/weixin_43269020/article/details/88191630)

[Python遍历列表时删除元素_Sui Xin的博客-CSDN博客](https://blog.csdn.net/weixin_43269020/article/details/88191630)

```python
s5 = s
for i in s:
    if i == 1:
        s5.remove(i)
print(s5)
```

下面的赋值操作给新列表是不行的，因为新变量和原变量的物理地址是相同的，可通过`id()`函数查看。

可通过[深拷贝](http://www.runoob.com/w3cnote/python-understanding-dict-copy-shallow-or-deep.html)解决上述问题：

```python
import copy

s5 = copy.deepcopy(s)
for i in s:
    if i == 1:
        s5.remove(i)
print(s5)
```

#### list.sort()多个list嵌套的情况

对于嵌套的，跟cpp差不多，先看第一项，然后比第二项

```python
pairs = [[1, 2], [5, 6], [2, 4], [7, 9], [7, 8]]
pairs.sort()
print(pairs)
# [[1, 2], [2, 4], [5, 6], [7, 8], [7, 9]]
```

#### for i in range

[Python for i in range ()用法详解_for i in range怎么用-CSDN博客](https://blog.csdn.net/qq_45937199/article/details/111463106)

##### 从后往前遍历list

```python
lists = [0, 1, 2, 3, 4, 5]
# 输出 5, 4, 3, 2, 1, 0
for i in range(5, -1, -1):
    print(lists[i])
 
# 输出5, 4, 3
for i in range(5, 2, -1):
    print(lists[i])
```

##### zip()同时遍历两个

```python
nums = [1, 3, -1, -3, 5, 3, 6, 7]
mono_queue = [[nums[0], 0]]

for i, num in zip(range(1, len(nums)), nums[1:]):
    if num > nums[len(nums) - 1]:
        mono_queue.append([num, i])
```

##### 反向遍历list

```python
a = [1, 2, 3]
for i in reversed(a):
    print(i)
```



### 优先队列

[Python 优先队列（priority queue）和堆（heap）_python priority-CSDN博客](https://blog.csdn.net/qq_39463274/article/details/105414188)

默认是最小堆

```python
from queue import Queue   # 队列，FIFO
from queue import PriorityQueue  #优先级队列，优先级高的先输出

q = PriorityQueue()
q.put(2)
q.put(1)
q.put(0)

###############队列（Queue）的使用，/python中也可是用列表（list）来实现队列###############
q = Queue(maxsize) 	#构建一个队列对象，maxsize初始化默认为零，此时队列无穷大
q.empty()		#判断队列是否为空(取数据之前要判断一下)
q.full()		#判断队列是否满了
q.put()			#向队列存放数据
q.get()			#从队列取数据,是取出！！！！不是top()!!!!!
q.qsize()		#获取队列大小，可用于判断是否满/空
```

### 堆实现优先队列（推荐）

[Python中heapq模块浅析_heapq.heappush-CSDN博客](https://blog.csdn.net/chandelierds/article/details/91357784)

### 链表

用数组模拟链表：

[826. 单链表 - AcWing题库](https://www.acwing.com/problem/content/828/)

```C
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
int head, e[N], ne[N], idx;

// 初始化
void init()
{
    head = -1;
    idx = 0;
}

// 在链表头插入一个数a
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}

// 将头结点删除，需要保证头结点存在
void remove()
{
    head = ne[head];
}
```

py:

```python
e = [0 for i in range(100005)]
ne = [0 for i in range(100005)]
head = -1
idx = 1 # e从1开始存

def insert_head(x: int):
    global idx
    global head
    e[idx] = x
    ne[idx] = head
    head = idx
    idx += 1

def insert_k(k: int, x: int):
    global idx
    e[idx] = x
    temp = ne[k]
    ne[k] = idx
    ne[idx] = temp
    idx += 1

def delete_k(k: int):
    global head
    if k == 0:
        head = ne[head]
    else:
        ne[k] = ne[ne[k]]

cnt = 0
M = int(input())
for i in range(M):
    line = list(map(str,input().split()))
    if line[0] == 'H':
        cnt += 1
        line[1] = int(line[1])
        insert_head(line[1])
    elif line[0] == 'I':
        cnt += 1
        line[1] = int(line[1])
        line[2] = int(line[2])
        insert_k(line[1], line[2])
    else:
        cnt -= 1
        line[1] = int(line[1])
        delete_k(line[1])

temp = head
for i in range(cnt):
    print(e[temp], end = ' ')
    temp = ne[temp]

```

### 栈

用deque代替，只用append和pop，都是从右边进出

## 深拷贝

[Python 直接赋值、浅拷贝和深度拷贝解析 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/python-understanding-dict-copy-shallow-or-deep.html)

浅拷贝：

```python
>>>a = {1: [1,2,3]}
>>> b = a.copy()
>>> a, b
({1: [1, 2, 3]}, {1: [1, 2, 3]})
>>> a[1].append(4)
>>> a, b
({1: [1, 2, 3, 4]}, {1: [1, 2, 3, 4]})
```

深拷贝：

```python
>>>import copy
>>> c = copy.deepcopy(a)
>>> a, c
({1: [1, 2, 3, 4]}, {1: [1, 2, 3, 4]})
>>> a[1].append(5)
>>> a, c
({1: [1, 2, 3, 4, 5]}, {1: [1, 2, 3, 4]})
```



## 三目运算符

```python
max = a if a>b else b
```



## 二分查找

```python
import bisect

ls = [1, 2, 5, 7]
x = 3
index1 = bisect.bisect(ls, x)  # 第1个参数是列表，第2个参数是要查找的数，返回值为索引
print(index1) # 2
index2 = bisect.bisect_left(ls, x)
print(index2) # 2
index3 = bisect.bisect_right(ls, x)
print(index3) # 2
```

bisect.bisect和bisect.bisect_right返回大于x的第一个下标(相当于C++中的upper_bound)，bisect.bisect_left返回大于等于x的第一个下标(相当于C++中的lower_bound)。



## split()

[python split 多个空格分隔-CSDN博客](https://blog.csdn.net/qq_32507417/article/details/107505719)

[两种方法分割python多空格字符串_python字符串多个空格后面的字符-CSDN博客](https://blog.csdn.net/lwgkzl/article/details/82145387)

## 输入输出

### 接受列表的输入

[【Python】列表元素输入_列表输入-CSDN博客](https://blog.csdn.net/huitinfeng/article/details/100568540)

```python
#希望的输入
#4
#4 5 9 4
#输入n
n = int(input())
#输入stones
ls = input().split()
stones = [int(x) for x in ls]
```

### 读取多行输入

[Python读取多行键盘输入_python读取多行输入-CSDN博客](https://blog.csdn.net/qq_26884501/article/details/89194211)

### 读1个整数

```python
n = int(input())
```

### 读几个整数

```python
n,k,m=map(int,input().split())
```



### 读1行整数

```python
nums = list(map(int, input().split()))
```

### 读多行整数

```python
n = int(input())
matrix = []
for i in range(n):
    matrix.append(list(map(int, input().split())))
print(matrix)
```

### 输出小数位数

```python
# 使用{:.nf} n表示取小数点后3位
x = 3.1415926
print('{:.3f}'.format(x))
```



##  位移运算

```python
print(2>>3) # 2//2^3 = 0，2的二进制10，向右最多移动2位后，所以多移动无疑为0
print(2>>1) # 2*2^1 = 4，向右移动一位为01,
print(3>>4) # 3*2^4 = 48,3的二进制为11，向右移动四位后00
print(3>>1) # 3*2^4 = 48,3的二进制为11，向右移动一位后为01
```

## int和str等等类型互转

```python
str_num = '99'
num = int(str_num)
```

**注意**，应该使用`int()`函数而不是`(int)`

## swap

```python
a = 18
b = 30
a, b = b, a
```

## 变量类型

### int和str等等类型互转

```python
str_num = '99'
num = int(str_num)
```

**注意**，应该使用`int()`函数而不是`(int)`

### 判断是否为int或str

[判断 Python 字符串中是否为：整数或字符或标题_python怎么判断一个字符串是整数-CSDN博客](https://blog.csdn.net/sinat_28631741/article/details/83270869)

### 判断字符串大小写

```python
# 获取用户输入
letter = input("请输入一个字母：")
 
# 判断字母是否为大写
if letter.isupper():
    print("该字母为大写字母。")
 
# 判断字母是否为小写
elif letter.islower():
    print("该字母为小写字母。")
 
# 如果既不是大写字母也不是小写字母，则输出错误信息
else:
    print("输入错误，请输入一个字母。")
```

### 大小写转换

```python
#encoding:UTF-8
msg = 'www.BAIDU.com.123'
print(msg.upper())  #upper()函数，将所有字母都转换成大写
print(msg.lower())  #lower()函数，将所有字母都转换成小写
print(msg.capitalize())  #capitalize()函数，将首字母都转换成大写，其余小写
print(msg.title())  #title()函数，将每个单词的首字母都转换成大写，其余小写
```

## \*解包符号

```python
def resnet_block(input_channels, num_channels, num_residuals,
                 first_block=False):
    blk = []
    for i in range(num_residuals):
        if i == 0 and not first_block:
            blk.append(Residual(input_channels, num_channels,
                                use_1x1conv=True, strides=2))
        else:
            blk.append(Residual(num_channels, num_channels))
    return blk



# *是解包符号
b2 = nn.Sequential(*resnet_block(64, 64, 2, first_block=True))
b3 = nn.Sequential(*resnet_block(64, 128, 2))
b4 = nn.Sequential(*resnet_block(128, 256, 2))
b5 = nn.Sequential(*resnet_block(256, 512, 2))
```

在Python中，`*`符号在函数调用时用作解包操作符（unpacking operator）。当你看到`*resnet_block(...)`这样的表达式时，它表示将`resnet_block`函数返回的序列（通常是列表或元组）中的元素解包，并将它们作为独立的参数传递给`nn.Sequential`函数。

以你给出的代码为例，`resnet_block`函数可能返回一个包含多个模块的列表，这些模块是构建残差块（ResNet block）所需的。然后，使用`*`操作符将这些模块解包，并将它们作为参数传递给`nn.Sequential`，这样`nn.Sequential`就会按照列表中的顺序将这些模块串联起来。

例如，如果`resnet_block`返回了一个列表`[conv1, conv2, conv3]`，那么`nn.Sequential(*[conv1, conv2, conv3])`就等同于`nn.Sequential(conv1, conv2, conv3)`。

在你的代码中，`b2`、`b3`、`b4`和`b5`是通过调用`resnet_block`函数，并使用`*`操作符将返回的模块列表解包，然后创建了四个`nn.Sequential`对象，每个对象包含一个残差块的不同层级的模块。

## ...的用法

`...` 被用在索引操作中，表示对数组 `x` 和 `target` 进行切片操作时，除了指定的维度外，其他所有维度都将被包含进来。

假设这 `x` 是一个形状为 `(N, C, H, W)` 的四维数组

```python
dice += dice_coeff(x[:, channel, ...], target[:, channel, ...], ignore_index, epsilon)
```

等价于：

```python
dice += dice_coeff(x[:, channel, :, :], target[:, channel, :, :], ignore_index, epsilon)
```

