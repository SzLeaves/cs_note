# 210128.控制结构

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
> * **`if`和`else`后面不要少写了冒号`:`**
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

**`range()`可以作为迭代器（`iterator`）直接放入`for x in ...`循环中：**
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
在使用`for x in...`结构时一定要记住，`in...`需要的是一个*迭代器对象*  
比如`list`,`tuple`,`range(n, m)`


## 小结
* **在所有的控制结构语句中都不要少了缩进**  
**在控制语句开头不要少了冒号`:`**

* `break`语句可以在循环过程中直接退出循环  
而`continue`语句可以提前结束本轮循环，并直接开始下一轮循环  
这两个语句通常都必须配合`if`语句使用

* 注意不要滥用`break`和`continue`语句  
`break`和`continue`会造成代码执行逻辑分叉过多，容易出错
