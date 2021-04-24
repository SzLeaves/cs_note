# 210423.生成器
通过列表生成式可以直接创建一个列表，但受到内存限制，列表容量肯定是有限的  
创建一个包含100万个元素的列表，不仅占用很大的存储空间  
如果仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了

所以，如果列表元素可以按照<u>某种算法</u>推算出来  
在**循环的过程中**不断推算出后续的元素，这样就不必创建完整的`list`，从而节省大量的空间  
在Python中，这种**一边循环一边计算**的机制，称为**生成器`generator`**

要创建一个`generator`，有很多种方法  

## 列表生成式创建`generator`
**只要把一个<u>列表生成式</u>外围的`[]`改成`()`**，就创建了一个`generator`：
```python
# 列表生成式
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# 生成器
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

> 创建`L`和`g`的区别仅在于最外层的`[]`和`()`，`L`是一个`list`，而`g`是一个`generator`

> `generator`是在for循环的过程中不断计算出下一个元素，并在适当的条件结束for循环  

如果要一个一个打印出来，可以通过`next()`函数获得`generator`的下一个返回值：
```python
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4

.... (省略部分输出)

>>> next(g)
81
>>> next(g)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
StopIteration
```

**`generator`保存的是算法**，每次调用`next(g)`，就计算出`g`的下一个元素的值  
直到计算到最后一个元素，**没有更多的元素时，就会抛出`StopIteration`的错误**

不过正确访问元素的方法是使用for循环，因为`generator`也是可迭代对象：
```python
>>> g = (x * x for x in range(10))
>>> for n in g:
... print(n)
...
0
1
4
9
16

.....(省略部分输出)
```
所以，当创建了一个`generator`后，基本上永远不会调用`next()`  
而是通过`for`循环来迭代它，并且不需要关心`StopIteration`的错误

## 函数创建`generator`
如果推算的算法比较复杂，用类似列表生成式的`for`循环无法实现时，还可以用函数来实现

使用函数创建一个`generator`的办法：
* 定义一个以 `yield` 关键字标识返回值的函数  
* 调用刚刚创建的函数，即可创建一个`generator`

举例：在Fibonacci数列中，**除第一个和第二个数外，任意一个数都可由前两个数相加得**
```
1, 1, 2, 3, 5, 8, 13, 21, 34, ...
```

Fibonacci数列用列表生成式写不出来，但是用函数把它打印出来却很容易：
```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```

> 注意：**赋值语句`a, b = b, a + b` 相当于**
> ```python
> # 注意赋值的顺序
> t = (b, a + b) # t是一个tuple
> a = t[0]       # a = b
> b = t[1]       # b = a + b
> ```
> 但不必显式写出临时变量`t`就可以赋值

上面的函数可以输出Fibonacci数列的前N个数：
```python
>>> fib(6)
1
1
2
3
5
8
'done'
```
可以看出，`fib`函数实际上是定义了Fibonacci数列的推算规则  
从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似`generator`

也就是说，上面的函数和`generator`仅一步之遥  
要把`fib`函数变成`generator`，只需要把`print(b)`改为`yield b`就可以了：
```python
def fib(max):
    n, a, b = 0, 0, 1
        while n < max:
            yield b
            a, b = b, a + b
            n = n + 1
    return 'done'
```

这就是定义`generator`的另一种方法，**如果一个函数定义中包含`yield`关键字**   
那么这个函数就不再是一个普通函数，而是一个`generator`  
此时如果直接调用函数，**解释器并不会直接执行函数中的代码，而是返回一个生成器对象：**
```python
# 返回一个生成器对象
>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>

# 使用next()获取下一个返回值
>>> next(f)
1
>>> next(f)
1
>>> next(f)
2

......(省略部分输出)

# 注意StopIteration返回的异常提示
>>> next(f)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
StopIteration: done
```

到末尾时，用`for`循环调用`generator`，发现拿不到`generator`中return语句的返回值  
**想要拿到返回值必须捕获`StopIteration`错误**，返回值包含在`StopIteration`的`value`中：
```python
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
...
g: 1
g: 1
g: 2
g: 3
g: 5
g: 8
Generator return value: done
```

**所以，`generator`函数和普通函数的执行流程不一样**  
普通函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回  

而使用`generator`函数，它会在每次调用`next()`的时候执行，遇到`yield`语句返回  
再次执行时，它将会**从上次返回的`yield`语句处继续执行**  
在循环过程中不断调用`yield`，就会不断中断  
当然要给循环设置一个条件来退出循环，不然就会产生一个无限数列出来

把函数改成`generator`后，基本上不会用`next()`来获取下一个返回值  
而是直接使用`for`循环来迭代：
```python
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```

举例，定义一个`generator`，依次返回数字1，3，5：
```python
def odd():
    print('step 1')
    yield 1

    print('step 2')
    yield(3)

    print('step 3')
    yield(5)
```

调用该`generator`时，**首先一定要生成一个新的`generator`对象**  
然后用`next()`函数不断获得下一个返回值：
```python
#  生成一个generator对象
>>> o = odd()

# 调用新生成的对象
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
>>> next(o)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
StopIteration
```

如果不生成新的对象，直接调用`odd()`，`generator`只会一直在第一个`yield`之前循环：
```python
>>> next(odd())
step 1
1
>>> next(odd())
step 1
1
>>> next(odd())
step 1
1
```

可以看到，`odd`不是普通函数，而是`generator`  
在执行过程中，遇到`yield`就中断，下次又继续执行  
执行3次`yield`后，已经没有`yield`可以执行了，所以第4次调用`next(o)`就报错
