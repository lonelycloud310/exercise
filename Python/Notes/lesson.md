[toc]
### 知识补充
#### 递归函数
在函数内部，可以调用其他函数。如果一个函数在内部调用自身本身，这个函数就是递归函数。**递归一般用分支语句实现，一个概念定义中用到了这个概念本身，这就叫递归， 要编写递归函数道先设置基例(出口)，再按照规律写出合理的链条。**。
举个例子，我们来计算阶乘<font color = 'red'>$n! = 1 \times 2 \times 3 \times \cdots \times n$</font>，用函数<font color = 'red'>fact(n)</font>表示，可以看出:$fact(n) = n! = 1 \times 2 \times 3 \times \cdots \times (n-1) \times n = (n-1)! \times n = fact(n-1) \times n$所以，<font color = 'red'> fact(n) </font>可以表示为<font color = 'red'>$n \times fact(n-1)$</font>，只有n=1时需要特殊处理。比如:
```python
def fact(n):
    if n <= 1:
        return 1
    return n * fact(n-1)

>>> fact(1)
1
>>> fact(5)
120
```

递归的数据共享情况：递归每一层间的数据是独立的，不共享，每一层的n的值不同，不会互相影响，可以理解成调用别的同名同功能函数。来看下一段求阶乘的代码：如果我们计算<font color ='red'>fact(5)</font>，函数计算过程如下：
```python
<Go>
====>n = 5 return n * fact(n-1) = 5 * fact(4)
====>n = 4 return n * fact(n-1) = 4 * fact(3)
====>n = 3 return n * fact(n-1) = 3 * fact(2)
====>n = 2 return n * fact(n-1) = 2 * fact(1)
====>n = 1 
<return> 1
====>n = 2 return n * fact(n-1) = 2 * 1 #fact(n-1) = 1返回值为 2
====>n = 3 return n * fact(n-1) = 3 * 2 * 1 #fact(n-1) = 2 返回值为6
====>n = 4 return n * fact(n-1) = 4 * 3 * 2 * 1#fact(n-1) = 6 返回值为24
====>n = 5 return n * fact(n-1) = 5 * 4 * 3 * 2 * 1
5 * 4 * 3 * 2 * 1 = 120
```

首先“递归”包括两个过程：递“去”的过程，“归”回的过程！先从一个简单的递归函数讲起
```python
def di_gui(n):
    print(n, "<===1====>")
    if n > 0:
        di_gui(n - 1)
    print(n, '<===2====>')

di_gui(5) # 外部调用后打印的结果是？
<go>
5 <===1====>               n = 5                 
4 <===1====>               n = 4
3 <===1====>               n = 3
2 <===1====>               n = 2
1 <===1====>               n = 1
0 <===1====>               n = 0
return nore
0 <===2====>               n = 2                 
1 <===2====>               n = 1
2 <===2====>               n = 2
3 <===2====>               n = 3
4 <===2====>               n = 4
5 <===2====>               n = 5
```

递归的执行过程：首先，递归会执行“去”的过程，只要不触发终止条件，就会一直在函数内，带着更新的参数，调用函数自身，注意：到内部调用函数， 最外层的代码不会被执行，__而是暂停阻塞__；此时 随着函数每调用一次自身，还没有触发返回值和到达终止条件，等同于在原来的基础上不断“向下/向内”开辟新的内存空间，记住，每次调用一次函数，就不是处在同一空间
什么时候“递”去的过程结束？记住有两种情况>>> __一是：当前这层空间函数全部执行结束（终止条件），二是：执行到了return 返回值，直接返回；__
试着解析下：下面这段代码的执行流程
```python
def get_num(num):
    if num > 2:
        get_num(num - 1)
    print(num)

get_num(4) # 输出结果为：2 3 4
```
代码变化一下
```python
def get_num(num):
    if num > 2:
        get_num(num - 1)
    else:
        print(num)


get_num(4) # 输出结果为 2
```

解析一下：加了else后，首先代码区有两个分支，
在num>2时，执行会有递归，当n=4 是开辟一层空间；
n=3时开辟一层空间，此时 get_num(2) 再开辟一个空间，
当它从上往下执行过程中，在他本层空间遇到if num>2 不成立，所以走分支 else，直接打印出来；
此时代码还没结束，<font color ='red'>return none</font> 返回到上一层空间，num=3, num>2 num=4 已经进入了if 不会走else，所以没有值输出！

在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一次函数调用，栈就会加一层栈帧，每当函数返回，栈就会逐一减少栈帧。由于栈的大小不是无限的，所以，递归 必须要有一个出口，如果没有出口就会重复调用，不断的开辟栈帧空间，递归调用的次数过多，会导致栈溢出。

#### 布尔表达式
**在一个<font color='red'>表达式</font>中，所有的空iterable、空字符串''、0、False和{}都计算为False**。有一些空格字符串，在被剥离时，会被缩减为空字符串，这些字符串的计算结果也是False

在布尔表达式<font color='red'>and</font>中，从左到右演算表达式的值，如果表达式中的所有值都为<font color='red'>真</font>，那么<font color='red'>and</font>返回最后一个真值，如果布尔上下文中的某个值为<font color='red'>假</font>，则 and 返回第一个<font color='red'>假值</font>。

<font color='red'>or</font>的返回值与and相反

#### 函数方法
##### enumerate()函数
Python 内置函数
enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在<font color='red'>for</font>循环当中。
**语法**
enumerate(sequence, [start=0])
**参数**
sequence -- 一个序列、迭代器或其他支持迭代对象。
start -- 下标起始位置，默认为0
**返回值**
返回 enumerate(枚举) 对象。
**实例**
```python
>>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))      
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```
for循环使用enumerate
```python
>>>seq = ['one', 'two', 'three']
>>> for i, element in enumerate(seq):
...     print i, element
... 
0 one
1 two
2 three
```

##### 字典 values() 方法
Python3 字典
<font color ='red'>values()</font> 方法返回一个视图对象。
<font color ='red'>dict.keys()、dict.values() 和 dict.items()</font> 返回的都是视图对象(view objects)，提供了字典实体的动态视图，这就意味着**字典改变，视图也会跟着变化。**
视图对象不是列表，不支持索引，可以使用 list() 来转换为列表。不能对视图对象进行任何的修改，因为字典的视图对象都是只读的。
**语法
dict.values()
**参数**
NA
**返回值**
返回视图对象
**实例**
```python
>>> dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}
>>> keys = dishes.keys()
>>> values = dishes.values()

>>> # 迭代
>>> n = 0
>>> for val in values:
...     n += val
>>> print(n)
504

>>> # keys 和 values 以相同顺序（插入顺序）进行迭代
>>> list(keys)     # 使用 list() 转换为列表
['eggs', 'sausage', 'bacon', 'spam']
>>> list(values)
[2, 1, 1, 500]

>>> # 视图对象是动态的，受字典变化的影响，以下删除了字典的 key，视图对象转为列表后也跟着变化
>>> del dishes['eggs']
>>> del dishes['sausage']
>>> list(values)
[1, 500]
>>> #相同两个 dict.values() 比较返回都是 False
>>> d = {'a': 1}
>>> d.values() == d.values()
False
```

##### os.listdir()
Python OS 文件/目录方法
<font color='red'>os.listdir()</font> 方法用于返回指定的文件夹包含的文件或文件夹的名字的列表。
它不包括 . 和 .. 即使它在文件夹中。
只支持在 Unix, Windows 下使用。
**语法**
<font color='red'>os.listdir(path)</font>
**参数**
<font color='red'>path</font> -- 需要列出的目录路径
**返回值**
返回指定路径下的文件和文件夹**列表**。
**实例**
```python
import os, sys

# 打开文件
path = "E:/py"
dirs = os.listdir(path)

# 输出所有文件和文件夹
for file in dirs:
   print (file)
```
##### 字符串join()方法
<font color='red'>join()</font> 方法用于将**序列中的元素**以指定的字符连接生成一个新的字符串。
**语法**
<font color='red'>str.join(sequence)</font>
**参数**
<font color='red'>sequence</font> -- 要连接的元素序列。
**返回值**
返回通过指定字符连接序列中元素后生成的**新字符串**。
**实例**
```python
>>> s = ['my','name','is','bob'] 
>>> ' '.join(s) 
'my name is bob' 
>>> '..'.join(s) 
'my..name..is..bob'
``` 

##### 函数iter()
Python 内置函数
**描述**
<font color='red'>iter()</font> 函数用来生成迭代器。
**语法**
<font color='red'>iter(object[, sentinel])</font>
**参数**
<font color='red'>object</font> -- 支持迭代的集合对象。
<font color='red'>sentinel</font> -- 如果传递了第二个参数，则参数 object 必须是一个可调用的对象（如，函数），此时，iter 创建了一个迭代器对象，每次调用这个迭代器对象的__next__()方法时，都会调用 object。
**返回值**
**迭代器对象**。
实例
```python
>>> isinstance('a, b, c', Iterator)
False
>>> isinstance(iter('a, b, c'), Iterator)
True
>>>
```
##### 字符串split()方法
<font color='red'>split()</font> 通过指定分隔符对字符串进行切片，如果参数 num 有指定值，则分隔 num+1 个子字符串
**语法**
<font color='red'>str.split(str="", num=string.count(str))</font>.
**参数**
<font color='red'>str</font> -- 分隔符，默认为所有的空字符，包括空格、换行(\n)、制表符(\t)等。
<font color='red'>num</font> -- 分割次数。默认为 -1, 即分隔所有。
**返回值**
**返回分割后的字符串列表**。

实例
```python
>>> a,b,c = input("请输入三角形三边的长：").split()
请输入三角形三边的长：3 4 5
>>> a
'3'
>>> b
'4'
>>> c
'5'

>>>str1 = "Line1-abcdef \nLine2-abc \nLine4-abcd"
>>> print(str1.split())
['Line1-abcdef', 'Line2-abc', 'Line4-abcd']
>>> print(str1.split(' ', 1))
['Line1-abcdef', '\nLine2-abc \nLine4-abcd']
```
**函数eval()**
Python 内置函数
eval() 函数用来执行一个字符串表达式，并返回表达式的值。<font color='red'>去掉最外层的引号</font>
**语法**
<font color='red'>eval(expression[, globals[, locals]])</font>
**参数**
<font color='red'>expression</font> -- 字符串表达式。
<font color='red'>globals</font> -- 变量作用域，全局命名空间，如果被提供，则必须是一个字典对象。
<font color='red'>locals</font> -- 变量作用域，局部命名空间，如果被提供，可以是任何映射对象。
返回值
返回表达式计算结果。
实例
```python
>>>x = 7
>>> eval( '3 * x' )
21
>>> eval('pow(2,2)')
4
>>> print(eval("{'name':'linux','age':age}",{"age":1822}))
{'name': 'linux', 'age': 1822}
age=18
print(eval("{'name':'linux','age':age}",{"age":1822},locals()))
{'name': 'linux', 'age': 18}
```

#### pyathon运算符优先级
以下表格列出了从最**高**到最低优先级的所有运算符：
<table><tbody>
    <tr>
        <td bgcolor="#7FFFD4">运算符</td><td bgcolor="#7FFFD4">运算符</td>
    </tr>
    <tr>
       <td bgcolor="#F0F8FF">**</td><td bgcolor="#F0F8FF">指数 (最高优先级)</td>
    </tr>
    <tr>
        <td bgcolor="#FFF8DC">~ + -</td><td bgcolor="#FFF8DC">按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@) &emsp; &emsp;&emsp;&emsp;&emsp;</td>
    </tr>
    <tr>
        <td bgcolor="#F0F8FF">* / % //</td><td bgcolor="#F0F8FF">乘，除，求余数和取整除</td>
    </tr>
        <tr>
        <td bgcolor="#FFF8DC">+ -</td><td bgcolor="#FFF8DC">加法减法</td>
    </tr>
        <tr>
        <td bgcolor="#F0F8FF">>> <<</td><td bgcolor="#F0F8FF">	右移，左移运算符</td>
    </tr>
        <tr>
        <td bgcolor="#FFF8DC">&</td><td bgcolor="#FFF8DC">位 'AND'</td>
    </tr>
        <tr>
        <td bgcolor="#F0F8FF">^ |</td><td bgcolor="#F0F8FF">位运算符</td>
    </tr>
        <tr>
        <td bgcolor="#FFF8DC"><= < > >=</td><td bgcolor="#FFF8DC">比较运算符</td>
    </tr>
        <tr>
        <td bgcolor="#F0F8FF">== !=</td><td bgcolor="#F0F8FF">等于运算符</td>
    </tr>
        <tr>
        <td bgcolor="#FFF8DC">= %= /= //= -= += *= **=/td><td bgcolor="#FFF8DC">赋值运算符</td>
    </tr>
        <tr>
        <td bgcolor="#F0F8FF">is is not</td><td bgcolor="#F0F8FF">身份运算符</td>
    </tr>
    </tr>
        <tr>
        <td bgcolor="#FFF8DC">in not in</td><td bgcolor="#FFF8DC">成员运算符</td>
    </tr>
    </tr>
        <tr>
        <td bgcolor="#F0F8FF">not and or</td><td bgcolor="#F0F8FF">逻辑运算符and有最高优先级</td>
    </tr>
