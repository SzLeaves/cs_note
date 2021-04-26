# 210201.函数的参数

在python中，函数的参数一共有5种类型：  
* 位置参数   
* 默认参数  
* 可变参数  
* 关键字参数  
* **命名**关键字参数  


## 位置参数
例：一个可以计算任意n次方的`power(x, n)`函数：  
```python
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x

    return s
```
```python
# output
>>> power(5, 2)
25
>>> power(5, 3)
125
```

`power(x, n)`函数有两个参数：`x`和`n`，这两个参数都是位置参数  
调用函数时，传入的两个值**按照位置顺序依次赋给参数`x`和`n`**


## 默认参数
对于上面的`power(x, n)`函数来说，实际使用的时候可能会经常计算平方  
所以，完全可以把第二个参数`n`的默认值设定为2：
```python
def power(x, n = 2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x

    return s
```

现在，当调用`power(5)`时，相当于调用`power(5, 2)`：
```python
# output
>>> power(5)
25
>>> power(5, 2)
25
```

而对于`n > 2`的其他情况，就必须明确地传入`n`，比如`power(5, 3)`

> 默认参数可以简化函数的调用  

设置默认参数有几点要注意：
* 当函数有多个参数时，**把变化大的参数放前面，变化小的参数放后面**  
变化小的参数就可以作为默认参数

* **必选参数在前，默认参数在后**，否则Python的解释器会报错  
    > 如果没有把默认参数放在最后面：
    > ````python
    > def fun(a, b = 2, c)
    >     ...
    >
    > 
    > # 调用函数
    > fun(1, 3, 4)  # 看起来没问题
    > fun(1, 2)     # ???
    > ````
    > 在上面的函数调用中，对于第二种调用形式  
    > 第二个参数会传递给`b`？还是传递给`c`？
    > 这个地方产生了歧义

* **默认参数必须指向不变对象**  
    举个例子，定义一个函数，传入一个`list`，添加一个`'END'`再返回再返回：
    ```python
    def add_end(L=[]):
        L.append('END')
        return L
    ```
    正常调用时，结果似乎不错：
    ```python    
    >>> add_end([1, 2, 3])
    [1, 2, 3, 'END']
    >>> add_end(['x', 'y', 'z'])
    ['x', 'y', 'z', 'END']
    ```
    使用默认参数调用时，一开始结果也是对的：
    ```python
    >>> add_end()
    ['END']
    ```
    但再次调用`add_end()`时，结果就不对了：
    ```python 
    # 默认参数被修改了!
    >>> add_end()
    ['END', 'END']
    >>> add_end()
    ['END', 'END', 'END']
    ```
    
    因为Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`  
    因为默认参数`L`也是一个变量，指向对象`[]`  
    每次调用该函数，如果改变了`L`的内容，默认参数的内容也会跟着变化
    
    要修改上面的例子，可以用`None`这个不变对象来实现：
    ```python 
    def add_end(L = None):
        # 当传入的参数是默认参数时执行
        if L is None:
            L = []

        L.append('END')
        return L
    ```

    现在无论调用多少次，都不会有问题：
    ```python
    >>> add_end()
    ['END']
    >>> add_end()
    ['END']
    ```
    > 不变对象一旦创建，对象内部的数据就不能修改  
    > 在多任务环境下同时读取不变对象也不需要加锁  
    > 这样就减少了**由于修改数据导致的错误**  

    > 编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象
    

## 可变参数
**函数传入的参数个数可以不确定**，这种参数被称为可变参数  
可变参数允许传入0个或任意个参数，**这些可变参数在函数调用时自动组装为一个`tuple`**

例如，定义一个能计算传入参数之和并返回的函数  
由于参数个数不确定，可以用`list`或`tuple`作为参数传入一组数字：
```python
def calc(numbers)
    sum = 0
    for x in numbers:
        sum = sum + x

    return sum
```
调用的时候，需要先组装出一个list或tuple：
```python
>>> calc([1, 2, 3])
6
>>> calc((1, 3, 5, 7))
16
```

如果使用可变参数，就可以把代码改成这样：
```python
# 可变参数
def calc(*numbers)
    sum = 0
    for x in numbers:
        sum = sum + x

    return sum
```
传入参数就可以这么写了：
```python
calc(1, 2, 3)
calc(1, 3, 5, 7)
```

> **注意：在函数内部，参数`numbers`接收到的是一个`tuple`**   

定义可变参数和定义一个`list`或`tuple`参数相比，只在**参数前面加了一个`*`号**  
调用该函数时，可以传入任意个参数，包括0个参数：
```python
>>> calc(1, 2)
5
>>> calc()
0
```
如果已经有一个`list`或者`tuple`，要调用一个可变参数  
**调用时可以在`list`或`tuple`前面加上`*`**
```python
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```
> `*nums`表示把`nums`这个`list`的所有元素作为可变参数传进去  
这种写法相当有用，而且很常见  
>
> 如果在传入`list`或`tuple`的时候没有加上`*`，传入的参数实际上只有1个：
> ````python
> def fun(*args):
>     print(args)
> 
> li = [1, 3, 5]
> # 比较两者输出的区别
> >>> fun(li)
> ([1, 3, 5],)
> 
> >>> fun(*li)
> (1, 3, 5)
> >>> 
> ````
> 在第一次函数调用中，解释器把传入的`li`当成了一整个参数放入了`args`  
 
> 下面的关键字参数也有类似的方式  
> 不过在关键字参数传入中，如果省略了`**`，python会直接报错


## 关键字参数
关键字参数允许你传入0个或任意个含参数名的参数  
**这些关键字参数在函数内部自动组装为一个`dict`**
> 由于需要组装成一个`dict`，所以传入的参数必须是键-值映射形式

例如，下面的函数传入2个位置参数和1个关键字参数`kw`：
```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`  

在调用该函数时，可以只传入必选参数：
```python
>>> person('Michael', 30)
name: Michael age: 30 other: {}
```

也可以传入任意个数的关键字参数：  
<u>**注意关键字参数的形式一定是`key = value`**</u>
```python
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}

