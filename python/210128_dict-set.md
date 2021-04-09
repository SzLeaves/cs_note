# 210128. 使用`dict`和`set`

## `dict`
Python内置了字典：`dict`的支持，`dict`全称dictionary  
``**使用键-值映射（key-value）存储，具有极快的查找速度**

例：假设要根据同学的名字查找对应的成绩，如果用`list`实现，需要两个`list`：
```python
names = ['Michael', 'Bob', 'Tracy']
scores = [95, 75, 85]
```
给定一个名字，要查找对应的成绩  
就先要在names中找到对应的位置，再从scores取出对应的成绩  
> `list`越长，查找元素的耗时越长

如果用`dict`实现，只需要一个`name - score`的对照表，直接根据名字查找成绩  
无论这个表有多大，查找速度都不会变慢

#### 创建`dict`
用Python写一个`dict`如下：

> **格式：`dict = { key1: value1, key2: value2, ...}`**  
> 最常用的`key`是字符串

**使用`d[key]`来直接访问`key`所对应的`value`**
```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95
```

#### 添加元素
给一个`dict`添加元素，除了初始化时指定外，还可以通过`key`放入：
```python
>>> d['Adam'] = 67
>>> d
{'Michael': 95, 'Bob': 75, 'Tracy': 85, 'Adam': 67}
```
> 注意：这种方式给`dict`添加元素的前提是**这个`dict`已经声明并存在**  
如果`dict`没有被声明创建，就会报错：
> ````python
> >>> s['name'] = 'steven'
> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
> NameError: name 's' is not defined
> ````

如果`key`不存在，`dict`会报错：
```python
>>> d['Thomas']
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
`key`Error: 'Thomas'
```

要避免`key`不存在的错误，有两种办法  
* 一是通过`in`判断`key`是否存在：
```python
>>> 'Thomas' in d
False
```

* 二是通过`dict`提供的`get()`方法  
如果`key`不存在，可以返回`None`，或者自己指定的`value`：
```python
# key不存在时默认返回None
>>> d.get('Thomas')

# key不存在时返回-1
>>> d.get('Thomas', -1)
-1
```
> 注意：返回`None`的时候，**在Python的终端交互环境下不会显示结果**

#### 删除元素
要删除一个`key`，用`pop(key)`方法，对应的`value`也会从`dict`中删除：
```python
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}
```
> **注意**：`dict`内部存放的顺序和`key`放入的顺序是没有关系的


#### `dict`的内部原理
`dict`本质上是一张`key`-`value`的键-值映射表  
它可以直接通过查找`key`来找出对应的`value`
这样的方式类似于书籍中的目录查找

 > 假设字典包含了1万个汉字，要查某一个字，一个办法是把字典从第一页往后翻  
直到找到我们想要的字为止，这种方法就是在`list`中查找元素的方法  
所以`list`越大，查找越慢  
> 
 > 第二种方法是先在字典的**目录索引表**里查这个字对应的页码  
 然后直接翻到该页，找到这个字。无论找哪个字，这种查找速度都非常快  
 不会随着字典大小的增加而变慢

`dict`就是第二种实现方式，给定一个`key`，python可以直接在内部找出对于的`value`  

所以这种存储方式，在放进去的时候，**必须根据`key`算出`value`的存放位置**  
取的时候才能根据`key`直接拿到`value`

和`list`比较，`dict`有以下几个特点：
* 查找和插入的速度极快，不会随着`key`的增加而变慢
* 需要占用大量的内存，内存浪费多

 而`list`相反：
* 查找和插入的时间随着元素的增加而增加
* 占用空间小，浪费内存很少

> `dict`是用空间来换取时间的一种方法


#### 注意事项

* <u>**`dict`的`key`必须是不可变对象**</u>  

因为`dict`根据`key`来计算`value`的存储位置  
如果每次计算相同的`key`得出的结果不同，那`dict`内部就完全混乱了  
> 这个通过`key`计算位置的算法称为哈希算法（Hash）

**要保证hash的正确性，作为`key`的对象就不能变**  
在Python中，字符串、整数等都是不可变的，因此可以作为`key`  
**而`list`是可变的，就不能作为`key`：**
```python
>>> key = [1, 2, 3]
>>> d[key] = 'a list'
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```
> `tuple`作为不可变对象，是可以作为`key`的
> ````python
> >>> d = {(1, 2): 'a, b'}
> >>> d
> {(1, 2): 'a, b'}
> ````