</table>
举例
```python
>>> e = (a + b) * (c / d)    # (30) * (15/5)
>>> print ("(a + b) * (c / d) 运算结果为：",  e)
(a + b) * (c / d) 运算结果为： 90.0
```
### 高级特性
#### 切片
一个完整的切片表达式包含两个<font color='red'>“:”</font>，用于分隔三个参数<font color ='red'>(start、stop、step)</font>。当只有一个<font color = 'red'>“:”</font>时，默认第三个参数step=1；当一个“:”也没有时，start_index=end_index，表示切取start_index指定的那个元素。

<font color = 'red'>step</font>：正负数均可，**其绝对值大小决定了切取数据时的‘‘步长”**，**而正负号决定了“切取方向”**，正表示“从左往右”取值，负表示“从右往左”取值</。当step省略时，默认为1，即从左往右以步长1取值。“切取方向非常重要！”“切取方向非常重要！”

<font color = 'red'>start</font>：表示开始取值（**包含该索引对应值**）；该参数省略时，表示从对象“端点”开始取值，至于是从“起点”还是从“终点”开始，则由step参数的正负决定，step为正从“起点”开始，为负从“终点”开始。

<font color = 'red'>stop</font>：表示终止取值（**不包含该索引对应值**）；该参数省略时，表示一直取到数据“端点(包括)”，至于是到“起点”还是到“终点”，同样由step参数的正负决定，step为正时直到“终点”，为负时直到“起点”。

Python切片操作详细例子
```python
>>>a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>>a[1:-6]
>>> [1, 2, 3]
step=1，从左到右从第二个元素开始到位置-6结束(不包括-6)
>>>a[-1:-6]
>>> []
step=1，从左到右从最后一个元素开始到位置-6结束，取值方向与step方向矛盾所以为这空
>>> a[-1:-6:-1]
[9, 8, 7, 6, 5]
step=-1，从右到左从最后一个元素开始到位置-6结束，(不包括-6)
```
多层切片:
```python
>>>a[:8][2:5][-1:]
>>> [4]
相当于：
a[:8]=[0, 1, 2, 3, 4, 5, 6, 7]
a[:8][2:5]= [2, 3, 4]
a[:8][2:5][-1:] = [4]
理论上可无限次多层切片操作，只要上一次返回的是非空可切片对象即可。
```
修改单个元素
```python
>>>a[3] = ['A','B']
[0, 1, 2, ['A', 'B'], 4, 5, 6, 7, 8, 9]
```
在某个位置插入元素
```python
>>>a[3:3] = ['A','B','C']
[0, 1, 2, 'A', 'B', 'C', 3, 4, 5, 6, 7, 8, 9]
>>>a[0:0] = ['A','B']
['A', 'B', 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
替换一部分元素
```python
>>>a[3:6] = ['A','B']
[0, 1, 2, 'A', 'B', 6, 7, 8, 9]
```

#### 迭代

如果给定一个<font color ='red'>list</font>或<font color ='red'>tuple</font>，我们可以通过<font color ='red'>for</font>循环来遍历这个list或tuple，这种遍历我们称为迭代（Iteration）。
Python的<font color ='red'>for</font>循环不仅可以用在list或tuple上，还可以作用在其他可迭代对象上。list这种数据类型虽然有下标，但很多其他数据类型是没有下标的，但是，只要是可迭代对象，无论有无下标，都可以迭代，比如<font color ='red'>dict</font>就可以迭代：
```python
>>> d = {'a': 1, 'b': 2, 'c': 3}
>>> for key in d:
...     print(key)
...
a
b
c
```
因为<font color ='red'>dict</font>的存储不是按照<font color ='red'>list</font>的方式顺序排列，所以，迭代出的结果顺序很可能不一样。
默认情况下，<font color ='red'>dict迭代的是key</font>。如果要迭代value，可以用<font color ='red'>for value in d.values()</font>，如果要同时迭代key和value，可以用<font color ='red'>for k, v in d.items()</font>。

由于字符串也是可迭代对象，因此，也可以作用于for循环：
```python
>>> for ch in 'ABC':
...     print(ch)
...
A
B
C
```
所以，当我们使用for循环时，只要作用于一个可迭代对象，for循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。

那么，如何判断一个对象是可迭代对象呢？方法是通过<font color ='red'>collections.abc</font>模块的<font color ='red'>Iterable</font>类型判断：
```python
>>> from collections.abc import Iterable
>>> isinstance('abc', Iterable)   # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable)     # 整数是否可迭代
False
```
如果要对list实现类似Java那样的下标循环怎么办？Python内置的<font color ='red'>enumerate</font>函数可以把一个list变成索引-元素对，这样就可以在<font color ='red'>for</font>循环中同时迭代索引和元素本身：
```python
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```
上面的<font color='red'>for</font>循环里，同时引用了两个变量，在Python里是很常见的，比如下面的代码：
```python
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9
```

#### 列表生成式
列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。
举个例子，要生成list [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]可以用list(range(1, 11))：
```python
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
但如果要生成[1x1, 2x2, 3x3, ..., 10x10]怎么做？方法一是循环：
```python
>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
...
>>> L
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
但是循环太繁琐，而列表生成式则可以用一行语句代替循环生成上面的list：
```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
写列表生成式时，把要生成的元素<font color='red'>x * x</font>放到前面，后面跟<font color='red'>for</font>循环，就可以把list创建出来，十分有用，多写几次，很快就可以熟悉这种语法。
for循环后面还可以加上if判断，这样我们就可以筛选出仅偶数的平方：
```python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```
还可以使用两层循环，可以生成全排列：
```python
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```
for循环其实可以同时使用两个甚至多个变量，比如dict的items()可以同时迭代key和value：
```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> for k, v in d.items():
...     print(k, '=', v)
...
y = B
x = A
z = C
```
因此，列表生成式也可以使用两个变量来生成list：
```python
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```
最后把一个list中所有的字符串变成小写：
```python
>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```
if ... else
使用列表生成式的时候，有些童鞋经常搞不清楚if...else的用法。例如，以下代码正常输出偶数：
```python
>>> [x for x in range(1, 11) if x % 2 == 0]
[2, 4, 6, 8, 10]
```
但是，我们不能在最后的if加上else：<font color='red'>在for后面的if是一个筛选条件（只显示满足条件的元素)，不能带else，
if写在for前面**必须加**else且要有结果</font>，这是因为for前面的部分是一个表达式，它必须根据x计算出一个结果。
```python
>>> [x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]
```
上述for前面的表达式x if x % 2 == 0 else -x才能根据x计算出确定的结果。
可见，在一个列表生成式中，for前面的if ... else是表达式，而for后面的if是过滤条件，不能带else。

#### 生成器
通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的<font color='red'>[]</font>改成<font color='red'>()</font>，就创建了一个<font color='red'>generator</font>：
```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```
我们可以直接打印出list的每一个元素，但我们怎么打印出generator的每一个元素呢？
如果要一个一个打印出来，可以通过<font color='red'>next()</font>函数获得generator的下一个返回值：
```python
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
36
>>> next(g)
49
>>> next(g)
64
>>> next(g)
81
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
我们讲过，generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。
当然，上面这种不断调用next(g)实在是太变态了，正确的方法是使用for循环，因为generator也是可迭代对象：
```python
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81
```
所以，我们创建了一个generator后，基本上永远不会调用next()，而是通过<font color='red'>for</font>循环来迭代它，并且不需要关心StopIteration的错误。

generator非常强大。如果推算的算法比较复杂，用类似列表生成式的<font color='red'>for</font>循环无法实现的时候，还可以用<font color='red'>函数</font>来实现。
比如，著名的斐波拉契数列（Fibonacci），除第一个和第二个数外，任意一个数都可由前两个数相加得到：
1, 1, 2, 3, 5, 8, 13, 21, 34, ...
斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很容易：
```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
注意，赋值语句：

a, b = b, a + b
相当于：
t = (b, a + b) # t是一个tuple
a = t[0]
b = t[1]
```
a, b = b, a+b 这个表达式的意思就是说，**先计算=号的右边b的值，a+b的值，算好了，然后再分别赋值给左边的a和b**
上面的函数可以输出斐波那契数列的前N个数：
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
仔细观察，可以看出，fib函数实际上是定义了斐波拉契数列的推算规则，可以从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似generator。
也就是说，上面的函数和generator仅一步之遥。要把fib函数变成generator，只需要把print(b)改为<font color='red'>yield</font> b就可以了：
```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```
这就是定义generator的另一种方法。如果一个函数定义中包含<font color='red'>yield</font>关键字，那么这个函数就不再是一个普通函数，而是一个<font color='red'>generator</font>:
```python
>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>
>>> next(fib(11))
1
>>> next(fib(11))
1
>>> next(fib(11))
1
```
这里，最难理解的就是<font color='red'>generator</font>和函数的执行流程不一样。函数是<font color='red'>顺序执行</font>，遇到<font color='red'>return</font>语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用<font color='red'>next()</font>的时候执行，遇到<font color='red'>yield</font>语句返回，并返回一个<font color='red'>迭代值</font>，再次执行时从**<font color='red'>yield</font>的下一个语句继续执行**
举个简单的例子，定义一个generator，依次返回数字1，3，5：
```python
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield(3)
    print('step 3')
    yield(5)
```
调用该generator时，**首先要生成一个generator对象**，然后用<font color='red'>next()</font>函数不断获得下一个返回值：
```python
>>> o = odd()
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
回到fib的例子，我们在循环过程中不断调用<font color='red'>yield</font>，就会不断中断。当然要给循环设置一个条件来退出循环，不然就会产生一个无限数列出来。
同样的，把函数改成generator后，我们基本上从来不会用next()来获取下一个返回值，而是直接使用<font color='red'>for</font>循环来迭代：
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
但是用<font color='red'>for</font>循环调用generator时，发现拿不到generator的<font color='red'>return</font>语句的返回值。如果想要拿到返回值，必须捕获<font color='red'>StopIteration</font>错误，返回值包含在<font color='red'>StopIteration</font>的<font color='red'>value</font>中：
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

#### 迭代器
我们已经知道，可以直接作用于for循环的数据类型有以下几种：

一类是集合数据类型，如list、tuple、dict、set、str等；
一类是<font color='red'>generator</font>，包括生成器和带<font color='red'>yield</font>的<font color='red'>generator function</font>。

这些可以直接作用于for循环的对象统称为可迭代对象：<font color='red'>Iterable</font>。
可以使用isinstance()判断一个对象是否是Iterable对象：
```python
>>> from collections.abc import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```
而生成器不但可以作用于<font color='red'>for</font>循环，还可以被<font color='red'>next()</font>函数不断调用并返回下一个值，直到最后抛出StopIteration错误表示无法继续返回下一个值了。
可以被<font color='red'>next()</font>函数调用并不断返回下一个值的对象称为迭代器：<font color='red'>Iterator</font>。
可以使用isinstance()判断一个对象是否是Iterator对象：
```python
>>> from collections.abc import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```
生成器都是<font color='red'>Iterator</font>对象，但list、dict、str虽然是Iterable，却不是Iterator。
把list、dict、str等Iterable变成Iterator可以使用iter()函数：
```python
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```
为什么list、dict、str等数据类型不是Iterator？这是因为Python的Iterator对象表示的是一个数据流，<font color='red'>Iterator</font>对象可以被<font color='red'>next()</font>函数调用并不断返回下一个数据，直到没有数据时抛出<font color='red'>StopIteration</font>错误。可以把这个数据流看做是一个**有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据**，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。
**Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。**
小结
凡是可作用于<font color='red'>for</font>循环的对象都是<font color='red'>Iterable</font>类型；
凡是可作用于<font color='red'>next()</font>函数的对象都是<font color='red'>Iterator</font>类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过<font color='red'>iter()</font>函数获得一个Iterator对象。
Python的for循环本质上就是通过不断调用next()函数实现的，例如：
```python
for x in [1, 2, 3, 4, 5]:
    pass

实际上完全等价于：

# 首先获得Iterator对象:
it = iter([1, 2, 3, 4, 5])
# 循环:
while True:
    try:
        # 获得下一个值:
        x = next(it)
    except StopIteration:
        # 遇到StopIteration就退出循环
        break
```

### 函数式编程
函数是Python内建支持的一种封装，我们通过把大段代码拆成函数，通过一层一层的函数调用，就可以把复杂任务分解成简单的任务，这种分解可以称之为面向过程的程序设计。函数就是面向过程的程序设计的基本单元。

而函数式编程（请注意多了一个“式”字）——Functional Programming，虽然也可以归结到面向过程的程序设计，但其思想更接近数学计算。

我们首先要搞明白计算机（Computer）和计算（Compute）的概念。

