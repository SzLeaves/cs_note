# `Bash`快速指南

### 脚本接收参数
* `$0`              对应的是当前Shell脚本程序的名称
* `$#`              对应的是总共有几个参数
* `$*`              对应的是所有位置的参数值
* `$?`              对应的是显示上一次命令的执行返回值
* `$1、$2、$3...`   分别对应着第N个位置的参数值

exmaple:  
```bash
#!/bin/bash
# 脚本内容：exam.sh
echo "script name: $0"
echo "args number: $#"
echo "args content: $*"
echo "the first arg: $1 , the third arg: $3"
```  
```bash
# 执行脚本
[ZBR107@localhost notes]$ ./exam.sh a b c d e
script name: ./exam.sh
args number: 5
args content: a b c d e
the first arg: a , the third arg: c
```

### 判断语句：`[ expression ]`
> 注意测试语句方括号两边的空格，不能少  
* 文件测试

|选项 |用途                         |表达式返回值                       |
|-----|-----------------------------|-----------------------------------|
| -d  |  测试文件是否为目录类型		|存在且是目录返回0，否则返回非0     |
| -f  |  判断是否为一般文件		    |存在且是一般文件返回0，否则返回非0 |
| -e  |  测试文件是否存在		    |存在返回0，否则返回非0             |
| -r  |  测试当前用户是否有权限读取	|有权限返回0，否则返回非0           |
| -w  |  测试当前用户是否有权限写入	|有权限返回0，否则返回非0           |
| -x  |  测试当前用户是否有权限执行	|有权限返回0，否则返回非0           |

example:  
```bash
[ZBR107@localhost ~]$ [ -e ~/notes/exam.sh ] && echo $?
0
[ZBR107@localhost ~]$ [ -e ~/notes/noexits ] || echo $?
1
```

* 整数值测试  

> 运算符只能用于整数比较                

|选项|用途           |返回值                   |
|----|---------------|-------------------------|
|-eq |是否等于       |符合条件返回0，否则返回1 |
|-ne |是否不等于     |符合条件返回0，否则返回1 |  
|-gt |是否大于       |符合条件返回0，否则返回1 |  
|-ge |是否大于或等于 |符合条件返回0，否则返回1 |
|-lt |是否小于       |符合条件返回0，否则返回1 | 
|-le |是否等于或小于 |符合条件返回0，否则返回1 |


### 逻辑判断符号：`&&  ||  !  =`
* `&&` 连接多个命令，当前面的命令**执行成功**时才会执行后面连接的命令  
```bash
# 后面连接的命令没有执行，没有任何输出
[ZBR107@localhost ~]$ [ -r /root ] && echo "access"
[ZBR107@localhost ~]$
```

* `||` 连接多个命令，当前面的命令**执行失败**时才会执行后面连接的命令  
```bash
[ZBR107@localhost ~]$ [ -r /root ] || echo "no access"
no access
```

* `!` 取反，放置于条件表达式之前  
> `!`符号后面的表达式需用空格隔开  
> 因为bash对“存在”状态取0，所以取反符经常用于上面的条件判断中用于反转判断条件  
```bash
# 对false的表达式取反得到true
[ZBR107@localhost ~]$ [ ! -r /root ] && echo "no access"
no access
```

* `=` 判断两边的表达式是否相等
```bash
# 判断当前shell是否为bash
[ZBR107@localhost notes]$ [ $SHELL = /bin/bash ] && echo $?
0
```

### `bash`变量
使用变量：`$[variable_name]`  
1. bash内置的一些变量  
> 使用`echo $[变量名称]`查看变量的值	
* `HOME` 		当前用户的主目录
* `LOGNAME` 	指定日志记录用户名
* `PWD` 		当前工作目录
* `USERNAME`	当前登录用户名称
* `LANG`		系统当前语言模式

2. 用户自定义变量
格式：`[variable_name]=[variable_value]`  
> 等号两边不要有空格  

example:   
```bash
[ZBR107@localhost notes]$ MYNOTES=~/notes
[ZBR107@localhost notes]$ echo $MYNOTES
/home/ZBR107/notes
```
删除变量：`unset $[variable_name]`


### 使用示例
* 判断当前用户是否为root  
```bash
[ ! $USER = root ] && echo "normal" || echo "root"
```
> 解析：对表达式取反：判断`$USER`是否为普通用户（是返回1，否返回0）  
>
> * 对于普通用户，第一个表达式返回1，然后bash会执行`&&`连接的语句  
（`&&`在前面执行成功后才执行）  
>
> * 对于root用户，第一个表达式返回0，然后bash会执行`||`连接的语句  
（`||`在前面执行失败后才执行，对于`&&`来说，第一个表达式为0，执行失败）

```bash
# 使用示例
[ZBR107@localhost notes]$ [ ! $USER = root ] && echo "normal" || echo "root"
normal
# 切换root用户
[ZBR107@localhost notes]$ su - root
Password: 
[root@localhost ~]# [ ! $USER = root ] && echo "normal" || echo "root"
root
```

> 类似条件判断运算符`?::`的写法：**`[! expression ] && [true] || [false]`**  


### 分支：`if...else`