* <u>**一个`key`只能对应一个`value`**</u>  

多次对一个`key`放入`value`，**后面的值会把前面的值替换掉：**
```python
>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88
```
> `dict`中不存在重复的`key`  
**放入相同的`key`只会把原有映射关系中对应的`value`替换掉**


##  `set`
`set`和`dict`类似，也是一组`key`的集合，**但不存储`value`**  
> 和`dict`一样，在`set`中没有重复的`key`

#### 创建`set`
要创建一个`set`，**需要提供一个<u>`iterable`</u>对象作为输入集合：**
```python
# 放入list
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}

# 放入tuple
>>> s = set((1, 2, 3))
>>> s
{1, 2, 3}

# 放入str
>>> s = set('abcd')
>>> s
{'b', 'd', 'c', 'a'}
```

> 格式：`set_list = set( [iterable] )`

如果没有放入`iterable`对象，会报错：
```python
>>> s = set(1, 2, 3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: set expected at most 1 argument, got 3
```
> 注意到报错的参数`set expected at most 1 argument`  
这也说明`set()`只能传入一个参数
>

注意：传入的参数`[1, 2, 3]`是一个`list`  
而显示的`{1, 2, 3}`只是说明这个`set`内部有`1，2，3`这3个元素  
**显示的顺序也不表示`set`是有序的**

**重复元素在`set`中自动被过滤：**
```python
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
```

`set`可以看成数学意义上的**无序和无重复元素**的集合  
因此，两个`set`可以做数学意义上的交集、并集等操作：
```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])

# 取交集
>>> s1 & s2
{2, 3}

# 取并集
>>> s1 | s2
{1, 2, 3, 4}
```

#### 添加元素
**通过`add(key)`方法可以添加元素到`set`中**  
可以添加`set`中已有的元素，但不会有效果：
```python
>>> s = set([1, 2, 3])

>>> s.add(4)
>>> s
{1, 2, 3, 4}

# 添加重复元素
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

#### 删除元素
**通过`remove(key)`方法可以删除元素：**
```python
>>> s.remove(4)
>>> s
{1, 2, 3}
```

`set`和`dict`的唯一区别仅在于没有存储对应的`value`  
`set`的原理和`dict`一样，所以同样**不可以放入可变对象**  
```python
>>> s = set([1, 2, [3, 4]])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```
> 因为无法判断两个可变对象是否相等，也就无法保证`set`内部“不会有重复元素”


## “不可变对象”
对于可变对象，比如`list`，对`list`进行操作，`list`内部的内容是会变化的：  
```python
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

而对于不可变对象，比如`str`：
```python
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'

# 注意a的值
>>> a
'abc'
```

虽然字符串有个`replace()`方法，也确实“变”出了'Abc'  
但变量a最后仍是'abc'

因为`replace()`方法并没有真正改变字符串的内容  
**在调用它的时候，它会创建一个新的字符串`'Abc'`并返回**  
显然，这个新字符串与原来的变量`a`是没有任何关系的  

把代码改成下面这样就直观多了：
```python
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
'Abc'
>>> a
'abc'
```
可以看到`replace()`返回了一个新字符串`'Abc'`， 并赋给了`b`，但对于`a`并没有影响  

示意图：
```
# 调用方法前
┌───┐            ┌───────┐
│ a │───────────>│ 'abc' │
└───┘            └───────┘
```
```
# 调用方法后
┌───┐            ┌───────┐
│ a │───────────>│ 'abc' │
└───┘            └───────┘
┌───┐            ┌───────┐
│ b │───────────>│ 'Abc' │
└───┘            └───────┘
```

如果代码改成下面这样，把`replace()`的返回值赋给`a`
```python
>>> a = a.replace('a', 'A')
>>> a
'Abc'
```
这样也只是改变了`a`的指向，现在它指向新的字符串`Abc`  
这个新字符串与原来的字符串`abc`也是没有关系的
> 当然，如果原来的字符串值没有了任何指向它的变量，就会被内存回收

> 对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容  
相反，这些方法会**创建新的对象并返回**  
这样就保证了不可变对象本身永远是不可变的

## 小结
* 使用`key`-`value`存储结构的`dict`在Python中非常有用  
* 选择不可变对象作为`key`很重要  
* **最常用的`key`是字符串**
