# Python学习

## 一、环境搭建及IDE

下载等操作就不讲了

### 1.1 pycharm的使用简介

> 系列内容：
>
> * 代码自动补全
> * 查找功能
> * debug
> * 其他功能
>   * virtualenv管理
>   * todo
>   * 代码块缩进
>   * 定位功能
>   * PEP8规范
>   * 变量标记
>   * 创建包
>   * 多页面同时展示
>   * 版本控制
>   * 支持markdown

安装：

> 社区版、专业版

### 1.2Python简介

为什么要学Python这样的语言？

> 胶水语言为了辅助，有时候比如想写个电子邮件客户端，没必要从最底层写网络协议相关的代码吧。高级编程语言通常都会提供一个比较完善的基础代码库，让你能直接调用，比如，针对电子邮件协议的SMTP库，针对桌面环境的GUI库，在这些已有的代码库的基础上开发，一个电子邮件客户端几天就能开发出来。

## 二、第一个Python程序

### 2.1使用文本编辑器

windows自带的txt编辑器会自动加上 一串字符（保证UTF-8）这就会导致意想不到的事情，所以一般咱们用Notepad等。

> 在当前目录下用NotePad写一个：
>
> > print('hello py')
>
> #保存成 first.py;然后cmd运行：
>
> > > ```shell
> > > C:\Users\IEUser>python hello.py
> > > python: can't open file 'hello.py': [Errno 2] No such file or directory
> >  > ```
>
> #**直接运行py文件**
>
> > windows上不能直接运行，但是linux和mac上可以，方法是在.py文件第一行加上一句：
> >
> > ```shell
> > #!/user/bin/env python3
> > ```
> >
> > 然后通过命令给first.py以执行权限
> >
> > ```shell
> > chmod a+x first.py
> > ```
>
> 其他就是用IDE了，比如PyCharm 或者Jupiter等

### 2.2输入输出

输出：

> ```python
> print('hello')//字符串单引用
> print('the quick brown fox','jump over')//打印的时候逗号会变成空格
> print(100)
> print(100+200)
> print('100+200=',100+200)
> ```

输入：

> ```python
>  name = input()
> print('hello',name);
> ##改进
> name = input('请输入名字')；
> print('hello',name)
> ```

小例子：

```python
name = int(input('a num:'));#input的是字符串，
print('absnum:',abs(name));
```



## 三、Python基础

语法上用缩进当作代码块，#作为注释，

### 3.1数据类型和变量

数据类型：

* 整数	int

  1、0、100000000、0xff00、0x11223344

* 浮点数  float

  1.23e9	12.3e8	1.2e-5

* 字符串 str

  'I\\'m lau'	"I'm lau"	'''...多行...'''	r'''...'''(r默认不转义)

* 布尔值 bool

  none

* 空值

变量：

```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)#ABC

```

常量：

> 全部大写表示常量
>
> ```python
> PI = 3.14
> ```
>
> /表示浮点除法
>
> //整除

### 3.2字符串和编码

#### 字符编码：

ASCII编码

> 适合英语的情况

Unicode编码

> 把所有语言统一到一套编码里

如果统一成Unicode编码，乱码问题从此消失了。但是，如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

UTF-8编码

> 所以，本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的`UTF-8`编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间：

| 字符 | ASCII    | Unicode           | UTF-8                      |
| :--- | :------- | :---------------- | :------------------------- |
| A    | 01000001 | 00000000 01000001 | 01000001                   |
| 中   | x        | 01001110 00101101 | 11100100 10111000 10101101 |

搞清楚三种编码方式的区别后看下计算机系统通用的字符编码工作方式：

> 在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。
>
> 用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件
>
> 浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器

#### Python的字符串：

是Unicode编码的，支持多语言。

> 对于单个字符的编码，Python提供了`ord()`函数获取字符的整数表示，`chr()`函数把编码转换为对应的字符：
>
> ```
> >>> ord('A')
> 65
> >>> ord('中')
> 20013
> >>> chr(66)
> 'B'
> >>> chr(25991)
> '文'
> ```

