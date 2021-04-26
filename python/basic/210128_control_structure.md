# 210128.控制结构

Python关于`True/False`的判定：
* **如果判断对象为`0`,`""`(空字符串),`None`，则判断结果为`False`**
* 如果判断对象为**非空值，非`0`**，则判断结果为`True`

## 条件判断

### `if`,`else`
> 格式：`if (...) : [Tabs] some sentence...`  
```python
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
```

根据Python的缩进规则，如果`if`语句判断是`True`    
就把**缩进**的两行`print`语句执行了，否则什么也不做

也可以给`if`添加一个`else`语句
```python
age = 3

if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')
```

> 务必注意：
> * <u>**`if`和`else`后面不要少写了冒号"`:`"**</u>
> * **在控制语句中不要少了缩进（确保按下Tab键会缩进四个）**

### `elif`
**`elif`是`else-if`的缩写**，可以有多个`elif`  
所以`if`语句的完整形式就是：
```python
age = 3

if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

伪代码：
```
if <条件判断1>:
    <执行1>

elif <条件判断2>:
    <执行2>

elif <条件判断3>:
    <执行3>

else:
    <执行4>
```

> `if`判断条件还可以简写，比如写：
> ````python
> if x:
>   print('True')
> ````
> 只要`x`是**非零数值、非空字符串、非空`list`等**，就判断为`True`，否则为`False`

不同的`if-else`语句可以嵌套使用，但是不建议30层以上的嵌套：
```python
if _expression_a:
    if _expression_b:
        if _expression_c:
            ....
        elif _expression_d:
            ....
```

### 简化的判断：三目运算符
C与Java的判断三目运算符类似于这样：
```c
[ Expression ] ? True_statements : False_statements
```  

**Python的则是这样：**
```python
True_statements if [ Expression ] else False_statements
```

当`[ Expression ]`的值为`True`时，执行`True_statements`的语句  
否则执行`False_statements`的语句


## 循环

### `for x in...`

`for x in...`循环结构依次把`list`或`tuple`中的每个元素迭代出来  
**执行时它会把列表每个元素代入变量`x`**，然后执行缩进块的语句：
```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

执行这段代码，会依次打印`names`的每一个元素：
```python
# output
Michael
Bob
Tracy
```

> **注意`for x in...`后面不要少写了冒号`:`**

`for`循环里同时引用两个变量，在Python里是很常见的：
```python
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
... print(x, y)
...
1 1
2 4
3 9
```

### `for-else`
**只要没有使用`break`**，`for`循环正常结束后就会执行`for-else`子句：
```python
# 碰到break，循环提前结束，for-else子句不会执行
for index in range(3):
    if index == 2:
        break
    print(index, end=' ')
else:
    print('for-else子句执行')

print('\n-----')

# 循环正常结束，for-else子句执行
for index in range(3):
    print(index, end=' ')
else:
    print('for-else子句执行')
```
```
# output
0 1 
-----
0 1 2 for-else子句执行
```

### `while`
`while`循环只要条件满足，就不断循环，条件不满足时才会退出循环  

例：计算100以内所有奇数之和：
```python
sum = 0

n = 99
while n > 0:
    sum = sum + n
    n = n - 2
    print(sum)
```

在循环内部变量`n`不断自减，直到变为-1时，不再满足`while`条件，循环退出

### `break`
在循环中，`break`语句可以提前退出循环  

例如，本来要循环打印1～100的数字：
```python
n = 1
while n <= 100:
    print(n)
    n = n + 1

print('END')
```

如果要提前结束循环，可以用`break`语句：
```python
n = 1
while n <= 100:
    if n > 10:    # 当n = 11时，条件满足，执行break语句
        break     # break语句会结束当前循环
    print(n)
    n = n + 1

print('END')
```
> 注意观察控制结构中的缩进，十分重要

执行上面的代码可以看到，打印出1~10后，紧接着打印`END`，程序结束


### `continue`
在循环过程中，也可以通过`continue`语句，跳过当前的这次循环，直接开始下一次循环

例：只打印10以内的奇数：  
```python
n = 0
while n < 10:
    n = n + 1

    if n % 2 == 0:  # 如果n是偶数，执行continue语句
        continue    # continue语句会直接继续下一轮循环
    print(n)
```
执行上面的代码可以看到，打印是1，3，5，7，9

### `pass`
`pass`是空语句，是为了保持程序结构的完整性，它不做任何事情，一般用做占位语句
```python
for x in range(3):
    pass
```
`pass`很多时候用于测试用途  
如果在循环结构中不想写任何内容，但是不加`pass`，解释器会报语法错误
```python
>>> for x in range(3):
... 
  File "<stdin>", line 2
    ^
IndentationError: expected an indented block
```
> `pass`相当于C语言中单独使用的分隔符"`;`"，表示一个空语句

### `range()`
**Python提供一个`range()`函数，可以生成一个整数序列：**  
```python
>>> range(1, 10)
range(1, 10)
```

使用`list()`方法可以直接转换成`list`列表：
```python
>>> list(range(1, 10))
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**`range()`可以作为迭代器`（iterator）`直接放入`for x in ...`循环中：**
```python
for x in range(3):
    print(x)

# output
0
1
2
```

如果我们想计算1-10的整数之和，可以这么写：
```python
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
    print(sum)
