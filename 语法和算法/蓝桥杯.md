## python输入

### 读1个整数

```python
n = int(input())
```

### 读1行整数

```python
nums = list(map(int, input().split()))
```



```python
data = [int(x) for x in input().split()]
```



### 读多行整数

```python
n = int(input())
matrix = []
for i in range(n):
    matrix.append(list(map(int, input().split())))
print(matrix)
```

## python输出

不换行输出

```python
letters = {'g': 1, 'o': 2, 'd': 1}
for letter in letters.keys():
    for i in range(letters[letter]):
        print(letter, end=' ')
```