在计算机的层次上，CPU执行的是加减乘除的指令代码，以及各种条件判断和跳转指令，所以，汇编语言是最贴近计算机的语言。而计算则指数学意义上的计算，越是抽象的计算，离计算机硬件越远。

对应到编程语言，就是越低级的语言，越贴近计算机，抽象程度低，执行效率高，比如C语言；越高级的语言，越贴近计算，抽象程度高，执行效率低，比如Lisp语言。

**函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数我们称之为没有副作用。而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数**！

Python对函数式编程提供部分支持。由于Python允许使用变量，因此，Python不是纯函数式编程语言。

#### 高阶函数
高阶函数英文叫Higher-order function。什么是高阶函数？我们以实际代码为例子，一步一步深入概念。

变量可以指向函数
以Python内置的求绝对值的函数abs()为例，调用该函数用以下代码：
```python
>>> abs(-10)
10
```
但是，如果把函数本身赋值给变量呢？
```python
>>> f = abs
>>> f
<built-in function abs>
>>> f = abs
>>> f(-10)
10
```
说明变量f现在已经指向了abs函数本身。直接调用<font color='red'>abs()</font>函数和调用变量<font color='red'>f()</font>完全相同
函数名也是变量
那么函数名是什么呢？函数名其实就是指向函数的变量！对于abs()这个函数，完全可以把函数名abs看成变量，它指向一个可以计算绝对值的函数！
如果把abs指向其他对象，会有什么情况发生？
```python
>>> abs = 10
>>> abs(-10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```
**把abs指向10后，就无法通过abs(-10)调用该函数了！因为abs这个变量已经不指向求绝对值函数而是指向一个整数10！**
当然实际代码绝对不能这么写，这里是为了说明函数名也是变量。要恢复abs函数，请重启Python交互环境。
注：由于abs函数实际上是定义在<font color='red'>import builtins</font>模块中的，所以要让修改abs变量的指向在其它模块也生效，要用<font color='red'>import builtins; builtins.abs = 10</font>。
传入函数
既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

一个最简单的高阶函数：
```python
f = abs
def add(x, y, f):
    return f(x) + f(y)
```
当我们调用<font color='red'>add(-5, 6, abs)</font>时，参数<font color='red'>x，y和f</font>分别接收<font color='red'>-5，6和abs</font>。
##### map/reduce
Python内建函数<font color='red'>map()</font>和<font color='red'>reduce()</font>
我们先看map。<font color='red'>map()</font>函数接收两个参数，一个是<font color='red'>函数</font>，一个是<font color='red'>Iterable</font>，**map将传入的函数依次作用到可迭代对象的每个元素**，并把结果作为新的<font color='red'>Iterator</font>返回。

举例说明，比如我们有一个函数$f(x)=x^2$，要把这个函数作用在一个list [1, 2, 3, 4, 5, 6, 7, 8, 9]上，就可以用<font color='red'>map()</font>实现如下：
```python
>>>def f(x):
...return x * x
...
>>>r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>>list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
<font color='red'>map()</font>传入的第一个参数是<font color='red'>f</font>，即函数对象本身。由于结果r是一个<font color='red'>Iterator</font>，Iterator是惰性序列，因此通过<font color='red'>list()</font>函数让它把整个序列都计算出来并返回一个<font color='red'>list</font>。
map()作为高阶函数，事实上它把运算规则抽象了，因此，我们不但可以计算简单的f(x)=x2，还可以计算任意复杂的函数，比如，把这个list所有数字转为字符串：
```python
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```
再看<font color='red'>reduce</font>的用法。<font color='red'>reduce</font>**把一个函数作用在一个可迭代对象<font color='red'>$[x_1, x_2, x_3, ...]$</font>上**，这个函数必须接收**两个参数**，reduce把结果继续和序列的下一个元素做累积计算，其效果就是：
```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
比方说对一个序列求和，就可以用<font color='red'>reduce</font>实现：
```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```
要把序列[1, 3, 5, 7, 9]变换成整数13579
```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```
对上面的例子稍加改动，配合<font color='red'>map()</font>，我们就可以写出把<font color='red'>str</font>转换为<font color='red'>int</font>的函数：
```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579
```
整理成一个<font color='red'>str2int</font>的函数就是：
```python
from functools import reduce
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))
```    
还可以用<font color='red'>lambda</font>函数进一步简化成：
```python
from functools import reduce
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def char2num(s):
    return DIGITS[s]

def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```
也就是说，假设Python没有提供int()函数，你完全可以自己写一个把字符串转化为整数的函数，而且只需要几行代码！

**chr() 函数**
Python 内置函数
chr() 用一个范围在 range（256）内的（就是0～255）整数作参数，返回一个对应的<font color='red'>ASCII字符</font>。
**语法**
以下是 chr() 方法的语法:
<font color='red'>chr(i)</font>
**参数**
i -- 可以是10进制也可以是16进制的形式的数字。
**返回值**
返回值是当前整数对应的 <font color='red'>ASCII 字符</font>。
实例
```python
>>>print chr(0x30), chr(0x31), chr(0x61)   # 十六进制
0 1 a
>>> print chr(48), chr(49), chr(97)         # 十进制
0 1 a
```
##### filter函数
Python内建的<font color='red'>filter()</font>函数用于过滤序列。
和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是<font color='red'>True</font>还是<font color='red'>False</font>最后将返回<font color='red'>True</font>的元素放到新<font color='red'>Iterator</font>中

例如，在一个list中，删掉偶数，只保留奇数，可以这么写：
```python
def is_odd(n):
    return n % 2 == 1
list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```
把一个序列中的空字符串删掉，可以这么写：
```python
def not_empty(s):
    return s and s.strip()
list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
# 结果: ['A', 'B', 'C']
```

可见用<font color='red'>filter()</font>这个高阶函数，关键在于正确实现一个“筛选”函数。函数返回的是一个<font color='red'>Iterator</font>，也就是一个惰性序列，所以要强迫filter()完成计算结果，需要用list()函数获得所有结果并返回list。

用filter求素数
计算素数的一个方法是埃氏筛法，它的算法理解起来非常简单：
首先，列出从<font color='red'>2</font>开始的所有**自然数**，构造一个序列：
2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...
取序列的第一个数<font color='red'>2</font>，它一定是素数，然后**用2把序列的2的倍数筛掉**：
3, ~~4~~, 5, ~~6~~, 7, ~~8~~, 9, ~~10~~, 11, ~~12~~, 13, ~~14~~, 15, ~~16~~, 17, ~~18~~, 19...
取新序列的第一个数<font color='red'>3</font>，它一定是素数，然后**用3把序列的3的倍数筛掉**：
5, 7, 8, ~~9~~, 10, 11, ~~12~~, 13, 14, ~~15~~, 16, 17, ~~18~~, 19...
取新序列的第一个数<font color='red'>5</font>，然后用5把序列的5的倍数筛掉：
7, 11, 13, 14, ~~15~~, 16, 17, 19, ~~25~~ ...
不断筛下去，就可以得到所有的素数。
用Python来实现这个算法，可以先构造一个从3开始的奇数序列：
```python
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n
```
注意这是一个<font color='red'>生成器</font>，并且是一个无限序列。然后定义一个筛选函数：
```python
def _not_divisible(n):
    return lambda x: x % n > 0
```
最后，定义一个生成器，不断返回下一个素数：
```python
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
```
这个生成器先返回第一个素数2，然后，利用filter()不断产生筛选后的新的序列。

由于primes()也是一个无限序列，所以调用时需要设置一个退出循环的条件：
```python
打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```
注意到Iterator是惰性计算的序列，所以我们可以用Python表示“全体自然数”，“全体素数”这样的序列，而代码非常简洁。

**字符串strip()**
<font color='red'>strip()</font>方法用于移除字符串头尾指定的字符（默认为空格）或字符序列。
注意：该方法只能删除开头或是结尾的字符，不能删除中间部分的字符。
**语法**
<font color='red'>str.strip([chars])</font>
**参数**
<font color='red'>chars</font> -- 移除字符串头尾指定的字符序列。
**返回值**
返回移除字符串头尾指定的字符序列生成的新**字符串**。
实例 
```python
str1 = "*****this is **string** example....wow!!!*****"
print(str.strip('*'))  # 指定字符串 
this is **string** example....wow!!!
 
str1 = "123abcrunoob321"
print (str.strip( '12' ))  # 字符序列为 12
3abcrunoob3
```
##### sorted
排序算法
排序也是在程序中经常用到的算法。无论使用冒泡排序还是快速排序，排序的核心是比较两个元素的大小。如果是数字，我们可以直接比较，但如果是字符串或者两个dict呢？直接比较数学上的大小是没有意义的，因此，比较的过程必须通过函数抽象出来。
Python内置的<font color='red'>sorted()</font>函数就可以对<font color='red'>Iterable</font>进行排序：
```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```
此外，<font color='red'>sorted()</font>函数也是一个高阶函数，它还可以接收一个<font color='red'>key函数</font>来实现自定义的排序，例如按绝对值大小排序：
```pyton
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```
**key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序**。
我们再看一个字符串排序的例子：
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```
默认情况下，对字符串排序，是按照ASCII的大小比较的，由于<font color='red'>'Z' < 'a'</font>，结果，大写字母Z会排在小写字母a的前面。
现在，我们提出排序应该忽略大小写，按照字母序排序。要实现这个算法，不必对现有代码大加改动，只要我们能用一个<font color='red'>key函数</font>把字符串映射为忽略大小写排序即可。忽略大小写来比较两个字符串，实际上就是先把字符串都变成大写（或者都变成小写），再比较。
这样，我们给<font color='red'>sorted</font>传入<font color='red'>key函数</font>，即可实现忽略大小写的排序：
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']
```
要进行反向排序，不必改动key函数，可以传入第三个参数<font color='red'>reverse=True</font>：
```python
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```
从上述例子可以看出，高阶函数的抽象能力是非常强大的，而且，核心代码可以保持得非常简洁。
#### 返回函数
函数作为**返回值**
高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。
我们来实现一个可变参数的求和。通常情况下，求和的函数是这样定义的：
```python
def calc_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax
```
但是，如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎么办？**可以不返回求和的结果，而是返回求和的函数**：
```python
def lazy_sum(*args):
    def sum1():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum1
```
当我们调用<font color='red'>lazy_sum()</font>时，返回的并**不是求和结果**，而是**求和函数**：
```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum1 at 0x101c6ed90>
```
调用函数f时，才真正计算求和的结果：
```python
>>> f()
25
```
在这个例子中，我们在函数<font color='red'>lazy_sum</font>中又定义了函数<font color='red'>sum1</font>，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量，当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为<font color='red'>“闭包（Closure）</font>”
请再注意一点，当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数：
```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1 == f2
False
```
**f1()和f2()的调用结果互不影响**。

**闭包**
注意到返回的函数在其定义内部引用了**外函数的局部变量<font color='red'>args</font>**，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。
如果在一个函数的内部定义了另一个函数，外部的函数叫它外函数，内部的函数叫它内函数。

**闭包条件**
在一个外函数中定义了一个内函数。
**内函数里运用了外函数的临时变量**。
**并且外函数的返回值是内函数的引用**。

一般情况下，如果一个函数结束，函数的内部所有东西都会释放掉，还给内存，局部变量都会消失。但是闭包是一种特殊情况，如果外函数在结束的时候发现有自己的临时变量将来会在内部函数中用到，就把这个临时变量绑定给了内部函数，然后自己再结束。

另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了<font color='red'>f()</font>才执行。我们来看一个例子：
```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```
在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回了。你可能认为调用f1()，f2()和f3()结果应该是<font color='red'>1，4，9</font>，但实际结果是：
```python
>>> f1()
9
>>> f2()
9
>>> f3()
9
```
全部都是9！原因就在于返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了3，因此最终结果为9。

 返回闭包时牢记一点：**循环体内定义的函数是无法保存循环执行过程中的不停变化的外部变量的，即普通函数无法保存运行环境！**
如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```
再看看结果：
```python
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```

#### 匿名函数
当我们在传入函数时，有些时候，不需要显式地定义函数，直接传入匿名函数更方便。
在Python中，对匿名函数提供了有限支持。还是以map()函数为例，计算f(x)=x2时，除了定义一个f(x)的函数外，还可以直接传入匿名函数：

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

通过对比可以看出，匿名函数<font color='red'>lambda x: x * x</font>实际上就是：

```python
def f(x):
    return x * x
```

关键字<font color='red'>lambda</font>表示匿名函数，冒号前面的<font color='red'>x</font>表示**函数参数**。匿名函数有个限制，就是**只能有一个表达式**，返回值是**函数名**。

匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

#### 装饰器
简言之，python装饰器就是用于**拓展原来函数功能**的一种函数，这个函数的特殊之处在于它的返回值也是一个函数，**使用python装饰器的好处就是在不用更改原函数的代码前提下给函数增加新的功能**。
一般而言，我们要想拓展原来函数代码，最直接的办法就是侵入代码里面修改，例如：
```python
import time
def func():
    print("hello")
    time.sleep(1)
    print("world")