Python的字符串类型是str，内存中以Unicode表示，一个字符对应若干字节。 如果要在网上传输或者保存到磁盘就需要转换成以字节为单位的bytes；b'ABC'表示。

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
    
    #纯英文的str可以用ASCII编码为bytes，内容是一样的，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。
    
```

如果`bytes`中包含无法解码的字节，`decode()`方法会报错：

```python
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果`bytes`中只有一小部分无效的字节，可以传入==errors='ignore'==忽略错误的字节：

```
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

要计算`str`包含多少个字符，可以用`len()`函数：

```
>>> len('ABC')
3
>>> len('中文')
2
```

`len()`函数计算的是`str`的字符数，如果换成`bytes`，`len()`函数就计算字节数：

```
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

可见，1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。

#### 格式化：

| 占位符 |   替换内容   |
| :----: | :----------: |
|   %d   |     整数     |
|   %f   |    浮点数    |
|   %s   |    字符串    |
|   %x   | 十六进制整数 |

格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

```python
print('%2d-%02d' % (3, 1))#
print('%5.2f' % 3.1415926)#一共5位，小数点后2位
```

#### format():

```python
>>>'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```

eg:

```python
# -*- coding: utf-8 -*-

s1 = 72

s2 = 85

r = (s2-s1)/s1*100

print('%s的成绩从去年的%d提升到了今年的%d，提升了%.1f%%' % ('小明',s1,s2,r))
## %%=>'%'
```

```python
# -*- coding: utf-8 -*-

first_score=72

second_score=85

ratio=(second_score-first_score)/first_score*100

print('hello,{0},你去年考了{1},今年考了{2},成绩提升了{3:.1f}%'.format('小明',72,85,ratio))
```

### 3.3使用list和tuple

#### list列表[]:

##### 关键点：

* 二维‘数组’：

  > ```python
  > spa = ['abc','def','ght']
  > print(spa[0][1])#b
  > ```

* 负数下标：

  > spa[-1] 是‘ght’

* 切片：

* 能修改：

  > \+ \= append() 	insert()	pop()	del()

* 多重赋值：

  > ```python
  > cat = ['fat', 'black','loud']
  > size,color,balabala = cat 
  > ```
  >
  > 

##### 基本表示：

```python
>>>classmates = ['Michael', 'Bob', 'Tracy']
>>> len(classmates)
3
```

##### 访问：

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
    #当索引超出了范围时，Python会报一个IndexError错误，所以，要确保索引不要越界，记得最后一个元素的索引是len(classmates) - 1
```

##### 追加append：

```python
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

##### 插入insert：

```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

##### 删除pop：

```python
>>> classmates.pop()####
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
>>> classmates.pop(1)####
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

##### 替换=：

```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

##### 内部类型可以不同

```python
>>> L = ['Apple', 123, True]
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4

>>>a = [[1,2,3],[4,5,6]]#可以表示多维数组
>>>print(a[0][2])
3
```

#### tuple元组（）：

元组和列表非常类似，但是一旦初始化就不能修改。

##### 基本表示：

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')
```

空的元组：t = ();

但是一个元素的元组 t = (1)中的t是1这个数！！！（规定的）要表示一个元素的元组应该这样： t = (1,)

##### 访问：

​		`classmates[0]`，`classmates[-1]`

##### 元组不能修改：

元组没有append、insert函数，不能修改；但是！！！:

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
##但是这里为什么能够修改元组呢？
```

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向`'a'`，就不能改成指向`'b'`，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

### 3.4条件判断

#### if :

注意 ==缩进== 和 ==冒号==

```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```

`if`语句执行有个特点，它是从上往下判断，如果在某个判断上是`True`，把该判断对应的语句执行后，就忽略掉剩下的

```python
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')
#################
teenager
```

#### 再议input:

注意input进来的是str类型！！！

```python
##出错！！！！
birth = input('birth: ')
if birth < 2000:
    print('00前')
else:
    print('00后')
```

```python
s = input('birth: ')
birth = int(s)
```

### 3.5循环

#### for...in...

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

#### while(条件判断)：

```python
n = 1
while n <= 100:
    print(n)
    n = n + 1
