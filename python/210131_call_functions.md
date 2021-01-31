# 210131.调用函数 

调用函数，需要知道这个函数的名称与参数：
> 格式：`function_name(args0, args1...)`

#### 例1：调用`abs`函数：
```python
>>> abs(100)
100
>>> abs(-20)
20
>>> abs(12.34)
12.34
```

如果传入的参数数量不对，会报`TypeError`，并给出详细错误信息：
```python
>>> abs(1, 2)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: abs() takes exactly one argument (2 given)
```

如果传入的参数类型不能被函数所接受，也会报`TypeError`的错误
```python
>>> abs('a')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: bad operand type for abs(): 'str'
```

> Python的内建`build-in`函数列表：  
> http://docs.python.org/3/library/functions.html#abs   
> 也可以在交互式命令行通过`help(abs)`查看abs函数的帮助信息

#### 例2：`max`函数  
`max()`可以接收任意多个参数，并返回最大的那个：
```python
>>> max(1, 2)
2
>>> max(2, 3, 1, -5)
3
```

#### 例3：数据类型转换  
Python内置的常用函数还包括数据类型转换函数   
比如`int()`函数可以把其他数据类型转换为整数：
```python
# 注意整数转换的“趋零截断”
>>> int('123')
123
>>> int(12.34)
12

>>> float('12.34')
12.34

>>> str(1.23)
'1.23'
>>> str(100)
'100'

>>> bool(1)
True
>>> bool('')
False
```

> **函数名其实就是指向一个函数对象的引用**  
> 完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”：
> ````python
> >>> a = abs   # 变量a指向abs函数
> >>> a(-1)     # 所以也可以通过a调用abs函数
> 1
> ````

> ***注意！***   
> 由于函数名本身是一个指向函数对象的引用，**这个引用的指向是可以更改的**  
所以操作不慎就会造成函数“消失”的危险！
> ````python
> # 正常的abs函数
> >>> abs(-100)
> 100
> 
> # 不慎更改了函数的指向：用函数名作为一个变量名
> >>> abs = 10
> 
> # 现在，abs作为一个变量名，指向一个整数，原来的函数“消失”了
> >>> abs
> 10
> >>> abs(-100)
> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
> TypeError: 'int' object is not callable
> >>> 
> ````
> 若想恢复正常，需要重启python的交互环境！