```
这是我们最原始的的一个函数，然后我们试图记录下这个函数执行的总时间，那最简单的做法就是：
```python
#原始侵入，篡改原函数
import time
def func():
    startTime = time.time()
    print("hello")
    time.sleep(1)
    print("world")
    endTime = time.time()
    msecs = (endTime - startTime)*1000
    print("time is %d ms" % msecs)
```
假设我们要增强<font color='red'>func()</font>函数的功能，**但又不希望修改func()函数的定义**，这种在代码运行期间动态增加功能的方式，称之为<font color='red'>“装饰器”（Decorator）</font>。
```python
import time

def deco(func):
    def wrapper():
        startTime = time.time()
        func()
        endTime = time.time()
        msecs = (endTime - startTime)*1000
        print("time is %d ms" %msecs)
    return wrapper

@deco
def func():
    print("hello")
    time.sleep(1)
    print("world")

if __name__ == '__main__':
    f = func #这里f被赋值为func，执行f()就是执行func()
    f()
```
把<font color='red'>@deco</font>放到<font color='red'>func()</font>函数的定义处，相当于执行了语句：
```python
func = deco(func)
```
如果需拓展的函数有很多可是有参数怎么办呢：
```python
#带有不定参数的装饰器
import time

def deco(func):
    def wrapper(*args, **kw):
        startTime = time.time()
        func(*args, **kwargs)
        endTime = time.time()
        msecs = (endTime - startTime)*1000
        print("time is %d ms" %msecs)
    return wrapper

@deco
def func(a,b):
    print("hello，here is a func for add :")
    time.sleep(1)
    print("result is %d" %(a+b))

@deco
def func2(a,b,c):
    print("hello，here is a func for add :")
    time.sleep(1)
    print("result is %d" %(a+b+c))

if __name__ == '__main__':
    f = func
    func2(3,4,5)
    f(3,4)
```
wrapper()函数的参数定义是<font color='red'>(*args, **kw)</font>，因此，wrapper()函数可以接受任意参数的调用。
如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数，写出来会更复杂。比如，要自定义log的文本：
```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```
这个3层嵌套的decorator用法如下：
```python
@log('execute')
def now():
    print('2015-3-25')

>>> now()
execute now():
2015-3-25
```
和两层嵌套的decorator相比，3层嵌套的效果是这样的：
```python
>>> now = log('execute')(now)
```
我们来剖析上面的语句，首先执行<font color='red'>log('execute')</font>，返回的是<font color='red'>decorator函数</font>，再调用返回的函数，参数是<font color='red'>now函数</font>，返回值最终是<font color='red'>wrapper函数</font>。

以上两种decorator的定义都没有问题，但还差最后一步。因为我们讲了函数也是对象，它有__name__等属性，但你去看经过decorator装饰之后的函数，它们的__name__已经从原来的<font color='red'>'now'</font>变成了<font color='red'>'wrapper'</font>：
```python
>>> now.__name__
'wrapper'
```
因为返回的那个wrapper()函数名字就是'wrapper'，所以，需要把原始函数的__name__等属性复制到wrapper()函数中，**否则，有些依赖函数签名的代码执行就会出错**。
不需要编写XXX__name__ = func.__name__这样的代码，Python内置的<font color='red'>functools.wraps</font>就是干这个事的，所以，一个完整的decorator的写法如下：
```python
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```
import functools是导入functools模块。只需记住在定义<font color='red'>wrapper()</font>的前面加上<font color='red'>@functools.wraps(func)</font>即可。

#### 偏函数
偏函数
Python的<font color='red'>functools</font>模块提供了很多有用的功能，其中一个就是偏函数<font color='red'>（Partial function）</font>。要注意，这里的偏函数和数学意义上的偏函数不一样。
在介绍函数参数的时候，我们讲到，通过设定参数的默认值，可以降低函数调用的难度。而偏函数也可以做到这一点。举例如下：

<font color='red'>int()</font>函数可以把字符串转换为整数，当仅传入字符串时，<font color='red'>int()</font>函数默认按十进制转换：
```python
>>> int('12345')
12345
```
但int()函数还提供额外的<font color='red'>base</font>参数，默认值为<font color='red'>10</font>。如果传入<font color='red'>base</font>参数，就可以做N进制的转换：
```python
>>> int('12345', base=8)
5349
>>> int('12345', 16)
74565
```
假设要转换大量的二进制字符串，每次都传入int(x, base=2)非常麻烦，于是，我们想到，可以定义一个int2()的函数，默认把base=2传进去：
```python
def int2(x, base=2):
    return int(x, base)

>>> int2('1000000')
64
>>> int2('1010101')
85
```
<font color='red'>functools.partial</font>就是帮助我们创建一个偏函数的，不需要我们自己定义int2()，可以直接使用下面的代码创建一个新的函数int2：
```python
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
```
所以，简单总结<font color='red'>functools.partial</font>的作用就是，**把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数**，调用这个新函数会更简单。

注意到上面的新的<font color='red'>int2</font>函数，仅仅是把base参数重新设定默认值为<font color='red'>2</font>，但也可以在函数调用时传入其他值：
```pyton
>>> int2('1000000', base=10)
1000000
```
最后，创建偏函数时，实际上可以接收<font color='red'>函数对象</font>、<font color='red'>*args</font>和<font color='red'>**kw</font>这3个参数，当传入：
```python
int2 = functools.partial(int, base=2)
```
实际上固定了<font color='red'>int()</font>函数的关键字参数<font color='red'>base</font>，也就是：
```python
int2('10010')
相当于：

kw = { 'base': 2 }
int('10010', **kw)
```
当传入：
```python
max2 = functools.partial(max, 10)
```
实际上会把10作为<font color='red'>*args</font>的一部分自动加到左边，也就是：
```python
max2(5, 6, 7)
相当于：

args = (10, 5, 6, 7)
max(*args)
```
结果为<font color='red'>10</font>。

小结
当函数的参数个数太多，需要简化时，使用<font color='red'>functools.partial</font>可以创建一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用时更简单。

### 模块
在计算机程序的开发过程中，随着程序代码越写越多，在一个文件里代码就会越来越长，越来越不容易维护。
为了编写可维护的代码，我们把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。在Python中，一个<font color='red'>.py</font>文件就称之为一个模块<font color='red'>（Module）</font>。

使用模块有什么好处？
最大的好处是大大提高了代码的可维护性。其次，编写代码不必从零开始。当一个模块编写完毕，就可以被其他地方引用。我们在编写程序的时候，也经常引用其他模块，包括Python内置的模块和来自第三方的模块。

使用模块还可以避免函数名和变量名冲突。**相同名字的函数和变量完全可以分别存在不同的模块中**，因此，我们自己在编写模块时，不必考虑名字会与其他模块冲突。但是也要注意，**尽量不要与内置函数名字冲突**。

你也许还想到，如果不同的人编写的模块名相同怎么办？为了避免模块名冲突，Python又引入了按<font color='red'>目录</font>来组织模块的方法，称为包<font color='red'>（Package）</font>。

举个例子，一个<font color='red'>abc.py</font>的文件就是一个名字叫<font color='red'>abc</font>的模块，一个<font color='red'>xyz.py</font>的文件就是一个名字叫xyz的模块。

现在，假设我们的abc和xyz这两个模块名字与其他模块冲突了，于是我们可以**通过包来组织模块，避免冲突**。方法是选择一个顶层包名，比如<font color='red'>mycompany</font>，按照如下目录存放：
```python
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```
引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，<font color='red'>abc.py</font>模块的名字就变成了<font color='red'>mycompany.abc</font>，类似的，<font color='red'>xyz.py</font>的模块名变成了<font color='red'>mycompany.xyz</font>。

注意，每一个包目录下面都会有一个<font color='red'>\_\_init__.py</font>的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。<font color='red'>\_\_init__.py</font>可以是空文件，也可以有Python代码，因为<font color='red'>\_\_init__.py</font>本身就是一个模块，而它的模块名就是<font color='red'>mycompany</font>。

类似的，可以有多级目录，组成多级层次的包结构。比如如下的目录结构：
```python
mycompany
 ├─ web
 │  ├─ __init__.py
 │  ├─ utils.py
 │  └─ www.py
 ├─ __init__.py
 ├─ abc.py
 └─ utils.py
```
文件www.py的模块名就是mycompany.web.www，两个文件utils.py的模块名分别是mycompany.utils和mycompany.web.utils。

自己创建模块时要注意命名，不能和Python自带的模块名称冲突。例如，系统自带了sys模块，自己的模块就不可命名为`sys.py`，否则将无法导入系统自带的sys模块。

总结
模块是一组Python代码的集合，可以使用其他模块，也可以被其他模块使用。

创建自己的模块时，要注意：
**模块名要遵循Python变量命名规范，不要使用数字、特殊字符**；
模块名不要和系统模块名冲突，最好先查看系统是否已存在该模块，检查方法是在Python交互环境执行<font color='red'>import module</font>，若成功则说明系统存在此模块。

#### 使用模块
python本身就内置了很多非常有用的模块，只要安装完毕，这些模块就可以立刻使用。

我们以内建的sys模块为例，编写一个hello的模块：
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-


' a test module '
__author__ = 'Michael Liao'

import sys

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```
第1行和第2行是标准注释，第1行注释可以让这个hello.py文件直接在Unix/Linux/Mac上运行，第2行注释表示.py文件本身使用标准UTF-8编码；第4行是一个字符串，表示模块的文档注释，**任何模块代码的第一个字符串都被视为模块的文档注释**；第6行使用<font color='red'>\_\_author__</font>变量把作者写进去，这样当你公开源代码后别人就可以瞻仰你的大名；

以上就是Python模块的标准文件模板，当然也可以全部删掉不写，但是，按标准办事肯定没错。

后面开始就是真正的代码部分，使用sys模块的第一步，就是导入该模块：<font color='red'>import sys</font>导入<font color='red'>sys</font>模块后，我们就有了**变量sys指向该模块，利用sys这个变量，就可以访问sys模块的所有功能**。

sys模块有一个<font color='red'>argv</font>变量，用<font color='red'>list</font>存储了命令行的所有参数。argv至少有一个元素，因为第一个参数永远是**该.py文件的名称和目录**，例如：
运行`python python3 hello.py` 获得的sys.argv就是`['hello.py']`；运行`python3 hello.py Michael`获得的<font color='red'>sys.argv</font>就是`['hello.py', 'Michael']`。
最后，注意到这两行代码：
```python
if __name__=='__main__':
    test()
```
当我们在命令行运行hello模块文件时，Python解释器把一个特殊变量<font color='red'>\_\_name__</font>置为<font color='red'>\_\_main__</font>，而如果在其他地方导入该hello模块时，<font color='red'>if</font>判断将失败，因此，这种<font color='red'>if</font>测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。
我们可以用命令行运行hello.py看看效果：
```python
python hello.py
Hello, world!
python hello.py Michael
Hello, Michael!
```
如果启动Python交互环境，再导入hello模块：
```python
>>> import hello
>>>
```
导入时，没有打印Hello, word!，因为没有执行test()函数。调用hello.test()时，才能打印出Hello, word!：
```python
>>> hello.test()
Hello, world!
```
**作用域**
在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在Python中，是通过<font color ='red' size=3>_</font>前缀来实现的。

正常的函数和变量名是公开的（public），可以被直接引用，比如：<font color ='red'>abc，x123，PI</font>等；

类似<font color ='red'>\_\_xxx__</font>这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的<font color ='red'>\_\_author__</font>，<font color ='red'>\_\_name__</font>就是特殊变量，hello模块定义的文档注释也可以用特殊变量<font color ='red'>\_\_doc__</font>访问，我们自己的变量一般不要用这种变量名；

类似<font color ='red'>_xxx</font>和<font color ='red'>__xxx</font>这样的函数或变量就是非公开的（private），不应该被直接引用，比如<font color ='red'>_abc</font>，<font color ='red'>__abc</font>等；

之所以我们说，private函数和变量“不应该”被直接引用，而不是“不能”被直接引用，是因为Python并没有一种方法可以完全限制访问private函数或变量，但是，从编程习惯上不应该引用private函数或变量。
private函数或变量不应该被别人引用，那它们有什么用呢？请看例子：
```python
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```
我们在模块里公开greeting()函数，而把内部逻辑用private函数隐藏起来了，这样，调用greeting()函数不用关心内部的private函数细节，这也是一种非常有用的代码封装和抽象的方法，即：
**外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public**。

