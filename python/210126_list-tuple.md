# 210126.使用`list`和`tuple`

## `list`
Python内置的一种数据类型是**列表：`list`**  
**`list`是一种有序的集合，可以随时添加和删除其中的元素**  

`list`在**操作方式上与数组类似**：  
```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates
['Michael', 'Bob', 'Tracy']
```

变量classmates就是一个`list`  

#### 获取长度
用`len()`函数可以获得`list`元素的个数
```python
>>> len(classmates)
3
```

#### 通过索引访问元素
用索引来访问`list`中每一个位置的元素，**索引是从0开始**：
```python
>>> classmates[0]
'Michael'
>>> classmates[1]
'Bob'
>>> classmates[2]
'Tracy'
>>> classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```
当索引超出了范围时，Python会报一个IndexError错误  
要确保索引不要越界，<u>记得最后一个元素的索引是`len(classmates) - 1`</u>

**还可以用`-n`（索引的负数）表示倒数递第`n`个索引下的数字：**
```python
>>> classmates[-1]
'Tracy'

# 以此类推，可以获取倒数第2个、倒数第3个：
>>> classmates[-2]
'Bob'
>>> classmates[-3]
'Michael'

# 此时也要注意不要超出索引范围：
>>> classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: `list` index out of range
```

#### 添加元素
`list`是一个可变的有序表，**可以使用`append`方法**往`list`中追加元素到**末尾**：
```python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

**也可以使用`insert`方法把元素插入到指定的位置**，比如索引号为1的位置：
```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

#### 删除元素
要删除`list`末尾的元素，**用`pop()`方法：**
```python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

要删除指定位置的元素，**用`pop(i)`方法，其中`i`是索引位置：**
```python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

#### 修改元素的值

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：
```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

`list`里面的元素的数据类型也**可以不同**：
```python
>>> L = ['Apple', 123, True]
```

**`list`元素也可以是包含另一个`list`：**
```python
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
```

> **注意s只有4个元素**，其中`s[2]`又是一个`list`，可以这样拆开写：
> ````python
> >>> p = ['asp', 'php']
> >>> s = ['python', 'java', p, 'scheme']
> ````

要拿到`'php'`，可以写`p[1]`或者`s[2][1]`  
因此s可以看成是一个**二维数组**，同理也可以写出三维数组形式的`list`：
```python
>>> list = [1, [2, [3]]]
>>> list[1][1][0]
3
```

如果一个`list`中一个元素也没有，就是一个空的`list`，它的长度为0：
```python
>>> L = []
>>> len(L)
0
```


## `tuple`
另一种有序列表叫**元组：`tuple`**  
`tuple`和`list`非常类似，但是`tuple`一旦初始化就不能修改：  
```python
>>> classmates = ('Michael', 'Bob', 'Tracy')
```

现在，classmates这个`tuple`不能变了，它也没有`append()`，`insert()`这样的方法  

其他获取元素的方法和`list`是一样的  
可以正常地使用`classmates[0]，classmates[-1]`，**但不能赋值成另外的元素**  

####  “不可变”
所谓的“不可变”，在`tuple`这里是指：  
* **长度不可变**
* **初始化后元素的指向（可以看成元素的值）不可变**  

注意：**对于第二点，`list`比较特殊**，`list`内的元素还是可以重新赋值的：
```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

不可变的`tuple`有什么意义？因为`tuple`不可变，所以代码更安全  
在某些特殊业务背景下，就需要用`tuple`代替`list`

#### `tuple`的陷阱  
因为不可变的特点，在定义的时候，`tuple`的元素就必须被确定下来  
**即：`tuple`在声明的时候就要被初始化**  
```python
>>> t = (1, 2)
>>> t
(1, 2)
```

如果要定义一个空的`tuple`，可以写成`()`：
```python
>>> t = ()
>>> t
()
```

但是，要定义一个只有1个元素的`tuple`，如果这么定义：
```python
>>> t = (1)
>>> t
1
```
注意到定义的**不是`tuple`**，是1这个数，此时`t`是一个数值为1的普通变量  

这是因为括号`()`既可以表示`tuple`，又可以表示数学公式中的小括号，这就产生了歧义  
Python规定，这种情况下，按小括号进行计算，计算结果自然是1

**所以，只有1个元素的`tuple`定义时必须加一个逗号`,`，来消除歧义：**
```python
>>> t = (1,)
>>> t
(1,)
```

> Python在显示只有1个元素的`tuple`时，也会加一个逗号`,`  
> 以免误解成数学计算意义上的括号
