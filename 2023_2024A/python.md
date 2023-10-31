# python

## 遍历list时删除元素

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