#### 安装第三方模块
在Python中，安装第三方模块，是通过包管理工具<font color='red'>pip</font>完成的。如果你正在使用Mac或Linux，安装pip本身这个步骤就可以跳过了。
如果你正在使用Windows，请参考安装Python一节的内容，确保安装时勾选了<font color='red'>pip</font>和<font color='red'>Add python.exe to Path</font>。
注意：Mac或Linux上有可能并存Python 3.x和Python 2.x，因此对应的pip命令是<font color='red'>pip3</font>。
例如，我们要安装一个第三方库——Python Imaging Library，这是Python下非常强大的处理图像的工具库。不过，PIL目前只支持到Python 2.7，并且有年头没有更新了，因此，基于<font color='red'>PIL</font>的<font color='red'>Pillow</font>项目开发非常活跃，并且支持最新的Python 3。
一般来说，第三方库都会在Python官方的 pypi.python.org 网站注册，要安装一个第三方库，必须先知道该库的名称，可以在官网或者pypi上搜索，比如Pillow的名称叫Pillow，因此，安装Pillow的命令就是：
```python
pip install Pillow
```
在Anaconda中启动<font color='red'>Anaconda prompt</font>选择环境后执行同样的命令
Anaconda常用命令:
查看环境：
**conda env list
conda info -e
conda info --envs**
创建环境：
**conda create -n [name] python=[version]
conda create --name [name] python=[version]**
删除环境：
**conda remove --name [name] --all**
激活环境：
**activate name**
关闭环境：
**deactivate**
打开python解释器：
**python**
Anaconda环境变量**Drive:\Anaconda3\Library\bin;Drive:\Anaconda3\Scripts\;Drive:\Anaconda3\;**

### 面向对象编程
面向对象编程——Object Oriented Programming，简称<font color='red'>OOP</font>，是一种程序设计思想。OOP把对象作为程序的基本单元，**一个对象包含了数据和操作数据的函数**。

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

在Python中，所有数据类型都可以视为对象，当然也可以自定义对象。自定义的对象数据类型就是面向对象中的类（Class）的概念。我们以一个例子来说明面向过程和面向对象在程序流程上的不同之处。

假设我们要处理学生的成绩表，为了表示一个学生的成绩，面向过程的程序可以用一个dict表示：
```python
std1 = { 'name': 'Michael', 'score': 98 }
std2 = { 'name': 'Bob', 'score': 81 }
def print_score(std):
    print('%s: %s' % (std['name'], std['score']))
```
如果采用面向对象的程序设计思想，我们首选思考的不是程序的执行流程，而是Student这种数据类型应该被视为一个对象，这个对象拥有<font color='red'>name</font>和font color='red'>score</font>这两个属性（Property）。如果要打印一个学生的成绩，首先必须创建出这个学生对应的对象，然后，给对象发一个<font color='red'>print_score</font>消息，让对象自己把自己的数据打印出来。
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```
给对象发消息实际上就是调用对象对应的关联函数，我们称之为对象的方法（Method）。面向对象的程序写出来就像这样：
```python
bart = Student('Bart Simpson', 59)
lisa = Student('Lisa Simpson', 87)
bart.print_score()
lisa.print_score()
```
面向对象的设计思想是从自然界中来的，因为在自然界中，类（Class）和实例（Instance）的概念是很自然的。Class是一种抽象概念，比如我们定义的Class——Student，是指学生这个概念，而实例（Instance）则是一个个具体的Student，比如，Bart Simpson和Lisa Simpson是两个具体的Student。
所以，面向对象的设计思想是抽象出Class，根据Class创建Instance。
面向对象的抽象程度又比函数要高，因为一个Class既包含数据，又包含操作数据的方法。

#### 类和实例
面向对象最重要的概念就是<font color='red'>类（Class）</font>和<font color='red'>实例（Instance）</font>，必须牢记类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但各自的数据可能不同。

仍以Student类为例，在Python中，定义类是通过<font color='red'>class</font>关键字：
```python
class Student(object):
    pass
```
<font color='red'>class</font>后面紧接着是类名，即Student，**类名通常是大写开头的单词**，紧接着是<font color='red'>(object)</font>，表示该类是从哪个类继承下来的，继承的概念我们后面再讲，通常，如果没有合适的继承类，就使用object类，这是所有类最终都会继承的类。
定义好了Student类，就可以根据Student类创建出Student的实例，创建实例是通过<font color='red'>类名+()</font>实现的：
```python
>>> bart = Student()
>>> bart
<__main__.Student object at 0x10a67a590>
>>> Student
<class '__main__.Student'>
```
可以看到，**变量bart指向的就是一个<font color='red'>Student</font>的实例**，后面的0x10a67a590是内存地址，每个object的地址都不一样，而<font color='red'>Student</font>本身则是一个类。可以自由地给一个实例变量绑定属性，比如，给实例bart绑定一个name属性：
```python
>>> bart.name = 'Bart Simpson'
>>> bart.name
'Bart Simpson'
```
由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的<font color='red'>\_\_init__</font>方法，在创建实例的时候，就把name，score等属性绑上去：**类中的函数称为方法**<font color='red'>\_\_init__()</font> 是一个特殊的方法，每当你根据类创建新实例时，Python都会自动运行它。
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score
```
注意到<font color='red'>\_\_init__</font>方法的第一个参数永远是<font color='red'>self</font>，**表示创建的实例本身**，因此，在<font color='red'>\_\_init__</font>方法内部，就可以把各种属性绑定到<font color='red'>self</font>，因为self就指向创建的实例本身。有了<font color='red'>\_\_init__</font>方法，在创建实例的时候，就不能传入空的参数了，必须传入与<font color='red'>\_\_init__</font>方法匹配的参数，但self不需要传，Python解释器自己会把实例变量传进去。两个变量都有前缀<font color='red'>self</font> 。**以self 为前缀的变量都可供类中的所有方法使用，我们还可以通过类的任何实例来访问这些变量**：
```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
```
和普通的函数相比，在类中定义的函数只有一点不同，就是**第一个参数永远是实例变量self**，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

**数据封装**
面向对象编程的一个重要特点就是数据封装。在上面的Student类中，每个实例就拥有各自的name和score这些数据。我们可以通过函数来访问这些数据，比如打印一个学生的成绩：
```python
>>> def print_score(std):
...     print('%s: %s' % (std.name, std.score))
...
>>> print_score(bart)
Bart Simpson: 59
```
但是，既然Student实例本身就拥有这些数据，要访问这些数据，就没有必要从外面的函数去访问，可以直接在Student类的内部定义访问数据的函数，这样，就把“数据”给封装起来了。这些封装数据的函数是和Student类本身是关联起来的，我们称之为类的方法：
```pyton
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```
要定义一个方法，除了第一个参数是self外，其他和普通函数一样。要调用一个方法，只需要在实例变量上直接调用，除了self不用传递，其他参数正常传入：
```python
>>> bart.print_score()
Bart Simpson: 59
```
这样一来，我们从外部看Student类，就只需要知道，创建实例需要给出name和score，而如何打印，都是在Student类的内部定义的，这些数据和逻辑被“封装”起来了，调用很容易，但却不用知道内部实现的细节。

封装的另一个好处是可以给Student类增加新的方法，比如get_grade：
```python
class Student(object):
    ...

    def get_grade(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 60:
            return 'B'
        else:
            return 'C'
```
同样的，get_grade方法可以直接在实例变量上调用，不需要知道内部实现细节：

####访问限制
在Class内部，可以有属性和方法，而外部代码可以通过直接调用实例变量的方法来操作数据，这样，就隐藏了内部的复杂逻辑。

但是，从前面Student类的定义来看，外部代码还是可以自由地修改一个实例的name、score属性：
```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.score
59
>>> bart.score = 99
>>> bart.score
99
```
如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线<font color='red'>__</font>，在Python中，实例的变量名如果以<font color='red'>__</font>开头，就变成了一个**私有变量（private）**，只有内部可以访问，外部不能访问，所以，我们把Student类改一改：
```python
class Student(object):

    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```
改完后，对于外部代码来说，没什么变动，但是已经无法从外部访问实例变量<font color='red'>.__name</font>和实例变量<font color='red'>.__score</font>了：
```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.__name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute '__name'
```
这样就确保了外部代码不能随意修改对象内部的状态，这样通过访问限制的保护，代码更加健壮。

但是如果外部代码要获取<font color='red'>name</font>和<font color='red'>score</font>怎么办？可以给Student类增加<font color='red'>get_name</font>和<font color='red'>get_score</font>这样的方法：
```python
class Student(object):
    ...

    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
```
如果又要允许外部代码修改score怎么办？可以再给Student类增加<font color='red'>set_score</font>方法：
```python
class Student(object):
    ...

    def set_score(self, score):
        self.__score = score
``` 
你也许会问，原先那种直接通过bart.score = 99也可以修改啊，为什么要定义一个方法大费周折？因为在方法中，***可以对参数做检查**，避免传入无效的参数：
```python
class Student(object):
    ...

    def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```
需要注意的是，在Python中，变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用__name__、__score__这样的变量名。
有些时候，你会看到以一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。

双下划线开头的实例变量是不是一定不能从外部访问呢？其实也不是。不能直接访问<font color='red'>__name</font>是因为Python解释器对外把__name变量改成了<font color='red'>_Student__name</font>，所以，仍然可以通过<font color='red'>_Student__name</font>来访问__name变量：
```pyton
>>> bart._Student__name
'Bart Simpson'
```
但是强烈建议你不要这么干，**因为不同版本的Python解释器可能会把__name改成不同的变量名**。总的来说就是，Python本身没有任何机制阻止你干坏事，一切全靠自觉。
最后注意下面的这种错误写法：
```python
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'
```
表面上看，外部代码“成功”地设置了__name变量，但实际上这个__name变量和class内部的__name变量不是一个变量！内部的__name变量已经被Python解释器自动改成了<font color='red'>_Student__name</font>，而外部代码给**bart新增了一个__name变量**。不信试试：
```python
>>> bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```

#### 继承和多态
在OOP程序设计中，当我们定义一个class的时候，可以从某个现有的class继承，新的class称为**子类（Subclass）**，而被继承的class称为**基类、父类或超类（Base class、Super class）**。

比如，我们已经编写了一个名为<font color='red'>Animal</font>的class，有一个<font color='red'>run()</font>方法可以直接打印：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
```
当我们需要编写<font color='red'>Dog</font>和<font color='red'>Cat</font>类时，就可以直接从<font color='red'>Animal</font>类继承：
```python
class Dog(Animal):
    pass

class Cat(Animal):
    pass
```
对于<font color='red'>Dog</font>来说，<font color='red'>Animal</font>就是它的父类，对于<font color='red'>Animal</font>来说，<font color='red'>Dog</font>就是它的子类。Cat和Dog类似。继承有什么好处？最大的好处是子类**获得了父类的全部功能**。由于Animial实现了run()方法，因此，Dog和Cat作为它的子类，什么事也没干，就自动拥有了run()方法：
```python
dog = Dog()
dog.run()

cat = Cat()
cat.run()

运行结果如下：

Animal is running...
Animal is running...
```
当然，也可以对子类增加一些方法，比如<font color='red'>Dog</font>类：
```python
class Dog(Animal):

    def run(self):
        print('Dog is running...')

    def eat(self):
        print('Eating meat...')
```
继承的第二个好处需要我们对代码做一点改进。你看到了，无论是<font color='red'>Dog</font>还是<font color='red'>Cat</font>，它们<font color='red'>run()</font>的时候，显示的都是<font color='red'>Animal is running...</font>，符合逻辑的做法是分别显示<font color='red'>Dog is running...</font>和<font color='red'>Cat is running...</font>，因此，对Dog和Cat类改进如下：
```python
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
再次运行，结果如下：

Dog is running...
Cat is running...
```
当子类和父类都存在相同的<font color='red'>run()</font>方法时，我们说，**子类的run()覆盖了父类的run()**，在代码运行的时候，总是会调用子类的<font color='red'>run()</font>。这样，我们就获得了继承的另一个好处：多态。

要理解什么是多态，我们首先要对数据类型再作一点说明。当我们定义一个class的时候，我们实际上就定义了一种数据类型。我们定义的数据类型和Python自带的数据类型，比如str、list、dict没什么两样：
```python
a = list() # a是list类型
b = Animal() # b是Animal类型
c = Dog() # c是Dog类型
```
判断一个变量是否是某个类型可以用<font color='red'>isinstance()</font>判断：
```python
>>> isinstance(a, list)
True
>>> isinstance(b, Animal)
True
>>> isinstance(c, Dog)
True
```
看来a、b、c确实对应着**list、Animal、Dog**这3种类型。但是等等，试试：
```python
>>> isinstance(c, Animal)
True
```
**看来c不仅仅是Dog，c还是Animal**！

不过仔细想想，这是有道理的，因为Dog是从Animal继承下来的，当我们创建了一个Dog的实例c时，我们认为c的数据类型是Dog没错，但c同时也是Animal也没错，Dog本来就是Animal的一种！

**所以，在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。但是，反过来就不行：**
```python
>>> b = Animal()
>>> isinstance(b, Dog)
False
```
Dog可以看成Animal，但Animal不可以看成Dog。

要理解多态的好处，我们还需要再编写一个函数，这个函数接受一个Animal类型的变量：
```python
def run_twice(animal):
    animal.run()
    animal.run()
