# 210126.基本数据类型和变量

## 整数
在python中的表示方法和数学上的写法一模一样，例如：1，100，-8080，0等

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便  
**十六进制用`0x`前缀和`0-9`，`a-f`表示**，例如：`0xff00`，`0xa5b4c3d2`等

对于很大的数，例如`10000000000`，很难数清楚0的个数  
**Python允许在数字中间以`_(下划线)`分隔**，因此可以写成`10_000_000_000`  
> 十六进制数也可以写成`0xa1b2_c3d4`


## 浮点数
浮点数可以用数学写法，如1.23，3.14，-9.01等  
对于很大或很小的浮点数，必须用科学计数法表示（e表示法）  
所以：
* `1.23x10^9`就是`1.23e9`，或者`12.3e8`  
* `0.000012`可以写成`1.2e-5`

整数和浮点数在计算机内部存储的方式是不同的  
整数运算永远是精确的，而浮点数运算则可能会有四舍五入的误差

## 小整数对象池和大整数对象池
整数在程序中的使用非常广泛  
Python为了优化速度，使用了**小整数对象池**，避免为整数频繁申请和销毁内存空间  

Python 对小整数的定义是 **`[-5, 256]`**  
小整数的缓冲池是在Python初始化的时候被创建的，不会被垃圾回收  

所以，**位于`-5 ~ 256`这个范围内的整数**使用的都是**同一个对象**：
```python
>>> num1 = 10
>>> num2 = 10

# 使用id()查看对象所在的内存地址 
>>> id(num1)
139826798971472
>>> id(num2)
139826798971472
```

而超出小整数的范围的数值即为大整数，每次都会创建一个新的对象  
**但是处于一个代码块的大整数是同一个对象**  
```python
>>> class A(object):  # 创建class A,在A里面定义4个整数对象
...     a = 100
...     b = 100
...     c = 1000
...     d = 1000
...
>>> class B(object):  # 创建class B，在B里面定义2个整数对象
...     a = 100
...     b = 1000
...
```
```python
>>> A.a is A.b   # 同一代码块，同值的小整数，是同一个对象
True
>>> A.a is B.a   # 不同代码块，同值的小整数，是同一个对象
True
>>> A.c is A.d   # 同一代码块，同值的大整数，是同一个对象
True
>>> A.d is B.b   # 不同代码块，同值的大整数，不是同一个对象
False
```
> **注意**：在终端交互模式操作下，每执行一次命令，所有的大整数都会重新创建  
> 但是在如同Pycharm这样的IDE中，因为程序运行时所有代码都会加载到内存中  
> 所以在这样的环境下，**在大整数池中分配的数值指向的地址是一样的（属于一个整体）**
> ```python
> # 在如同Pycharm这样的IDE环境下，下面语句判断的结果为True
> print(id(A.d), id(B.b), A.d is B.d)
>
> # output
> 140696846398032 140696846398032 True
> ```


## 字符串
字符串是以单引号'或双引号"括起来的任意文本，比如`'abc'`，`"xyz"`等  
如果字符串内部既包含`'`又包含`"`，可以用转义字符`\`来标识  
比如：`'I\'m \"OK\"!'` 表示的字符串内容是： `I'm "OK"!`

**为了简化转义字符的使用，Python还允许用`r''`表示`''`内部的字符串默认不转义**：
```python
print(r'\\\t\\\')
```
```python
# output
\\\t\\\
```

Python允许用'''...'''的格式表示多行内容：
```python
>>> print('''line1
... line2
... line3''')
```
```python
# output:
line1
line2
line3
```

> 在交互式命令行内输入多行内容时，提示符由`>>>`变为`...`  
提示可以接着上一行输入，**注意`...`是提示符，不是代码的一部分**

如果写成python代码：
```python
print('''line1
line2
line3''')
```
> 多行字符串还可以在前面加上`r`使用


### 字符串的`intern`机制

Python解释器为了提高字符串使用的效率和使用性能，做了很多优化  
例如：Python解释器中使用了`intern`（字符串驻留）的技术来提高字符串效率：  

> **如果字符串保存的值相同，字符串对象仅仅会保存一份**，放在一个字符串储蓄池中共用  
> 但是字符串不能被改变，这也决定了字符串必须是不可变对象

实现`intern`机制的方式非常简单，就是通过维护一个字符串存储池  
这个存储池是一个字典结构，如果字符串已经存在于池中就不再去创建新的字符串，直接返回之前创建好的字符串对象  
如果之前还没有加入到该池子中，则先构造一个字符串对象，并把这个对象加入到池子中去，方便下一次获取

但解释器内部对`intern`机制的使用策略是有考究的  
有些场景会自动使用`intern`，有些地方需要通过手动方式才能启动

在终端交互模式中，仅**包含有下划线，数字，字母的字符串才会加入到存储池**：
```python
>>> a = "hello"
>>> b = "hello"
>>> a is b
True
```
如果字符串中带有空格，`intern`机制是不起作用的：
```python
>>> a = "hello "
>>> b = "hello "
>>> a is b
False
```

但是在如同Pycharm这样的IDE中，`intern`总是会起作用：
```python
# 在如同Pycharm这样的IDE环境下，下面语句判断的结果为True
a = "hello "
b = "hello "
print(a is b)

# output
True
```