```

但是这样做有些繁琐，不如使用`range(0, 10)`或者`range(10)`代替：
```python
sum = 0
for x in range(10):
    sum = sum + x
    print(sum)
```

> `range(n)`生成的序列是一个**从0开始，小于n**的整数序列  
> `range(n, m)`生成的序列是一个**从n开始，小于m**的整数序列  
> `range(n, m, step)` 生成的序列是一个**从n开始，小于m，步长为step的整数序列**

在python的控制结构中，如果想写一个类似C/C++这样的循环：
```c
// C/C++循环
for (int i = 0; i < 5; i++) {
    ...
}
```

在python中可以这么写（相当于每次索引数递增1的循环）：
```python
# python循环
for x in range(5)
    ...
```

如果需要每次对索引数递增`n`次，可以用上`while`循环的实现：
```python
n = 0
while (n < 5)
    ...
    # 每次循环对索引数递增2
    n = n + 2;
```
在使用`for x in...`结构时一定要记住，`in...`需要的是一个**迭代器`(iterator)`对象**  
比如`list`,`tuple`,`range(n, m)`

> **可以进行迭代的对象**，被称之为**迭代器`(Iterator)`**

## 迭代器 `(Iterator)`

迭代器`(Iterator)`是一个对象，它的工作是遍历并选择序列中的对象  
它提供了一种**访问一个容器`(container)`对象中的各个元素，又不暴露对象内部细节的方法**   
通过迭代器，开发人员不需要了解容器底层的结构，就可以实现对容器的遍历  
由于创建迭代器的代价小，因此迭代器通常被称为轻量级的容器

在Python中，**任何实现了`__iter__()`和`__next__()`方法的对象都是迭代器**  
`__iter__`返回迭代器自身，`__next__`返回容器中的下一个值  
如果容器中没有更多元素（比如已经遍历到容器尾部），则抛出`StopIteration`异常

迭代器的特点：
* 迭代器是一个<u>可以记住遍历的位置的对象</u>
* 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束
* 迭代器**只能往前不会后退**
* 迭代器有两个基本的方法：`iter()`和`next()`
* 生成器一定是迭代器（反之不成立）
* 可以使用迭代器进行访问的对象：
    * 序列：字符串`(str)`、列表`(list)`、元组`(tuple)`
    * 非序列：字典`(dict)`、文件
    * 自定义类：用户自定义的类实现了`__iter__()`或`__next__()`方法的对象  

> `__iter__()` 方法返回一个特殊的迭代器对象  
> 这个迭代器对象实现了`__next__()`方法并通过`StopIteration`异常标识迭代的完成  

> `__next__()` 方法会返回下一个迭代器对象

## 迭代 `(Iteration)`
如果给定一个`list`或`tuple`之类的**迭代器`(iterator)`**  
我们可以**通过`for`循环**来遍历这个`list`或`tuple`  
这种遍历我们称为**迭代`（Iteration）`**  

在Python中，迭代是通过`for ... in`来完成的  
```python
for index in list:
    n = index
```

而很多语言（比如C），迭代`list`是通过下标完成的：
```c
// C 
for (i = 0; i < list_length; i++) {
    // 取出数组中的元素
    n = list[i];
}
```

可以看出，**Python的`for`循环抽象程度要高于C语言等的`for`循环**  
因为Python的`for`循环不仅可以用在`list`或`tuple`上，还可以作用在其他可迭代对象上

`list`这种数据类型虽然有下标，但很多其他数据类型是没有下标的  
但是，**只要是可迭代对象**，无论有无下标，都可以迭代，比如`dict`就可以迭代：
```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
```
```python
>>> for key in d:
...     print(key)
...
a
c
b
```
> `dict`的存储不是按照`list`的方式顺序排列，所以迭代出的结果顺序很可能不一样

**默认情况下，`dict`迭代的是`key`**  
如果要迭代`value`，可以用`for value in d.values()`  
```python
>>> for value in d.values():
...     print(value)
... 
1
2
3
```

如果要同时迭代`key`和`value`，可以用`for k, v in d.items()`  
```python
>>> for key,value in d.items():
...     print(key, value)
... 
a 1
b 2
c 3
```

由于字符串也是可迭代对象，因此，也可以作用于`for`循环：
```python
>>> for ch in 'ABC':
...     print(ch)
...
A
B
C
```

所以，当我们使用`for`循环时，**只要作用于一个可迭代对象**，`for`循环就可以正常运行  
而不需要太关心该对象究竟是`list`还是其他数据类型

## 判断对象是否可迭代
通过`collections`模块的`Iterable`类型，使用`isinstance()`方法判断：

```python
>>> from collections import Iterable # 导入模块
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

## 实现下标索引循环
Python内置的`enumerate`函数可以把一个`list`变成索引-元素对  
这样就可以在`for`循环中同时迭代索引和元素本身：
```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```
> 这种效果类似于C语言所使用的下标循环


## 小结
* **在所有的控制结构语句中都不要少了缩进**  
**在控制语句开头不要少了冒号`:`**

* `break`语句可以在循环过程中直接退出循环  
而`continue`语句可以提前结束本轮循环，并直接开始下一轮循环  
这两个语句通常都必须配合`if`语句使用

* 注意不要滥用`break`和`continue`语句  
`break`和`continue`会造成代码执行逻辑分叉过多，容易出错