```
当我们传入<font color='red'>Animal</font>的实例时，<font color='red'>run_twice()</font>就打印出：
```python
>>> run_twice(Animal())
Animal is running...
Animal is running...
```
当我们传入<font color='red'>Dog</font>的实例时，<font color='red'>run_twice()</font>就打印出：
```python
>>> run_twice(Dog())
Dog is running...
Dog is running...
```
当我们传入<font color='red'>Cat</font>的实例时，<font color='red'>run_twice()</font>就打印出：
```python
>>> run_twice(Cat())
Cat is running...
Cat is running...
```
看上去没啥意思，但是仔细想想，现在，如果我们再定义一个<font color='red'>Tortoise</font>类型，也从<font color='red'>Animal</font>派生：
```python
class Tortoise(Animal):
    def run(self):
        print('Tortoise is running slowly...')
```
当我们调用<font color='red'>run_twice()</font>时，传入<font color='red'>Tortoise</font>的实例：
```python
>>> run_twice(Tortoise())
Tortoise is running slowly...
Tortoise is running slowly...
```
你会发现，新增一个Animal的子类，不必对run_twice()做任何修改，实际上，**任何依赖Animal作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态**。

多态的好处就是，当我们需要传入<font color='red'>Dog、Cat、Tortoise……</font>时，我们只需要接收<font color='red'>Animal</font>类型就可以了。因为<font color='red'>Dog、Cat、Tortoise……</font>都是<font color='red'>Animal类型</font>，然后，按照<font color='red'>Animal</font>类型进行操作即可。由于<font color='red'>Animal</font>类型有<font color='red'>run()</font>方法，因此，传入的任意类型，**只要是Animal类或者子类，就会自动调用实际类型的run()方法，这就是多态的意思**：

对于一个变量，我们只需要知道它是Animal类型，无需确切地知道它的子类型，就可以放心地调用run()方法，而具体调用的run()方法是作用在Animal、Dog、Cat还是Tortoise对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种Animal的子类时，只要确保run()方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：

**对扩展开放：允许新增Animal子类；**
**对修改封闭：不需要修改依赖Animal类型的run_twice()等函数。**

继承还可以一级一级地继承下来，就好比从爷爷到爸爸、再到儿子这样的关系。而任何类，最终都可以追溯到根类object，这些继承关系看上去就像一颗倒着的树。比如如下的继承树：
```python

                ┌───────────────┐
                │    object     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │   Animal    │           │    Plant    │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │   Cat   │  │  Tree   │  │ Flower  │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
```
静态语言 vs 动态语言
对于静态语言（例如Java）来说，如果需要传入<font color='red'>Animal</font>类型，则传入的对象**必须是Animal类型或者它的子类，否则，将无法调用run()方法。**

对于Python这样的动态语言来说，则不一定需要传入<font color='red'>Animal</font>类型。我们只需要保证传入的对象有一个<font color='red'>run()</font>方法就可以了：
```python
class Timer(object):
    def run(self):
        print('Start...')

run_twice(Timer())
Start...
Start...
```
这就是动态语言的**鸭子类型**，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

Python的“file-like object“就是一种鸭子类型。对真正的文件对象，它有一个<font color='red'>read()</font>方法，返回其内容。但是，许多对象，只要有<font color='red'>read()</font>方法，都被视为“file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全可以传入任何实现了<font color='red'>read()</font>方法的对象。

#### 获取对象信息
当我们拿到一个对象的引用时，如何知道这个对象是什么类型、有哪些方法呢？

使用<font color='red'>type()</font>
首先，我们来判断对象类型，使用<font color='red'>type()</font>函数，基本类型都可以用<font color='red'>type()</font>判断：
```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```
如果一个变量指向函数或者类，也可以用type()判断：
```python
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```
但是<font color='red'>type()</font>函数返回的是什么类型呢？它返回对应的Class类型。如果我们要在if语句中判断，就需要比较两个变量的type类型是否相同：
```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```
判断基本数据类型可以直接写<font color='red'>int</font>，<font color='red'>str</font>等，但如果要判断一个对象是否是函数怎么办？可以使用<font color='red'>types</font>模块中定义的常量：
```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```
使用<font color='red'>isinstance()</font>
对于class的继承关系来说，使用type()就很不方便。我们要判断class的类型，可以使用isinstance()函数。
我们回顾上次的例子，如果继承关系是：
object -> Animal -> Dog -> Husky
那么，isinstance()就可以告诉我们，一个对象是否是某种类型。先创建3种类型的对象：
```python
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
```
然后，判断：
```python
>>> isinstance(h, Husky)
True
```
没有问题，因为<font color='red'>h</font>变量指向的就是<font color='red'>Husky</font>对象。再判断：
```python
>>> isinstance(h, Dog)
True
```
<font color='red'>h</font>虽然自身是Husky类型，但由于Husky是从<font color='red'>Dog</font>继承下来的，所以，<font color='red'>h</font>也还是Dog类型。换句话说，<font color='red'>isinstance()</font>判断的是一个对象是否是该类型本身，或者位于该类型的父继承链上。因此，我们可以确信，<font color='red'>h</font>还是Animal类型：
```python
>>> isinstance(h, Animal)
True
```
同理，实际类型是Dog的d也是Animal类型：
```python
>>> isinstance(d, Dog) and isinstance(d, Animal)
True
```
但是，d不是Husky类型：
```python
>>> isinstance(d, Husky)
False
```
能用<font color='red'>type()</font>判断的基本类型也可以用<font color='red'>isinstance()</font>判断：
```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```
并且还可以判断一个变量是否是某些类型中的一种，比如下面的代码就可以判断是否是<font color='red'>list</font>或者<font color='red'>tuple</font>：
```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```
总是优先使用<font color='red'>isinstance()</font>判断类型，可以将指定类型及其子类“一网打尽”。

**使用dir()**
如果要获得一个对象的所有属性和方法，可以使用<font color='red'>dir()</font>函数，它返回一个包含字符串的<font color='red'>list</font>，比如，获得一个<font color='red'>str</font>对象的所有属性和方法：
```python
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```
类似<font color='red'>\_\_xxx__</font>的属性和方法在Python中都是有特殊用途的，比如<font color='red'>\_\_len__</font>方法返回长度。在Python中，如果你调用<font color='red'>len()</font>函数试图获取一个对象的长度，实际上，在<font color='red'>len()</font>函数内部，它自动去调用该对象的<font color='red'>\_\_len__()</font>方法，所以，下面的代码是等价的：
```python
>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```
我们自己写的类，如果也想用<font color='red'>len(myObj)</font>的话，就自己写一个<font color='red'>\_\_len__()</font>方法：
```python
>>> class MyDog(object):
...     def __len__(self):
...         return 100
...
>>> dog = MyDog()
>>> len(dog)
100
```
剩下的都是普通属性或方法，比如<font color='red'>lower()</font>返回小写的字符串：
```python
>>> 'ABC'.lower()
'abc'
```
仅仅把属性和方法列出来是不够的，配合<font color='red'>getattr()</font>、<font color='red'>setattr()</font>以及<font color='red'>hasattr()</font>，我们可以直接操作一个对象的状态：
```python
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()
```
紧接着，可以测试该对象的属性：
```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```
如果试图获取不存在的属性，会抛出AttributeError的错误：
```python
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```
可以传入一个<font color='red'>default</font>参数，如果属性不存在，就返回默认值：
```python
>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404
```
也可以获得对象的方法：
```python
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```
小结
通过内置的一系列函数，我们可以对任意一个Python对象进行剖析，拿到其内部的数据。要注意的是，只有在不知道对象信息的时候，我们才会去获取对象信息。如果可以直接写：
```python
sum = obj.x + obj.y
```
就不要写：
```python
sum = getattr(obj, 'x') + getattr(obj, 'y')
```
一个正确的用法的例子如下：
```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
    return None
```
假设我们希望从文件流fp中读取图像，我们首先要判断该fp对象是否存在<font color='red'>read</font>方法，如果存在，则该对象是一个流，如果不存在，则无法读取。<font color='red'>hasattr()</font>就派上了用场。

请注意，在Python这类动态语言中，根据鸭子类型，有read()方法，不代表该fp对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要read()方法返回的是有效的图像数据，就不影响读取图像的功能。

#### 实例属性和类属性
由于Python是动态语言，根据类创建的实例可以任意绑定属性。给实例绑定属性的方法是通过实例变量，或者通过<font color='red'>self</font>变量：
```python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```
但是，如果<font color='red'>Student</font>类本身需要绑定一个属性呢？可以直接在class中定义属性，这种属性是类属性，归<font color='red'>Student</font>类所有：
```python
class Student(object):
    name = 'Student'
```
当我们定义了一个类属性后，这个属性虽然归类所有，但类的所有实例都可以访问到。来测试一下：
```python
>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```
从上面的例子可以看出，在编写程序的时候，**千万不要对实例属性和类属性使用相同的名字，因为相同名称的实例属性将屏蔽掉类属性**，但是当你删除实例属性后，再使用相同的名称，访问到的将是类属性。

### 面向对象高级编程
#### 使用__slots__
正常情况下，当我们定义了一个class，创建了一个class的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性。先定义class：
```python
class Student(object):
    pass
```
然后，尝试给实例绑定一个属性：
```python
>>> s = Student()
>>> s.name = 'Michael' # 动态给实例绑定一个属性
>>> print(s.name)
Michael
```
还可以尝试给实例绑定一个方法：
```python
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```
但是，给一个实例绑定的方法，对另一个实例是不起作用的：
```python
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'
```
为了给所有实例都绑定方法，可以给class绑定方法：
```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```
给class绑定方法后，所有实例均可调用：
```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```
通常情况下，上面的<font color='red'>set_score</font>方法可以直接定义在class中，但动态绑定允许我们在程序运行的过程中动态给class加上功能，这在静态语言中很难实现。

**使用__slots__**
但是，如果我们想要限制实例的属性怎么办？比如，只允许对Student实例添加<font color='red'>name</font>和<font color='red'>age</font>属性。

为了达到限制的目的，Python允许在定义class的时候，定义一个特殊的<font color='red'>\_\_slots__</font>变量，来限制该class实例能添加的属性：
```python
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
然后，我们试试：
```python
>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```
由于<font color='red'>'score'</font>没有被放到<font color='red'>\_\_slots__</font>中，所以不能绑定<font color='red'>score</font>属性，试图绑定score将得到AttributeError的错误。

使用<font color='red'>\_\_slots__</font>要注意，<font color='red'>\_\_slots__</font>定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
```python
>>> class GraduateStudent(Student):
...     pass
...
>>> g = GraduateStudent()
>>> g.score = 9999
```
除非在子类中也定义<font color='red'>\_\_slots__</font>，这样，子类实例允许定义的属性就是自身的<font color='red'>\_\_slots__</font>加上父类的<font color='red'>\_\_slots__</font>。

**types.MethodType()**
<font color='red'>types.MethodType</font>可以给实例或类绑定新的方法或属性
**语法**
<font color='red'>types.MethodType(method, object, TypeName)</font>
**参数**
<font color='red'>method</font> -- 要绑定的方法
<font color='red'>object</font> -- 绑定的对象
TypeName -- 类名，可省略
**返回值**
method
实例
```python
from types import MethodType

class Student(object):
    pass

def set_name(self, name):
    self.name = name

>>> s1 = Student()
>>> s2 = Student()
>>> s1.set_name = MethodType(set_name, s1)
>>> s1.set_name('Bob')
>>> s1.name
'Bob'
```

#### 使用@property
在绑定属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，导致可以把成绩随便改：
```python
s = Student()
s.score = 9999
```
这显然不合逻辑。为了限制score的范围，可以通过一个<font color='red'>set_score()</font>方法来设置成绩，再通过一个<font color='red'>get_score()</font>来获取成绩，这样，在<font color='red'>set_score()</font>方法里，就可以检查参数：
```python
class Student(object):

    def get_score(self):
         return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
现在，对任意的Student实例进行操作，就不能随心所欲地设置score了：
```python
>>> s = Student()
>>> s.set_score(60) # ok!
>>> s.get_score()
60
>>> s.set_score(9999)
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```
但是，上面的调用方法又略显复杂，没有直接用属性这么直接简单。有没有既能检查参数，又可以用类似属性这样简单的方式来访问类的变量呢？对于追求完美的Python程序员来说，这是必须要做到的！

还记得装饰器（decorator）可以给函数动态加上功能吗？对于类的方法，装饰器一样起作用。Python内置的<font color='red'>@property</font>装饰器就是负责把一个方法变成属性调用的：
```python
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
<font color='red'>@property</font>的实现比较复杂，我们先考察如何使用。把一个<font color='red'>getter</font>方法变成属性，只需要加上<font color='red'>@property</font>就可以了，此时，<font color='red'>@property</font>本身又创建了另一个装饰器<font color='red'>@score.setter</font>，负责把一个<font color='red'>setter</font>方法变成属性赋值，于是，我们就拥有一个可控的属性操作：
```python
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```
注意到这个神奇的@property，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性：
```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```
上面的<font color='red'>birth</font>是可读写属性，而<font color='red'>age</font>就是一个只读属性，因为<font color='red'>age</font>可以根据<font color='red'>birth</font>和当前时间计算出来。

