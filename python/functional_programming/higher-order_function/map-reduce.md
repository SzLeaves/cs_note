# `map/reduce`

Python内建了`map()`和`reduce()`函数

## `map()`
`map()`函数接收两个参数  

`map(func, *iterables)`
* `func`：函数，该函数可接收多个参数
* `iterable`：可迭代对象 

`map`将传入的函数**依次作用到序列的每个元素**，并把结果作为**新的`Iterator`返回**

也就是：序列里每一个元素都被当做`x`变量  
放到一个函数`f(x)`里，其结果是`f(x1)、f(x2)、f(x3)...`组成的新的`Iterator`

> 从Python3开始，`map`将返回迭代器`Iterator`

举例：有一个函数`f(x) = x * x`  
要把这个函数作用在一个`list`上，就可以用`map()`实现如下：
```python
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

过程示意图：
```
            f(x) = x * x                 --> 作用函数

                  │
                  │
  ┌───┬───┬───┬───┼───┬───┬───┬───┐
  │   │   │   │   │   │   │   │   │
  ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼

[ 1   2   3   4   5   6   7   8   9 ]    --> 被作用对象
--------------------------------------
             map对f(x)映射 
--------------------------------------
  │   │   │   │   │   │   │   │   │ 
  │   │   │   │   │   │   │   │   │
  ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼   ▼

[ 1   4   9  16  25  36  49  64  81 ]    --> 作用结果
```


`map()`传入的第一个参数是`f`，即函数对象本身  
由于结果`r`是一个`Iterator`，`Iterator`是惰性序列  
因此通过`list()`函数让它把整个序列都计算出来并返回一个`list`

实际上不需要`map()`函数，写一个循环，也可以计算出结果：
```python
L = []
for n in [1, 2, 3, 4, 5, 6, 7, 8, 9]:
    L.append(f(n))
print(L)
```
但是上面的循环代码，没办法一眼看明白  
“把`f(x)`**作用在`list`的每一个元素**，并把结果生成一个**新的`list`**”这样的意思

所以，`map()`作为高阶函数，事实上它把运算规则抽象了  
它可以计算任意复杂的函数，比如，把这个`list`所有数字转为字符串：
```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

## `reduce`
> 在Python3中，`reduce`已不在内建函数中，**所以要从`functools`库导入才能使用**：  
> `from functools import reduce`

`reduce`需要至少两个参数：  

`reduce(func, seq[, init]) `  
* `func`：函数，该函数**只能接收两个参数**
* `seq`：序列
* `init`：初始值（可选）

`reduce`把一个**函数**作用在一个序列`[x1, x2, x3, ...]`上  
把结果继续和序列的下一个元素做累积计算，其效果就是：
```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

`reduce`在迭代序列的过程中，首先把**前两个元素（只能两个）**传给函数  
函数加工后，然后把得到的结果，和**第三个元素**作为两个参数传给函数参数   
函数加工后得到的结果，又和**第四个元素**一起作为两个参数传给函数参数，依次类推

比方说对一个序列求和，就可以用`reduce`实现：
```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```
> 对序列元素求和也可以用`sum()`实现

把序列`[1, 3, 5, 7, 9]`变换成整数`13579`：
```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y   # 转换成十进制数形式
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

字符串`str`也是一个序列，对上面的例子稍加改动，配合`map()`  
就可以写出把`str`转换为`int`的函数：
```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
        # 使用dict进行key-value对应标识
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
        # 取出str中对应的value
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579
```

整理成函数：
```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
    return x * 10 + y

def char2num(s):
    return DIGITS[s]
    return reduce(fn, map(char2num, s))
```
