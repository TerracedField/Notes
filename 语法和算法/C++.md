# C++

[toc]

## 迭代器

### 删除元素

对于vector的迭代器，删除一个元素后仍停留在原来的位置上，让后面的元素自动落到前面来。

### 获取指定索引的迭代器

```c++
vector<int> ints = { 3, 6, 1, 5, 8 };
    int index;
 
    // 使用普通迭代器
    index = 2;
    std::vector<int>::iterator it = ints.begin() + index;
    cout << "Element at index " << index << " is " << *it << endl;
```

**输出:**

Element at index 2 is 1

### 获取迭代器对应索引

```c++
itr-v.begin();
```

### 获取容器最大/最小元素对应索引

```c++
max_element(arr.begin(),arr.end())-arr.begin(); 
```

max_element/min_element函数在algorithm算法库里面。

### auto关键字

```c++
unordered_map<string,vector<string>> omp;
for(int i = 0;i < (int)strs.size();i++){
    string key = strs[i];
    sort(key.begin(),key.end());
    omp[key].emplace_back(strs[i]);
}
for(auto itr = omp.begin();itr != omp.end();itr++){

}
```



## string

### string的方法

[C++ 字符串（string）常用操作总结 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/553061732)

### string的初始化

参数：

```c++
string( string &str, size_type index, size_type length );
```



```c++
string str4( str3, 11, 4 );  //将str3，开始剪切的位置索引，剪切长度
```