print('END')
```

#### break、continue

和c差不多

### 3.6使用dict和set

#### dict {字典map}

**==键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行==**

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}#value 也可以是字符串
>>> d['Michael']#访问方式
95
```

##### 判断key在不在 key in dict

```python
>>> 'Thomas' in d
False

#二是通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

##### 删除key：pop(key)

```python
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}

##
del dict['Name']  # 删除键是'Name'的条目
dict.clear()      # 清空字典所有条目
del dict          # 删除字典
```

##### **keys()\values()\items()**

它们返回类似列表的值，代表 键、值、键值对；不能被修改，没有append

```python
def main():
    dict = {'a':1,'b':2,'c':3}
    for i in dict.values():
        print(i)#1/2/3
     
    list = dict.values()
    #list = list(dict.values())这样就转成普通list了
    list.append()#也是不行！！因为list指向就是dict.values()
    
```

##### get()方法

> ```python
> dict.get('a',0)#有a就返回a对应的值，没有就返回0
> ```

##### ==setdefault()方法==

实现如下功能：

```python
span = {'name':'Pooka','age':5}
if 'color' not in span:
    spam['color'] = 'black'
```

案例：

```python
def main():
   message = "dadasdaffwbvdahjsagfufguiS"
   count = {}
   for i in message:
       count.setdefault(i,0)#给没有出现过的i设置字典 i：0
       count[i]+=1
   print(count)
```







[菜鸟教程-dict](https://www.runoob.com/python/python-dictionary.html)

#### set()

##### 表示方式

集合（set）是一个无序的不重复元素序列。

可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

创建格式：

```python
parame = {value01,value02,...}
或者
set(value)
```

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。试试把list放入set，看看是否会报错。

##### 添加元素

###### s.add( x )

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.add("Facebook")
>>> print(thisset)
{'Taobao', 'Facebook', 'Google', 'Runoob'}
```

###### s.update(x)

还有一个方法，也可以添加元素，且参数可以是列表，元组，字典等，语法格式如下：

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.update({1,3})#{}表示一个个元素
>>> print(thisset)
{1, 3, 'Google', 'Taobao', 'Runoob'}
>>> thisset.update([1,4],[5,6])  
>>> print(thisset)
{1, 3, 4, 5, 6, 'Google', 'Taobao', 'Runoob'}
>>>
```

##### 移除元素

###### s.remove( x )

元素不存在就报错

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.remove("Taobao")
>>> print(thisset)
{'Google', 'Runoob'}
>>> thisset.remove("Facebook")   # 不存在会发生错误
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Facebook'
>>>
```

###### s.discard( x )

如果元素不存在，不会发生错误。

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.discard("Facebook")  # 不存在不会发生错误
>>> print(thisset)
{'Taobao', 'Google', 'Runoob'}
```

###### s.pop()

##### 计算元素个数len(s)

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> len(thisset)
3
```

##### 清空集合s.clear()

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> thisset.clear()
>>> print(thisset)
set()
```

##### 判断元素在不在x in s

```python
>>>thisset = set(("Google", "Runoob", "Taobao"))
>>> "Runoob" in thisset
True
>>> "Facebook" in thisset
False
>>>
```

[菜鸟-set](https://www.runoob.com/python3/python3-set.html)

#### 再议不可变对象

List 可变：这里a这个变量变化了

```python
>>> a = ['c', 'b', 'a']
>>> a.sort()
>>> a
['a', 'b', 'c']
```

str 不可变：注意！！

```python
>>> a = 'abc'
>>> a.replace('a', 'A')
'Abc'
>>> a
'abc'
```

应该这样理解：

```python
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
'Abc'
>>> a
'abc'
```

要始终牢记的是，`a`是变量，而`'abc'`才是字符串对象！有些时候，我们经常说，对象`a`的内容是`'abc'`，但其实是指，`a`本身是一个变量，它指向的对象的内容才是`'abc'`：

```ascii
┌───┐                  ┌───────┐
│ a │─────────────────>│ 'abc' │
└───┘                  └───────┘
```

当我们调用`a.replace('a', 'A')`时，实际上调用方法`replace`是作用在字符串对象`'abc'`上的，而这个方法虽然名字叫`replace`，但却没有改变字符串`'abc'`的内容。相反，`replace`方法创建了一个新字符串`'Abc'`并返回，如果我们用变量`b`指向该新字符串，就容易理解了，变量`a`仍指向原有的字符串`'abc'`，但变量`b`却指向新字符串`'Abc'`了：

```ascii
┌───┐                  ┌───────┐
│ a │─────────────────>│ 'abc' │
└───┘                  └───────┘
┌───┐                  ┌───────┐
│ b │─────────────────>│ 'Abc' │
└───┘                  └───────┘
```

所以，对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容。相反，这些方法会创建新的对象并返回，这样，就保证了不可变对象本身永远是不可变的。

## 四、函数

### 4.1调用函数

内置函数：abs()	max()	等等

类型转换：int(exp)	float(exp) 等等

### 4.2定义函数

```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