### 字符串的格式化
------------------
#### 占位符格式化  
python中，字符串的格式化方式与**C语言中的`printf()`函数相同**：
```python
>>> 'Hello, %s' % 'world'
'Hello, world'

>>> 'Hi, %s, you have $%d.' % ('Steven', 1000)
'Hi, Steven, you have $1000.'
```
--------
#### `format()`  
使用字符串的`format()`方法进行格式化  
它会用传入的参数替换字符串中的占位符`{0}`，`{1}`  
```python
>>> 'Hello, {0}, I am {1} years old'.format('Steven', 20)
'Hello, Steven, I am 20 years old'
```
如果想格式化浮点数输出格式，就需要在数字标号后加上`:`并添加格式化选项
```python
{0:.2f}  # 表示标号为0的占位符输出保留2位小数的浮点型数据
{1:3.1f} # 表示标号为1的占位符输出字段宽度为3，保留1为小数的浮点型数据
```
> 注意这里的格式化选项与C语言中的使用方式相同
--------------------
#### `f-string`  
这种格式化方式有点像`format()`方式将占位符直接指定类型的样子：
```python
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```
> 格式：`f'{variable}'` 或 `f'{variable: control_argument}'`

上述代码中，`{r}`被变量`r`的值替换，`{s:.2f}`被变量`s`的值替换  
并且`:`后面的`.2f`指定了格式化参数

> `f-string`方式中，如果字符串包含`{xxx}`，就会以对应变量替换  

### python的字符串编码
在最新的Python 3版本中，字符串是以Unicode编码，支持多语言，例如：
```python
>>> print('包含中文的string')
包含中文的string
```

#### 单字符编码
对于单个字符的编码  
Python提供了`ord()`函数获取字符的整数表示  
`chr()`函数把编码转换为对应的字符：
```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```

> 如果知道字符的整数编码，还可以用十六进制这么写`str`：
> ````python
> >>> '\u4e2d\u6587'
> '中文'
> ````
> 两种写法完全是等价的。


#### 字符串`str`转换为字节`bytes`
如果要在网络上传输，或者保存到磁盘上  
就需要把`str`变为以**字节为单位的`bytes`**

Python对`bytes`类型的数据用**带b前缀的单引号或双引号**表示：
```python
x = b'ABC'
```

要注意区分`'ABC'`和`b'ABC'`  
前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节  

#### 字节编码`encode()`
以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`，例如：
```python
>>> 'ABC'.encode('ascii')
b'ABC'
```
```python
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
```
```python
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in  
position 0-1: ordinal not in range(128)
```

> 注意到最后一句输出的报错  
含有中文的`str`无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围

在`bytes`中，无法显示为ASCII字符的字节，用`\x##`显示。

#### 字节解码`decode()`
反过来，如果我们从网络或磁盘上读取了字节流  
那么读到的数据就是`bytes`，要把`bytes`变为`str`，就需要用`decode()`方法：
```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

如果`bytes`中包含无法解码的字节，decode()方法会报错：
```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in  
position 3: invalid start byte
```

如果`bytes`中只有一小部分无效的字节  
可以传入`errors='ignore'`忽略错误的字节：
```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

#### 计算字符串中字符的数量
要计算`str`包含多少个字符，可以用`len()`函数：
```python
>>> len('ABC')
3
>>> len('中文')
2
```

`len()`函数计算的是`str`的字符数，如果参数换成`bytes`，`len()`函数就计算字节数：
```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```
> 1个中文字符经过UTF-8编码后通常会占用3个字节  
1个英文字符只占用1个字节

> **为了避免乱码问题，应当始终坚持使用UTF-8编码对`str`和`bytes`进行转换**

#### 指定源代码文件的编码
由于Python源代码也是一个文本文件  
所以当源代码中包含中文的时候，**保存源代码就需要务必指定保存为UTF-8编码**  

为了让python解释器按UTF-8编码读取，通常在文件开头写上这两行：
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
第一行注释是为了告诉Linux/OS X系统  
这是一个Python可执行程序，Windows系统会忽略这个注释

第二行注释是为了告诉python解释器  
按照UTF-8编码读取源代码，否则在源代码中写的中文输出可能会有乱码

> 申明了UTF-8编码并不意味着`.py`文件就是`UTF-8`编码的  
必须并且要确保文本编辑器正在使用`UTF-8 without BOM`编码

## 布尔值
布尔值和布尔代数的表示完全一致，一个布尔值只有`True`、`False`两种值  
在Python中，可以直接用`True`、`False`表示布尔值（**注意大小写**)  
也可以通过布尔运算计算出来：
```python
>>> True
True
>>> False
False
>>> 3 > 2
True
>>> 3 > 5
False
```

## 逻辑运算符

**and运算是与运算，只有所有都为True，and运算结果才是True：**
```python
>>> True and True
True
>>> True and False
False
>>> False and False
False
>>> 5 > 3 and 3 > 1
True
```

**or运算是或运算，只要其中有一个为True，or运算结果就是True：**
```python
>>> True or True
True
>>> True or False
True
>>> False or False
False
>>> 5 > 3 or 1 > 3
True
```

**not是取反操作符，它是一个单目运算符，把True变成False，False变成True：**
```python
>>> not True
False
>>> not False
True
>>> not 1 > 2
True
```

## 空值
空值是Python里一个特殊的值，用None表示  
None不能理解为0，因为0是有意义的，而None是一个特殊的空值

Python还提供了列表、字典等多种数据类型，还允许创建自定义数据类型


## 变量与“常量”
**python是一种动态类型语言，定义变量时不需要指定数据类型**  
所以python的变量类型随存储的数据类型来决定

python没有内置诸如C/C++中那样的`const`只读限定符  
实际上python并没有事实意义上的常量

如果想表现一个类型“常量”的类型，**它的名称需要大写**，如：  
```python
PI = 3.14159265359
```
但实际上这个“常量“仍可以修改，python中没有机制保证它不会被改变


## 小结
* Python的整数没有大小限制
* Python的浮点数也没有大小限制，但是超出一定范围就直接表示为`inf`（无限大）