>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```
在关键字参数中，`key=value`中的`key`最后会转换为`str`

> **关键字参数必须传入参数名**，这和位置参数不同  
> 如果没有传入参数名，调用将报错：
> ````python
> >>> person('Jack', 24, 'Beijing', 'Engineer')
> Traceback (most recent call last):
> File "<stdin>", line 1, in <module>
> TypeError: person() takes 2 positional arguments but 4 were given
> ````
> 由于调用时缺少参数名`city`和`job`，Python解释器把这4个参数均视为位置参数  
> 但`person()`函数仅接受2个位置参数


和可变参数类似，也可以先组装出一个`dict`，然后把该`dict`转换为关键字参数传进去：
```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
# 将值从extra中取出
>>> person('Jack', 24, city=extra['city'], job=extra['job'])
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

上面复杂的调用可以用简化的写法：在传入的`dict`名字前面加上`**` 
```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

`**extra`表示把`extra`这个`dict`的所有`key-value`用关键字参数传入到函数的`**kw`参数  

> `kw`将获得一个`dict`，注意`kw`获得的`dict`是`extra`的一份拷贝  
对`kw`的改动不会影响到函数外的`extra`


## 命名关键字参数
对于关键字参数，函数的调用者可以传入**任意不受限制的关键字参数**  
至于到底传入了哪些，就需要在函数内部通过`kw`检查

仍以`person()`函数为例，我们希望检查是否有`city`和`job`参数：
```python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    
    print('name:', name, 'age:', age, 'other:', kw)
```

但是调用者仍可以传入不受限制的关键字参数：
```python
person('Jack', 24, city='Beijing', addr='Chaoyang', zipcode=123456)
```
如果要限制关键字参数的名字，就可以用**命名关键字参数**  
和关键字参数`**kw`不同，命名关键字参数需要一个**特殊分隔符**  
后面的参数被视为命名关键字参数 

例如，只接收`city`和`job`作为关键字参数：
```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```
调用这个函数：
```python
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```

但如果函数定义中<u>已经有了一个可变参数</u>  
后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：
```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```
> 使用命名关键字参数时，要特别注意  
> **如果没有可变参数，就必须加一个`*`作为特殊分隔符**  
> 如果缺少*，Python解释器将无法识别位置参数和命名关键字参数：
> ````python
> def person(name, age, city, job):
>   # 缺少 *，city和job被视为位置参数
>   pass
> ````

命名关键字参数可以有缺省值，从而简化调用：
```python
def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
```
由于命名关键字参数`city`具有默认值，调用时可不传入`city`参数：
```python
>>> person('Jack', 24, job='Engineer')
Jack 24 Beijing Engineer
```


## 参数组合
以上5种参数都可以组合使用，但是参数定义的顺序必须是：
> **必选参数 --> 默认参数 --> 可变参数 --> 命名关键字参数 --> 关键字参数**

比如定义一个函数，包含上述若干种参数：
```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)
```

```python
# 包含命名关键字参数
def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
```

在函数调用的时候，Python解释器自动按照参数位置和参数名把对应的参数传进去
```python
# 调用f1()

>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}

>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}

>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}

>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
```

```python
# 调用f2()

# 在这里，第二个命名关键字参数要求传递一个dict或者key=value形式的值
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}

# 传递一个dict，不要忘记前面的'**'
extra = {'x' = 1, 'y' = 2}
f2(1, 2, d=99, **extra)
```

通过一个`tuple`和`dict`也可以调用上述函数：
```python
# 调用f1()
>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}

>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
```
```python
# 调用f2()
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}

>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```

所以对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它  
无论它的参数是如何定义的

> **不要同时使用太多的组合，否则函数接口的可理解性很差**


## 小结
* Python的函数具有非常灵活的参数形态  
既可以实现简单的调用，又可以传入非常复杂的参数

* 默认参数**一定要用不可变对象**，如果是可变对象，程序运行时会有逻辑错误

* 定义可变参数和关键字参数的语法：
    * `*args`是可变参数，`args`接收的是一个`tuple`

    * `**kw`是关键字参数，`kw`接收的是一个`dict`  

* 调用函数时如何传入可变参数和关键字参数的语法：
    * 可变参数既可以直接传入：`func(1, 2, 3)`  
又可以先组装`list`或`tuple`，再通过`*args`传入：`func(*(1, 2, 3))`

    * 关键字参数既可以直接传入：`func(a=1, b=2)`  
    又可以先组装`dict`，再通过`**kw`传入：`func(**{'a': 1, 'b': 2})`

    * 命名关键字参数**在函数定义时要记得分隔符`*,`的使用**：`def func(*, k1, k2)`  
    调用时需要显式指定关键字变量名称和参数：`func(k1 = value, k2 = value)`

    * 使用`*args`和`**kw`作为<u>形参名</u>是Python的习惯写法  
    当然也可以用其他参数名，但最好使用习惯用法

* 命名的关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值
* 定义命名的关键字参数在没有可变参数的情况下不要忘了写分隔符  
否则定义的将是位置参数
