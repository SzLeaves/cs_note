# 210416. 列表生成式，生成器
列表生成式，即List Comprehensions，是Python内置的创建`list`的生成式


列表生成式可以这么看：  
* 生成的对象是`list`，所以需要一个`[]`把下面的式子都包起来
* 包含<u>至少一个变量`x`</u>的**算法表达式`exp`**
* 结构：`for x in ...`
* 在`for..in`结构后，需要一个`iterable`作为遍历对象

**格式**：
```python
# exp中至少包含一个变量x，用于后面的for循环结构的运算
[ exp for x in iterable ]
```
在生成式中，首先会将可迭代对象`iterable`中的每个元素的结果赋值给`x`  
然后**通过算法表达式`exp`得到一个新的计算值**  
最后把所有通过`exp`得到的<u>计算值</u>**以一个新列表的形式返回**

举例：
生成一个包含1~10数字的序列，可以这么写：
```python
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

但是要生成`[1x1, 2x2, 3x3, ..., 10x10]`怎么做？方法之一是循环：
```python
>>> L = []
>>> for x in range(1, 11):
... L.append(x * x)
...
>>> L
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

但是循环太繁琐，而列表生成式则可以用一行语句代替循环生成上面的`list`：
```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

在写上面的列表生成式时，把要生成的元素的**算法表达式`x * x`放到前面**   
后面跟普通结构的`for`循环，就可以把`list`创建出来  

还可以使用两层循环，可以生成全排列：
```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

三层和三层以上的循环就很少用到了

运用列表生成式，可以写出非常简洁的代码  
例如，列出当前目录下的所有文件和目录名，可以通过一行代码实现：  
```python
>>> import os                       # 导入os模块
>>> [d for d in os.listdir('.')]    # os.listdir可以列出文件和目录
['.pki', '.vscode-oss', '.mozilla', 'git', '.bash_profile', ...]
```

`for`循环可以同时使用两个甚至多个变量  
比如`dict`的`items()`可以同时迭代`key`和`value`：
```
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> for k, v in d.items():
...     print(k, '=', v)
...
y = B
x = A
z = C
```
因此，列表生成式也可以使用两个变量来生成`list`：
```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```

最后把一个`list`中所有的字符串变成小写：
```python
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```

## 列表生成式的`if-else`
在列表生成式的`for`循环后面还可以加上`if`判断  
这样我们就可以筛选出仅偶数的平方：
```python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

举例：以下代码正常会生成一个偶数序列：  
```python
>>> [x for x in range(1, 11) if x % 2 == 0]
[2, 4, 6, 8, 10]
```
但**不能在最后的`if`加上`else`**：
```python
>>> [x for x in range(1, 11) if x % 2 == 0 else 0]
File "<stdin>", line 1
[x for x in range(1, 11) if x % 2 == 0 else 0]
^
SyntaxError: invalid syntax
```
这是因为跟在`for`后面的`if`是一个筛选条件，不能带`else`  

**把`if`写在`for`前面必须加`else`**，否则报错：
```python
>>> [x if x % 2 == 0 for x in range(1, 11)]
File "<stdin>", line 1
[x if x % 2 == 0 for x in range(1, 11)]
^
SyntaxError: invalid syntax
```

这是因为<u>`for`前面的部分是一个表达式，它必须根据x计算出一个结果</u>  
因此，考察表达式：`x if x % 2 == 0`，它无法根据x计算出结果  
因为缺少`else`，必须加上`else`：
```python
# x为偶数时添加到列表中，其余数输出None
>>> [x if x % 2 == 0 else None for x in range(1, 11)]
[None, 2, None, 4, None, 6, None, 8, None, 10]
```
上述`for`前面的表达式`x if x % 2 == 0 else -x`才能根据`x`计算出确定的结果。

可见，**在一个列表生成式中，`for`前面的`if ... else`是表达式**  
而`for`后面的`if`是过滤条件，不能带`else`