[C++string 初始化的几种方式_c++ string初始化_zhangbw~的博客-CSDN博客](https://blog.csdn.net/VariatioZbw/article/details/116592225)



###  string转char*

```c++
	string s1 = "123";
char* s2  = s1.c_str()
```



### string int互转

#### char转int

```C++
char a = '1';
int b = a - '0'; // b = 1
```

#### int转char

```C++
int a = 1;
char b = a + '0'; // b = '1';
```



#### string ->int

1. 采用最原始的string, 然后按照十进制的特点进行算术运算得到int。

```cpp
string s = "123";
int num = 0;
for (int i=0; i<s.size(); ++i) {
    num = 10 * num + (s[i] - '0');
}
```

2. 使用标准库中的 atoi 函数。

```cpp
string s = "123";
int num = atoi(s.c_str());
```

对于其他类型也都有相应的标准库函数，比如浮点型atof(),long型atol()等等。

#### c_str()

[string中c_str()的用法_string.c_str()_Lemonbr的博客-CSDN博客](https://blog.csdn.net/qq_41282102/article/details/82695562)

#### int -> string

1. 采用标准库中的 to_string 函数。

```cpp
int num = 123;
string num_str = to_string(num);
```

### size(),length(),sizeof()区别

[字符串中size()、length()与sizeof()用法及区别_length和sizeof_小魔王降临的博客-CSDN博客](https://blog.csdn.net/qq_30460949/article/details/89109807)

### string的动态

string的内存空间是动态的，但是：

首先,明确string的动态分配不是根据下标分配，而是所储存的数据大小。而下标访问操作是动态分配的空间大小确定后才能进行的操作。

[C++ string类字符串的基本用法详解：初始化、拼接、插入、查找、删除、其他常用方法_string s1从下标2开始_千与千与千的博客-CSDN博客](https://blog.csdn.net/liu_feng_zi_/article/details/102643854)

```c++
string two;//会报错，因为没给string初始大小
int i = 0;
while (sum != 0) {
		two[i++] = sum % 2 + '0';
		sum /= 2;
}
```

### string的比较

[c++ 字符串相等比较_c++判断字符串是否相等-CSDN博客](https://blog.csdn.net/leek5533/article/details/105950109)

**如果要比较的对象是两个string，则利用函数compare() 或者 ==**
若要比较string s1和s2则写为：

```cpp
s1.compare(s2)
```

或者直接大小与比较

```C++
static bool cmp(pair<int,string>& a, pair<int,string>& b){
    if(a.first == b.first){
        return a.second > b.second;//zh
    }
    return a.first < b.first;
}
```



## merge

```c++
vector<int>v1;
vector<int>v2;
//目标容器
vector<int>vTarrget;
vTarrget.resize(v1.size() + v2.size());
merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarrget.begin());
```

## STL深浅拷贝

试了下：

```c++
vector<int> nums1={5,3,9,8,999,2};
vector<int> nums2(nums1);

vector<int> nums1={5,3,9,8,999,2};
vector<int> nums2;
nums2 = nums1;
```

两种拷贝都是深拷贝

## vector

### 方法

[C++ 数组（vector）常用操作总结_c++ vector操作_地球被支点撬走啦的博客-CSDN博客](https://blog.csdn.net/Flag_ing/article/details/123380655)

### 初始化二维vector

[怎样初始化二维vector_二维vector初始化_selfsongs的博客-CSDN博客](https://blog.csdn.net/qq_35987777/article/details/105905452)

```c++
beZero.push_back(vector<int>{i,j});
```

直接带值的初始化

### 只初始化vector的大小

```c++
vector<int> sMap(26);
```



### 初始化vector为0

```c++
vector<int> v(n, 0);
```

### insert()

[C++ STL vector插入元素（insert()和emplace()）详解 (biancheng.net)](http://c.biancheng.net/view/6834.html)

### 空的vector

```C++
if (sLen < pLen) {
            return vector<int>();
}
```





## 累加accumulate

```c++
#include <iostream>
#include <numeric>
using namespace std;
int main(){
	int array[]={1,2,3,4,5,6,7,8,9};//定义数组array
	int sum = accumulate(array.begin(), array.end(),0);
	cout << "数组的和 = " << sum << endl;
	system("pause");
	return 0;
}
```

accumulate ( 形参1 , 形参2 , 形参3 )

前两个形参指定要累加的元素范围，第三个形参则是累加的初值（即sum的初值）

## lower_bound()/upper_bound()

lower_bound( begin,end,num)：闭区间

从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的迭代器，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。



upper_bound( begin,end,num)：开区间

从数组的begin位置到end-1位置二分查找第一个大于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。



[【C++】从没见过这么详细的lower_bound的讲解-CSDN博客](https://blog.csdn.net/weixin_43939593/article/details/105602530)

[C++ lower_bound()函数用法详解 (biancheng.net)](http://c.biancheng.net/view/7521.html)

### lower_bound对pair

[lower_bound对pair使用 - CSDN文库](https://wenku.csdn.net/answer/6x8i1qqww8)

## unordered_map

其实只是统计两个int之间的关系，就不用这个数据结构，可以直接用vector

```c++
vector<int> sMap(26);
vector<int> pMap(26);
for(int i = 0;i < p.size();i++){
    sMap[p[i]-'a']++;//计算s中第一个大小为p.size()的窗口的字母数量
    pMap[p[i]-'a']++;//计算p中每个小写字母的数量
}
```



[C++ unordered_map count()用法及代码示例 - 纯净天空 (vimsky.com)](https://vimsky.com/examples/usage/unordered_map-count-in-c.html)

[c++ unordered_map4种遍历方式_遍历unordered_map_菊头蝙蝠的博客-CSDN博客](https://blog.csdn.net/qq_21539375/article/details/122003559)

### ump的迭代器

```c++
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;
int main()
{
    //创建 umap 容器
    unordered_map<string, string> umap{
        {"Python教程","http://c.biancheng.net/python/"},
        {"Java教程","http://c.biancheng.net/java/"},
        {"Linux教程","http://c.biancheng.net/linux/"} };

    cout << "umap 存储的键值对包括：" << endl;
    //遍历输出 umap 容器中所有的键值对
    for (auto iter = umap.begin(); iter != umap.end(); ++iter) {
        cout << "<" << iter->first << ", " << iter->second << ">" << endl;
    }
    //获取指向指定键值对的前向迭代器
    unordered_map<string, string>::iterator iter = umap.find("Java教程");
    cout <<"umap.find(\"Java教程\") = " << "<" << iter->first << ", " << iter->second << ">" << endl;
    return 0;
}
```

###  ump的插入

```c++
umap.insert(make_pair(1,"Welcome")); 
umap.insert(make_pair(2,"to"));
```

### ump查找

```c++
map.find(key) == map.end()
```

如果key存在，则find返回key对应的迭代器，如果key不存在，则find返回unordered_map::end。

### map一种其它的遍历

```c++
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> mp;
        int maxFreq = 0;
        int length = s.size();
        for (auto &ch : s) {
            maxFreq = max(maxFreq, ++mp[ch]);
        }
        vector<string> buckets(maxFreq + 1);
        //这里开始！！！！！！！！！！！！！！
        //这里开始！！！！！！！！！！！！！！
        //这里开始！！！！！！！！！！！！！！
        //这里开始！！！！！！！！！！！！！！
        for (auto &[ch, num] : mp) {
            buckets[num].push_back(ch);
        }
        //这里结束！！！！！！！！！！！！！！
        //这里结束！！！！！！！！！！！！！！
        //这里结束！！！！！！！！！！！！！！
        //这里结束！！！！！！！！！！！！！！
        string ret;
        for (int i = maxFreq; i > 0; i--) {
            string &bucket = buckets[i];
            for (auto &ch : bucket) {
                for (int k = 0; k < i; k++) {
                    ret.push_back(ch);
                }
            }
        }
        return ret;
    }
};
```



## map

[C++ map中的count()方法_map.count_lemndo的博客-CSDN博客](https://blog.csdn.net/lemn_de/article/details/119325232)

## max_element

```c++
*max_element(nums.begin(),nums.end());//取出数组中最大的元素
```

## 深拷贝和浅拷贝

```c++
class copytest {
	char* str;
public:
	copytest() {
		
	}
	copytest(char* str) {
		this->str = str;
	}
	~copytest() {
		delete[] str;
	}
	void setValue(char* str){
		this->str = str;
	}
	char* getValue(){
		return str;
	}
};


int main() {
	copytest c1("AAA");
	copytest c2 = c1;
	c2.setValue("BBB");
	cout<<c1.getValue()<<endl;
	cout<<c2.getValue()<<endl;
	
	return 0;
}
```

这样写就是浅拷贝，没有自己写的拷贝函数，导致str被delete了两次，导致了doublefree，会报错

[图文并茂，一分钟看懂的C++深浅拷贝__ 菜 -∞的博客-CSDN博客](https://blog.csdn.net/duchenlong/article/details/105687389)

## unoreded_set

[c++ unordered_set的用法_c++ unordered_set用法_kzan的博客-CSDN博客](https://blog.csdn.net/weixin_44223786/article/details/123087010)

## pair的sort

[C++ pair用法及使用sort函数对pair数据进行排序_pair排序-CSDN博客](https://blog.csdn.net/qq_40618919/article/details/124388676)

***sort排序的时候优先比较pair.first，然后比较pair.second***    mp.emplace(make_pair(key,value));

### 在类中定义cmp函数

[错误解决方法：error: reference to non-static member function must be called-CSDN博客](https://blog.csdn.net/weixin_40710708/article/details/111269356)

```C++
class Solution {
public:
   static bool cmp(vector<int>& a,vector<int>& b){//!!!!!!!!!!!!!!!!!!!!静态
   //bool cmp(vector<int>& a,vector<int>& b) 错误
        if(b[1] == a[1])
            return a[0]<b[0];
        else
            return a[1]<b[1];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        stack<vector<int>> s;
        sort(intervals.begin(),intervals.end(),cmp);
        for(int i=intervals.size()-1;i>=0;i--){
            if(!s.empty()){
                if(intervals[i][1]<s.top()[0])
                    s.push(intervals[i]);
                else{
                    int left = min(intervals[i][0],s.top()[0]);
                    int right = max(intervals[i][1],s.top()[1]);
                    s.pop();
                    s.push({left,right});
                }
            }
            else
                s.push(intervals[i]);
        }
        vector<vector<int>> res;
        while(!s.empty()){
            res.push_back(s.top());
            s.pop();
        }
        return res;

    }
};

```



## deque

### deque的方法

[C++ STL deque容器（详解版） (biancheng.net)](http://c.biancheng.net/view/6860.html)

### 反向迭代器

```c++
for (auto it = dq.rbegin(); it != dq.rend(); ++it) {
	cout << *it << " ";
}
```

反向迭代器删除一个元素：

```c++
for(auto itr = dq.rbegin();itr != dq.rend();itr++){
    if((*itr).first == key){
        int temp = (*itr).second;
        dq.push_back(make_pair((*itr).first,(*itr).second));
        dq.erase((++itr).base());//(++itr).base(),itr.base()获得对应的正向迭代器的下一个位置的迭代器，所以要再往前一格
        return temp;
    }
}
```

### deque转vector

```c++
#include <iostream>
#include <deque>
#include <vector>

int main() {
    // 创建一个deque
    std::deque<int> myDeque = {1, 2, 3, 4, 5};

    // 创建一个vector，并将deque中的元素转移至vector
    std::vector<int> myVector(myDeque.begin(), myDeque.end());

    // 打印转换后的vector中的元素
    for (int i : myVector) {
        std::cout << i << " ";
    }
    
    return 0;
}
```

## 常见find

[常用STL容器查找函数find()使用小结_stl find_天赐细莲的博客-CSDN博客](https://blog.csdn.net/CUBE_lotus/article/details/119044709)

## lambda

[C++ Lambda表达式的完整介绍 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/384314474)

## stringstream分割字符串

[分割字符串以及istringstream提取空格分隔的string类型的单词-CSDN博客](https://blog.csdn.net/weixin_44321570/article/details/112846768)

## 优先队列 priority_queue

[C++ priority_queue(STL priority_queue)用法详解 (biancheng.net)](https://c.biancheng.net/view/480.html)

```c++
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        priority_queue<int> q(nums.begin(), nums.end());
        long long ans = 0;
        for (int _ = 0; _ < k; ++_) {
            int x = q.top();
            q.pop();
            ans += x;
            q.push((x + 2) / 3);
        }
        return ans;
    }
};
```

2530题

priority_queue优先队列 动态维护最大值在堆顶

[priority_queue用法详解_priority_queue时间复杂度-CSDN博客](https://blog.csdn.net/Grady_Ne/article/details/77969490)

```
(1) push() push(x) 令x入队，时间复杂度为O(log N), 其中 N 为当前优先队列中元素的个数

(2) top() 获得队首元素(即堆顶元素)，时间复杂度为O(1).

(3) pop() 令队首元素(即堆顶元素)出队，时间复杂度为O(log N).

(4) empty() 检测优先队列是否为空，返回 true则空，返回false 则非空。时间复杂度为O(1)

(5) size() 返回优先队列内元素的个数，时间复杂度为 O(1).
```

### 最大最小堆选择

[C++优先队列/priority_queue(最大堆、最小堆)_priorityqueue c++最小堆-CSDN博客](https://blog.csdn.net/geter_CS/article/details/102580332)

## unsigned循环问题

[C++循环条件中unsigned int踩坑 - 简书 (jianshu.com)](https://www.jianshu.com/p/1dd78b28fa9c)

```C++
for (int i = 0; i < nums.size() - 2; i++)
```

由于循环变量`i`是第一个数的下标，而一共有三个数，我便将循环条件设为了`i < nums.size() - 2`，问题就出在这里：

**`nums.size()`是`unsigned int`类型，如果它是0或1，那么减去2之后就变成了一个很大的无符号数（正数）。**于是循环里面肯定就有非法内存被访问、被使用。

## 二叉树

### 层序遍历

普通：

```c++
void FloorPrint_QUEUE(pTreeNode &Tree) //层序遍历_队列实现
{
    queue < pTreeNode> q;
    if (Tree != NULL)
    {
        q.push(Tree);   //根节点进队列
    }

    while (q.empty() == false)  //队列不为空判断
    {
        cout << q.front()->data << " → "; 

        if (q.front()->leftPtr != NULL)   //如果有左孩子，leftChild入队列
        {
            q.push(q.front()->leftPtr);   
        }

        if (q.front()->rightPtr != NULL)   //如果有右孩子，rightChild入队列
        {
            q.push(q.front()->rightPtr);
        }
        q.pop();  //已经遍历过的节点出队列
    }
}
```

记录层数：

[二叉树层次遍历，记录层次/每层节点数/每层最左边最右边的节点-CSDN博客](https://blog.csdn.net/weixin_43401437/article/details/114895416)

### 红黑树multiset

获取最大最小

[C++ STL multiset容器详解 (biancheng.net)](https://c.biancheng.net/view/7203.html)

[STL教程（九）：C++ STL常用容器之 set/multiset - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/361591650)

## 读写文件

### 读txt

```C++
ifstream file;
file.open("input.txt",ios::in);
string str_line;

while(getline(file,str_line))
{
    vector<int> nums(n,0);
    analyseLine(str_line, n, nums);//读每行的数到nums中
    if(line_index < 4){
        matrix_p.push_back(nums);
    }else{
        matrix_q.push_back(nums);
    }
    line_index++;
}
```

每次getline(file,str_line)就把file打开的txt中的一行读到str_line里面

### 写txt

[C++输出数据到TXT文档中_c++ 导出txt-CSDN博客](https://blog.csdn.net/y363703390/article/details/77749034)

```C++
int main()
{
    int a = 10;
    float b = 10.0;
    ofstream dataFile;
    dataFile.open("dataFile.txt", ofstream::app);
    // 朝TXT文档中写入数据
    dataFile << "dataFile:" << a << "\n"
        << b << endl;
    // 关闭文档
    dataFile.close();
}
```

## 随机数

[C++产生随机数的几种方法_c++随机数生成-CSDN博客](https://blog.csdn.net/onion23/article/details/118558454)

## sort比较

头：algorithm

[C++中sort函数从大到小排序的两种方法_sort从大到小排序-CSDN博客](https://blog.csdn.net/lytwy123/article/details/84503492)

## memset

```C
memset(p, 0, sizeof(p));
```

## pow

在cmath里面

## gcd

C++的标准库中提供了gcd

```C++
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
    int a,b;
    cin>>a>>b;
    cout<<__gcd(a,b)<<endl;
}
```

