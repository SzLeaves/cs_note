# 210131.定义函数

定义一个函数的步骤：  
* 声明`def`标识符
* 写出函数名称与函数需要的参数，**并在声明语句末尾加上冒号`:`**
* 写出函数主体部分（该部分缩进）
* 如果需要返回值，使用`return`语句
```python
def functions(args0, args1...):
    some sentences...

    return rev0, rev1....
```

例：自定义一个求绝对值的`my_abs`函数：
```python
# -*- coding: utf-8 -*-
def my_abs(number): 
    if number >= 0:
        return number
    else;
        return -number
```

如果没有`return`语句，函数执行完毕后也会返回结果，只是结果为`None`  
**`return None`可以简写为`return`**

> 在Python交互环境中定义函数时，注意Python会出现`...`的提示  
函数定义结束后需要按两次回车重新回到`>>>`提示符下  
>
> 可以该文件的当前目录下启动解释器，用以下命令**导入函数**：  
`from abstest import functions_name`  

## 空函数
如果想定义一个什么事也不做的空函数，可以用`pass`语句：  
`pass`语句实际上相当于占位语句
```python
def nop():
    pass
```

`pass`还可以用在其他语句里，例如：
```python
if age >= 18:
    pass    # 什么也不做
```
缺少了`pass`，代码运行就会有语法错误


## 参数检查
调用函数的参数个数不对，Python解释器会自动检查出来，并抛出`TypeError`：
```python
>>> my_abs(1, 2)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: my_abs() takes 1 positional argument but 2 were given
```

**但要对参数数据类型检查，就要用内置函数`isinstance()`实现：**  
```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')

    if x >= 0:
        return x
    else:
        return -x
```

添加了参数检查后，如果传入错误的参数类型，函数就可以抛出一个错误：
```python
>>> my_abs('A')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "<stdin>", line 3, in my_abs
TypeError: bad operand type
```


> **函数参数的类型检查是必要的**，例如下面的例子：
> ````python
> def functions(args):
>      # 确保args的类型是int
>     if not isinstance(args, int)
>         raise TypeError('bad operand type')
> 
>     return args + 1
> ````
> 由于变量`args`没有特定的数据类型，如果不使用参数检查  
> 在这个函数中，最后返回一个递增值的操作很可能无法完成  


## 返回多个值
Python的`return`语句可以以`tuple`的形式返回多个值：
```python
def mulitValue(x, y, z):
    x = x + 1
    y = y + 1
    z = z + 1

    return x, y, z

print(mulitValue(1, 2, 3))
```
```
# output 
(2, 3, 4)
```
> **注意：函数返回多个值的形式是一个`tuple`**  
所以本质上python的函数返回值其实还是只有一个


多个变量可以同时接收一个`tuple`，将按位置赋给对应的值：
```python
# a=2, b=3, c=4
(a, b, c) = mulitValue(1, 2, 3)
```
> 在语法上，返回一个`tuple`可以省略括号  


## 小结
* 定义函数时，需要确定函数名和参数个数
* 如果有必要，可以先对参数的数据类型做检查
* 函数体内部可以用`return`随时返回函数结果
* 函数执行完毕也没有`return`语句时，解释器会自动加上`return None`
* 函数可以同时返回多个值，但其实就是一个`tuple`