要特别注意：**属性的方法名不要和实例变量重名**。例如，以下的代码是错误的：
```python
class Student(object):

    # 方法名称和实例变量均为birth:
    @property
    def birth(self):
        return self.birth
```
这是因为调用s.birth时，首先转换为方法调用，在执行return self.birth时，又视为访问self的属性，于是又转换为方法调用，造成无限递归，最终导致栈溢出报错<font color='red'>RecursionError</font>。

####多重继承
继承是面向对象编程的一个重要的方式，因为通过继承，子类就可以扩展父类的功能。回忆一下Animal类层次的设计，假设我们要实现以下4种动物：
Dog - 狗狗；Bat - 蝙蝠；Parrot - 鹦鹉；Ostrich - 鸵鸟。如果按照哺乳动物和鸟类归类，我们可以设计出这样的类的层次：
```python
                ┌───────────────┐
                │    Animal     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │   Mammal    │           │    Bird     │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │   Bat   │  │ Parrot  │  │ Ostrich │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
```
但是如果按照“能跑”和“能飞”来归类，我们就应该设计出这样的类的层次：
```python
                ┌───────────────┐
                │    Animal     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │  Runnable   │           │   Flyable   │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │ Ostrich │  │ Parrot  │  │   Bat   │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
```
如果要把上面的两种分类都包含进来，我们就得设计更多的层次：哺乳类：能跑的哺乳类，能飞的哺乳类；鸟类：能跑的鸟类，能飞的鸟类。这么一来，类的层次就复杂了：
```python
                ┌───────────────┐
                │    Animal     │
                └───────────────┘
                        │
           ┌────────────┴────────────┐
           │                         │
           ▼                         ▼
    ┌─────────────┐           ┌─────────────┐
    │   Mammal    │           │    Bird     │
    └─────────────┘           └─────────────┘
           │                         │
     ┌─────┴──────┐            ┌─────┴──────┐
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│  MRun   │  │  MFly   │  │  BRun   │  │  BFly   │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
     │            │            │            │
     │            │            │            │
     ▼            ▼            ▼            ▼
┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐
│   Dog   │  │   Bat   │  │ Ostrich │  │ Parrot  │
└─────────┘  └─────────┘  └─────────┘  └─────────┘
```
如果要再增加“宠物类”和“非宠物类”，这么搞下去，类的数量会呈指数增长，很明显这样设计是不行的。正确的做法是采用多重继承。首先，主要的类层次仍按照哺乳类和鸟类设计：
```python
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```
现在，我们要给动物再加上<font color='red'>Runnable</font>和<font color='red'>Flyable</font>的功能，只需要先定义好<font color='red'>Runnable</font>和<font color='red'>Flyable</font>的类：
```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```
对于需要<font color='red'>Runnable</font>功能的动物，就多继承一个<font color='red'>Runnable</font>，例如Dog：
```python
class Dog(Mammal, Runnable):
    pass
对于需要Flyable功能的动物，就多继承一个Flyable，例如Bat：

class Bat(Mammal, Flyable):
    pass
```
通过多重继承，一个子类就可以同时获得多个父类的所有功能。

**MixIn**
在设计类的继承关系时，通常，主线都是单一继承下来的，例如，<font color='red'>Ostrich</font>继承自<font color='red'>Bird</font>。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让<font color='red'>Ostrich</font>除了继承自<font color='red'>Bird</font>外，再同时继承<font color='red'>Runnable</font>。这种设计通常称之为MixIn。

**为了更好地看出继承关系**，我们把<font color='red'>Runnable</font>和<font color='red'>Flyable</font>改为<font color='red'>RunnableMixIn</font>和<font color='red'>FlyableMixIn</font>。类似的，你还可以定义出肉食动物<font color='red'>CarnivorousMixIn</font>和植食动物<font color='red'>HerbivoresMixIn</font>，让某个动物同时拥有好几个MixIn：
```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```
MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。

Python自带的很多库也使用了MixIn。举个例子，Python自带了<font color='red'>TCPServer</font>和<font color='red'>UDPServer</font>这两类网络服务，而要同时服务多个用户就必须使用多进程或多线程模型，这两种模型由<font color='red'>ForkingMixIn</font>和<font color='red'>ThreadingMixIn</font>提供。通过组合，我们就可以创造出合适的服务来。比如，编写一个多进程模式的TCP服务，定义如下：
```python
class MyTCPServer(TCPServer, ForkingMixIn):
    pass
```
编写一个多线程模式的UDP服务，定义如下：
```python
class MyUDPServer(UDPServer, ThreadingMixIn):
    pass
```
如果你打算搞一个更先进的协程模型，可以编写一个<font color='red'>CoroutineMixIn</font>：
```python
class MyTCPServer(TCPServer, CoroutineMixIn):
    pass
```
这样一来，我们不需要复杂而庞大的继承链，只要选择组合不同的类的功能，就可以快速构造出所需的子类。

#### 定制类
看到类似<font color ='red'>\_\_slots__</font>这种形如<font color ='red'>\_\_xxx__</font>的变量或者函数名就要注意，这些在Python中是有特殊用途的。Python的class中还有许多这样有特殊用途的函数，可以帮助我们定制类。

**\_\_str__**
我们先定义一个Student类，打印一个实例：
```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print(Student('Michael'))
<__main__.Student object at 0x109afb190>
```
默认情况下，是输出是由哪一个类创建的对象，以及在内存中的地<font color ='red'><\_\_main__.Student object at 0x109afb190></font>怎么才能打印出想要的结果呢？只需要定义好<font color ='red'>\_\_str__()</font>方法，打印实例时就会从这个方法中打印出return的字符串数据。
```python
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print(Student('Michael'))
Student object (name: Michael)
```
这样打印出来的实例，不但好看，而且容易看出实例内部重要的数据。但是细心的朋友会发现直接敲变量不用print，打印出来的实例还是不好看：
```python
>>> s = Student('Michael')
>>> s
<__main__.Student object at 0x109afb310>
```
这是因为直接显示变量调用的不是<font color ='red'>\_\_str__()</font>，而是<font color ='red'>\_\_repr__()</font>，两者的区别是<font color ='red'>\_\_str__()</font>返回用户看到的字符串，而<font color ='red'>\_\_repr__()</font>返回程序开发者看到的字符串，也就是说，<font color ='red'>\_\_repr__()</font>是为调试服务的。

解决办法是再定义一个<font color ='red'>\_\_repr__()</font>。但是通常<font color ='red'>\_\_str__()</font>和<font color ='red'>\_\_repr__()</font>代码都是一样的，所以，有个偷懒的写法：
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```
<font color ='red'>\_\_str__</font>和<font color ='red'>\_\_repr__</font>的差别究竟在哪里，它们的功能都是实现类到字符串的转化，它们的特定并没有体现出用途上的差异。带着这个这个问题，我们就来python标准库中<font color ='red'>datetime.date</font>这个类是怎么在使用这两个方法的。
```python
>>> import datetime as d
>>> today = d.date.today()
>>> str(today)
'2021-07-17'
>>> repr(today)
'datetime.date(2021, 7, 17)'
```
<font color ='red'>\_\_str__</font>的返回结果可读性强。也就是说，<font color ='red'>\_\_str__</font>的意义是得到便于人们阅读的信息，就像上面的 <font color ='red'>'2018-04-03'</font>一样。

<font color ='red'>\_\_repr__</font>的返回结果应更准确。怎么说，<font color ='red'>\_\_repr__</font> 存在的目的在于调试，便于开发者使用。细心的读者会发现将<font color ='red'>\_\_repr__</font>返回的方式直接复制到命令行上，是可以直接执行的。

**\_\_iter__**
如果一个类想被用于<font color ='red'>for ... in</font>循环，类似list或tuple那样，就必须实现一个<font color ='red'>\_\_iter__()</font>方法，该方法返回一个迭代对象，然后，Python的for循环就会不断调用该迭代对象的<font color ='red'>\_\_next__()</font>方法拿到循环的下一个值，直到遇到<font color ='red'>StopIteration</font>错误时退出循环。

我们以斐波那契数列为例，写一个<font color ='red'>Fib</font>类，可以作用于for循环：
```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
```
现在，试试把Fib实例作用于for循环：
```python
>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```
**\_\_getitem__(索引方法)**
凡是在类中定义了这个<font color ='red'>\_\_getitem__ </font>方法，那么它的实例对象（假定为p），可以像这样<font color ='red'>p[key]</font> 取值，当实例对象做<font color ='red'>p[key]</font>运算时，会调用类中的方法<font color ='red'>\_\_getitem__</font>。
Fib实例虽然能作用于for循环，看起来和list有点像，但是，把它当成list来使用还是不行，比如，取第5个元素：
```python
>>> Fib()[5]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Fib' object does not support indexing
```
要表现得像list那样按照下标取出元素，需要实现<font color ='red'>\_\_getitem__()</font>方法：
```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a
```
现在，就可以按下标访问数列的任意一项了：
```python
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[10]
89
>>> f[100]
573147844013817084101
```
但是list有个神奇的切片方法：
```python
>>> list(range(100))[5:10]
[5, 6, 7, 8, 9]
```
对于Fib却报错。原因是<font color ='red'>\_\_getitem__()</font>传入的参数可能是一个<font color ='red'>int</font>，也可能是一个切片对象<font color ='red'>slice</font>，所以要做判断：
```python
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L
```
现在试试<font color ='red'>Fib</font>的切片：
```ptyhon
>>> f = Fib()
>>> f[0:5]
[1, 1, 2, 3, 5]
>>> f[:10]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
但是没有对step参数作处理：
```python
>>> f[:10:2]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```
也没有对负数作处理，所以，要正确实现一个<font color ='red'>\_\_getitem__()</font>还是有很多工作要做的。

此外，如果把对象看成<font color ='red'>dict</font>，<font color ='red'>\_\_getitem__()</font>的参数也可能是一个可以作key的object，例如<font color ='red'>str</font>。与之对应的是<font color ='red'>\_\_setitem__()</font>方法，把对象视作list或dict来对集合赋值。最后，还有一个<font color ='red'>\_\_delitem__()</font>方法，用于删除某个元素。总之，通过上面的方法，我们自己定义的类表现得和Python自带的list、tuple、dict没什么区别，这完全归功于动态语言的“鸭子类型”，不需要强制继承某个接口。

**\_\_getattr__**
正常情况下，当我们调用类的方法或属性时，如果不存在，就会报错。比如定义Student类：
```python
class Student(object):
    
    def __init__(self):
        self.name = 'Michael'
```
调用name属性，没问题，但是，调用不存在的score属性，就有问题了：
```python
>>> s = Student()
>>> print(s.name)
Michael
>>> print(s.score)
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'score'
```
错误信息很清楚地告诉我们，没有找到score这个attribute。要避免这个错误，除了可以加上一个score属性外，Python还有另一个机制，那就是写一个<font color ='red'>\_\_getattr__()</font>方法，动态返回一个属性。修改如下：
```python
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99
```
当调用不存在的属性时，比如score，Python解释器会试图调用<font color ='red'>\_\_getattr__(self, 'score')</font>来尝试获得属性，这样，我们就有机会返回score的值：
```python
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
99
```
返回函数也是完全可以的：
```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
```            
只是调用方式要变为：
```python
>>> s.age()
25
```
注意，只有在没有找到属性的情况下，才调用<font color ='red'>\_\_getattr__</font>，已有的属性，比如name，不会在<font color ='red'>__getattr__</font>中查找。

此外，注意到任意调用如<font color ='red'>s.abc</font>都会返回<font color ='red'>None</font>，这是因为我们定义的<font color ='red'>\_\_getattr__</font>默认返回就是<font color ='red'>None</font>。要让class只响应特定的几个属性，我们就要按照约定，抛出<font color ='red'>AttributeError</font>的错误：
```python
class Student(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```
这实际上可以把一个类的所有属性和方法调用全部动态化处理了，不需要任何特殊手段。

这种完全动态调用的特性有什么实际作用呢？作用就是，可以针对完全动态的情况作调用。

举个例子：

现在很多网站都搞REST API，比如新浪微博、豆瓣啥的，调用API的URL类似：
http://api.server/user/friends
http://api.server/user/timeline/list
如果要写SDK，给每个URL对应的API都写一个方法，那得累死，而且，API一旦改动，SDK也要改。
利用完全动态的__getattr__，我们可以写出一个链式调用：
```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__
```
试试：
```python
>>> Chain().status.user.timeline.list
'/status/user/timeline/list'
```
这样，无论API怎么变，SDK都可以根据URL实现完全动态的调用，而且，不随API的增加而改变！
还有些REST API会把参数放到URL中，比如GitHub的API：
```python
GET /users/:user/repos
```
调用时，需要把:user替换为实际用户名。如果我们能写出这样的链式调用：
```python
Chain().users('michael').repos
```
就可以非常方便地调用API了。有兴趣的童鞋可以试试写出来。