当传入了不恰当的参数时，内置函数`abs`会检查出参数错误，而我们定义的`my_abs`没有参数检查，会导致`if`语句出错，出错信息和`abs`不一样。所以，这个函数定义不够完善。

```python
>>> my_abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in my_abs
TypeError: unorderable types: str() >= int()
>>> abs('A')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: bad operand type for abs(): 'str'
```

#### 参数类型检查:

让我们修改一下`my_abs`的定义，对参数类型做检查，只允许整数和浮点数类型的参数。数据类型检查可以用内置函数`isinstance()`实现：

```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```

#### 返回多个值：

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny
```

```python
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0
```

但其实这只是一种假象，Python函数返回的仍然是单一值，原来返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。

```python
>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

### 4.3 函数的参数

常规的参数也就和c一样；

#### 默认参数：

```python
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

==定义默认参数要牢记一点：默认参数必须指向不变对象！==

#### 可变参数：

参数多的情况可以用list或者tuple传进来。

```python
>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84
```

定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个`*`号。在函数内部，==参数`numbers`接收到的是一个tuple==，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数：

```python
def calc(*numbers)://抽象参数，可以是很多个
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

```python
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```

#### 关键字参数：

可变参数允许你传入0个或**任意个参数**，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或**任意个==含参数名的==参数**，这些关键字参数在函数内部自动组装为一个dict。

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

```python
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

关键字参数有什么用？**它可以扩展函数的功能**。比如，在`person`函数里，我们保证能接收到`name`和`age`这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。

和可变参数类似，也可以先组装出一个dict，然后，把该dict转换为关键字参数传进去：

```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, city=extra['city'], job=extra['job'])
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

`**extra`表示把`extra`这个dict的所有key-value用关键字参数传入到函数的`**kw`参数，`kw`将获得一个dict，注意`kw`获得的dict是`extra`的一份拷贝，对`kw`的改动不会影响到函数外的`extra`。

#### 命名关键字参数：

函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过`kw`检查。  

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

### 4.4 异常处理

```python
def spam(divideBy):
    try:
        return 42/divideBy
    except ZeroDivisionError:
        print('Error:invalid input number')
```











#### ==这里的部分需要在菜鸟补充学习==

# 第二章 控制流

## 基本的东西

### bool

True	False

### 比较操作符

==	!=	<	<=	>	>=	返回bool值

注意的： =	与 ==

### 布尔操作符

and	or	not

### 关于range（）

for i in range(5)	[0-5)

for i in range(5,10)	[5-10)

for i in range(5,10,2)	[5,7,9]

for i in range(5,-1,-1)	[5,4,3,2,1,0]

### 导入模块

import sys

​				sys.exit()

### 传递引用

一句话就是：a=b 是指向同一个东西

```python
def eggs(something):
    something.append('hello world')
def main():
    spa = [-1,8,5,3,7]
    print(spa)
    eggs(spa)
    print(spa)
```





## 使用数据结构对真实世界建模

### 井字棋盘

用一个字典来模拟棋盘：

这里看pdf吧



