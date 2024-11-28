[toc]

numpy的array是一整块

## 函数

### 点积

[np.dot()函数的用法详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/353753915)

### 转置

A是矩阵

```python
A = A.T
```

### 矩阵相乘

AT是矩阵，W是矩阵，符合矩阵乘法条件

```python
Z = np.matmul(AT, W)
```

或者

```python
Z = AT @ W
```



## 结构

对于这个nparray nums

```python
nums = np.array([1,2,3,4,5,6])
```

```python
nums.reshape(2, 3)
```

变成

此为2\*3的矩阵

```
[[1, 2, 3],
 [4, 5, 6]]
```

## 