**\_\_call__**
一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用instance.method()来调用。能不能直接在实例本身上调用呢？在Python中，答案是肯定的。

任何类，只需要定义一个<font color='red'>\_\_call__()</font>方法，就可以直接对实例进行调用。请看示例：
```python
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self, age):
        print('My name is %s, age is %d.' % (self.name, age))
```
调用方式如下：
```python
>>> s = Student('Michael')
>>> s(22) # self参数不要传入
My name is Michael, age is 22
```
<font color='red'>\_\_call__()</font>还可以定义参数。对实例进行直接调用就好比对一个函数进行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为这两者之间本来就没啥根本的区别。如果你把对象看成函数，那么函数本身其实也可以在运行期动态创建出来，因为类的实例都是运行期创建出来的，这么一来，我们就模糊了对象和函数的界限。

那么，怎么判断一个变量是对象还是函数呢？其实，更多的时候，我们需要判断一个对象是否能被调用，能被调用的对象就是一个<font color='red'>Callable</font>对象，比如函数和我们上面定义的带有<font color='red'>\_\_call__()</font>的类实例：
```python
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```
通过<font color='red'>callable()</font>函数，我们就可以判断一个对象是否是“可调用”对象。

#### 使用枚举类
当我们需要定义常量时，一个办法是用大写变量通过整数来定义，例如月份：
```python
JAN = 1
FEB = 2
MAR = 3
...
NOV = 11
DEC = 12
```
好处是简单，缺点是类型是<font color='red'>int</font>，并且仍然是变量。更好的方法是为这样的枚举类型定义一个class类型，然后，每个常量都是class的一个唯一实例。Python提供了<font color='red'>Enum</font>类来实现这个功能：
```python
from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
这样我们就获得了Month类型的枚举类，可以直接使用<font color='red'>Month.Jan</font>来引用一个常量，或者枚举它的所有成员：
```python
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)

Jan => Month.Jan , 1
Feb => Month.Feb , 2
Mar => Month.Mar , 3
Apr => Month.Apr , 4
May => Month.May , 5
Jun => Month.Jun , 6
Jul => Month.Jul , 7
Aug => Month.Aug , 8
Sep => Month.Sep , 9
Oct => Month.Oct , 10
Nov => Month.Nov , 11
Dec => Month.Dec , 12
```
<font color='red'>value</font>属性则是自动赋给成员的<font color='red'>int</font>常量，默认从<font color='red'>1</font>开始计数。如果需要更精确地控制枚举类型，可以从<font color='red'>Enum</font>派生出自定义类：
```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```
<font color='red'>@unique</font>装饰器可以帮助我们检查保证没有重复值。
访问这些枚举类型可以有若干种方法：
```python
>>> day1 = Weekday.Mon
>>> print(day1)
Weekday.Mon
>>> print(Weekday.Tue)
Weekday.Tue
>>> print(Weekday['Tue'])
Weekday.Tue
>>> print(Weekday.Tue.value)
2
>>> print(day1 == Weekday.Mon)
True
>>> print(day1 == Weekday.Tue)
False
>>> print(Weekday(1))
Weekday.Mon
>>> print(day1 == Weekday(1))
True
>>> Weekday(7)
Traceback (most recent call last):
  ...
ValueError: 7 is not a valid Weekday
>>> for name, member in Weekday.__members__.items():
...     print(name, '=>', member)
...
Sun => Weekday.Sun
Mon => Weekday.Mon
Tue => Weekday.Tue
Wed => Weekday.Wed
Thu => Weekday.Thu
Fri => Weekday.Fri
Sat => Weekday.Sat
```
可见，既可以用成员名称引用枚举常量，又可以直接根据value的值获得枚举常量。

#### 使用元类
**type()**
动态语言和静态语言最大的不同，就是函数和类的定义，不是编译时定义的，而是运行时动态创建的。比方说我们要定义一个<font color='red'>Hello</font>的class，就写一个<font color='red'>hello.py</font>模块：
```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```
当Python解释器载入hello模块时，就会依次执行该模块的所有语句，执行结果就是动态创建出一个Hello的class对象，测试如下：
```python
>>> from hello import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class 'hello.Hello'>
```
<font color='red'>type()</font>函数可以查看一个类型或变量的类型，Hello是一个class，它的类型就是<font color='red'>type</font>，而<font color='red'>h</font>是一个实例，它的类型就是<font color='red'>class Hello</font>。

我们说class的定义是运行时动态创建的，而创建class的方法就是使用<font color='red'>type()</font>函数。<font color='red'>type()</font>函数既可以返回一个对象的类型，又可以创建出新的类型，比如，我们可以通过<font color='red'>type()</font>函数创建出<font color='red'>Hello</font>类，而无需通过<font color='red'>class Hello(object)...</font>的定义：
```python
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class '__main__.Hello'>
```
要创建一个class对象，<font color='red'>type()</font>函数依次传入<font color='red'>3</font>个参数：
**语法**
<font color='red'>type(name, tuple, dict)</font>
**参数**
<font color='red'>name</font>-类的名称
<font color='red'>bases</font>-表示一个元组，其中存储的是该类的父类，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法
<font color='red'>dict</font> 表示一个字典，用于表示类内定义的属性或者方法。
**返回值**
一个参数返回对象类型, 三个参数，返回新的**类型对象**。

通过<font color='red'>type()</font>函数创建的类和直接写class是完全一样的，因为Python解释器遇到class定义时，依然是用<font color='red'>type()</font>来创建这个类。

正常情况下，我们都用<font color='red'>class Xxx...</font>来定义类，但是，<font color='red'>type()</font>函数也允许我们动态创建出类来，也就是说，动态语言本身支持运行期动态创建类，这和静态语言有非常大的不同，要在静态语言运行期创建类，必须构造源代码字符串再调用编译器，或者借助一些工具生成字节码实现，本质上都是动态编译，会非常复杂。

**metaclass**
除了使用type()动态创建类以外，要控制类的创建行为，还可以使用metaclass。

metaclass，直译为元类，简单的解释就是：

当我们定义了类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例。

但是如果我们想创建出类呢？那就必须根据metaclass创建出类，所以：先定义metaclass，然后创建类。

连接起来就是：先定义metaclass，就可以创建类，最后创建实例。

所以，metaclass允许你创建类或者修改类。换句话说，你可以把类看成是metaclass创建出来的“实例”。

metaclass是Python面向对象里最难理解，也是最难使用的魔术代码。正常情况下，你不会碰到需要使用metaclass的情况，所以，以下内容看不懂也没关系，因为基本上你不会用到。

我们先看一个简单的例子，这个metaclass可以给我们自定义的MyList增加一个add方法：

定义ListMetaclass，按照默认习惯，metaclass的类名总是以Metaclass结尾，以便清楚地表示这是一个metaclass：

metaclass是类的模板，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)
有了ListMetaclass，我们在定义类的时候还要指示使用ListMetaclass来定制类，传入关键字参数metaclass：

class MyList(list, metaclass=ListMetaclass):
    pass
当我们传入关键字参数metaclass时，魔术就生效了，它指示Python解释器在创建MyList时，要通过ListMetaclass.__new__()来创建，在此，我们可以修改类的定义，比如，加上新的方法，然后，返回修改后的定义。

__new__()方法接收到的参数依次是：

当前准备创建的类的对象；

类的名字；

类继承的父类集合；

类的方法集合。

测试一下MyList是否可以调用add()方法：

>>> L = MyList()
>>> L.add(1)
>> L
[1]
而普通的list没有add()方法：

>>> L2 = list()
>>> L2.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'add'
动态修改有什么意义？直接在MyList定义中写上add()方法不是更简单吗？正常情况下，确实应该直接写，通过metaclass修改纯属变态。

但是，总会遇到需要通过metaclass修改类定义的。ORM就是一个典型的例子。

ORM全称“Object Relational Mapping”，即对象-关系映射，就是把关系数据库的一行映射为一个对象，也就是一个类对应一个表，这样，写代码更简单，不用直接操作SQL语句。

要编写一个ORM框架，所有的类都只能动态定义，因为只有使用者才能根据表的结构定义出对应的类来。

让我们来尝试编写一个ORM框架。

编写底层模块的第一步，就是先把调用接口写出来。比如，使用者如果使用这个ORM框架，想定义一个User类来操作对应的数据库表User，我们期待他写出这样的代码：

class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')

创建一个实例：
u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
保存到数据库：
u.save()
其中，父类Model和属性类型StringField、IntegerField是由ORM框架提供的，剩下的魔术方法比如save()全部由父类Model自动完成。虽然metaclass的编写会比较复杂，但ORM的使用者用起来却异常简单。

现在，我们就按上面的接口来实现该ORM。

首先来定义Field类，它负责保存数据库表的字段名和字段类型：

class Field(object):

    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)
在Field的基础上，进一步定义各种类型的Field，比如StringField，IntegerField等等：

class StringField(Field):

    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')

class IntegerField(Field):

    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')
下一步，就是编写最复杂的ModelMetaclass了：

class ModelMetaclass(type):

    def __new__(cls, name, bases, attrs):
        if name=='Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mappings # 保存属性和列的映射关系
        attrs['__table__'] = name # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)
以及基类Model：

class Model(dict, metaclass=ModelMetaclass):

    def __init__(self, **kw):
        super(Model, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self, k, None))
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
当用户定义一个class User(Model)时，Python解释器首先在当前类User的定义中查找metaclass，如果没有找到，就继续在父类Model中查找metaclass，找到了，就使用Model中定义的metaclass的ModelMetaclass来创建User类，也就是说，metaclass可以隐式地继承到子类，但子类自己却感觉不到。

在ModelMetaclass中，一共做了几件事情：

排除掉对Model类的修改；

在当前类（比如User）中查找定义的类的所有属性，如果找到一个Field属性，就把它保存到一个__mappings__的dict中，同时从类属性中删除该Field属性，否则，容易造成运行时错误（实例的属性会遮盖类的同名属性）；

把表名保存到__table__中，这里简化为表名默认为类名。

在Model类中，就可以定义各种操作数据库的方法，比如save()，delete()，find()，update等等。

我们实现了save()方法，把一个实例保存到数据库中。因为有表名，属性到字段的映射和属性值的集合，就可以构造出INSERT语句。

编写代码试试：

u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
u.save()
输出如下：

Found model: User
Found mapping: email ==> <StringField:email>
Found mapping: password ==> <StringField:password>
Found mapping: id ==> <IntegerField:uid>
Found mapping: name ==> <StringField:username>
SQL: insert into User (password,email,username,id) values (?,?,?,?)
ARGS: ['my-pwd', 'test@orm.org', 'Michael', 12345]
可以看到，save()方法已经打印出了可执行的SQL语句，以及参数列表，只需要真正连接到数据库，执行该SQL语句，就可以完成真正的功能。

不到100行代码，我们就通过metaclass实现了一个精简的ORM框架，是不是非常简单？

**super()函数**
通常情况下，我们在子类中定义了和父类同名的方法，那么子类的方法就会覆盖父类的方法。而<font color='red'>super</font>关键字实现了对父类方法的改写(增加了功能，增加的功能写在子类中，父类方法中原来的功能得以保留)。也可以说，<font color='red'>super</font>关键字帮助我们实现了在子类中调用父类的方法
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f'Hello, My name is {self.name}')

class Dog(Animal):
    def speak(self):
        super().speak()
        print('wang wang...')

输出如下：
>>>dog = Dog('Mike')
>>>dog.speak()
Hello, My name is Mike
wang wang...
```
**super函数深入**
<font color='red'>MRO(Method Resolution Order)</font>：**python对于每一个类都有一个MRO列表. 此表的生成有以下原则：子类永远在父类之前，如果有多个父类，那么按照它们在列表中的<font color='red'>顺序</font>被检查，如果下一个类有两个合法的选择，那么就只选择第一个**
```python
class Baseclass(object):
    def enter(self):
        print('enter Baseclass')
        print('leave Baseclass')

class B(Baseclass):
    def enter(self):
        print('enter B')
        super().enter()
        print('leave B')

class A(Baseclass):
    def __init__(self):
        print('Auto enter')
    def enter(self):
        print('enter A')
        print('leave A')

class D(B, A):
    def enter(self):
        print('enter D')
        super().enter()
        print('leave D')
```
执行结果:
```python
>>>from supperf import *
>>>d.enter()
enter D
enter B
enter A
leave A
leave B
leave D
```
可以用<font color='red'>\_\_mro__</font>方法检查一下<font color='red'>mro表</font>顺序:
```python
>>> D.__mro__
(<class 'supperf.D'>, <class 'supperf.B'>, <class 'supperf.A'>, <class 'supperf.
Baseclass'>, <class 'object'>)
```
因为mro表顺序的关系，这么写是错误的:
```python
class E(Baseclass, A):
    pass

class F(D, A, B):
    pass
```




