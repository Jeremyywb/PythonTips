# PythonTips
![handyfuntions](http://image.slidesharecdn.com/pythonfunctionsbeginner-140911195806-phpapp01/95/python-functions-pyatl-beginners-night-1-638.jpg?cb=1410465910)

----
[CSDN博客主页](http://blog.csdn.net/yb_udex/article/details/52647791)
 - 快速生成字典

```
In [1]: a =["a","b","c","d","e","f"]

In [2]: b = {a[i]:i for i in range(len(a))}

In [3]: b
Out[3]: {'a': 0, 'b': 1, 'c': 2, 'd': 3, 'e': 4, 'f': 5}
```

 - 简洁条件判断

```
In [4]: j = [x for x in range(10)]

In [5]: results = []
   ...: for i in j:
   ...:     result = 2<=i<6
   ...:     results.append(result)
   ...:     

In [6]: results
Out[6]: [False, False, True, True, True, True, False, False, False, False]
```

 - 快速生成随机测试数据框数据集

```
In [7]: import pandas as pd
   ...: import random
   ...: df = pd.DataFrame(
   ...: {"class":[random.choice(a) for x in range(10)],
   ...:  "scores":[random.randrange(100) for x in range(10)],
   ...:  "random_sex":["boy" if random.randrange(10) > 5 else "girl" for x in range(10)]
   ...: })
   
In [7]: df
Out[7]: 
  class random_sex  scores
0     f        boy      70
1     a       girl      56
2     f        boy      69
3     e        boy       8
4     b       girl      86
5     c       girl      63
6     a       girl       0
7     a       girl      81
8     d        boy      77
9     f        boy      48

In [9]: 
```

 - lamba表达式+布尔值，DataFrame快速筛选

```
In [9]: f = lambda x:60<x<70

In [10]: df[df.scores.map(f)]
Out[10]: 
  class random_sex  scores
2     f        boy      69
5     c       girl      63
```

 - 补充上面布尔式子

```
In [11]: j = [x for x in range(10)]
    ...: 
    ...: results = []
    ...: for i in j:
    ...:     result = 2<=i<6
    ...:     results.append(result)
    ...:     

In [12]: results
Out[12]: [False, False, True, True, True, True, False, False, False, False]
```

 - 下划线"_"检查最后输出

```
In [14]: _
Out[14]: [False, False, True, True, True, True, False, False, False, False]
```

 - 反向迭代

```
In [15]: a[::-1]
Out[15]: ['f', 'e', 'd', 'c', 'b', 'a']
```

 - zip,namespace

> 1、二元结构生成（见test）
> 2、字典的map（见test0）
> 3、类似展开（见test1）
> 4、同上（test2）

```
import itertools
t1 = ("a", "b", "c")
t2 = (10, 20, 30)
test =list(zip(t1,t2))
test0 =dict(zip(t1,t2))
test1 = list(itertools.chain.from_iterable(test0))
test2 = list(itertools.chain.from_iterable(test))

def namestr(obj, namespace):
    return [name for name in namespace if namespace[name] is obj]

for i in [test,test0,test1,test2]:
    print(namestr(i, globals())," =>:",i)
```
OUTPUT
```
['i', 'test']  =>: [('a', 10), ('b', 20), ('c', 30)]
['test0', 'i']  =>: {'b': 20, 'a': 10, 'c': 30}
['test1', 'i']  =>: ['b', 'a', 'c']
['test2', 'i']  =>: ['a', 10, 'b', 20, 'c', 30]
```

 - 递归-字符去重（仅连续字符有效）

```
def remvoedup(word):
    if len(word)<=1:
        return word
    elif word[0]==word[1]:
        return remvoedup(word[1:])
    else:
        return word[0]+remvoedup(word[1:])
```

OUPUT

```
In [19]: remvoedup("aaaaaaabbccdef")
Out[19]: 'abcdef'
```

 - 时间转日期？哪个简洁

**code1**
```
import datetime
import re

def textTOtime(text):
    ##use re
    listtext =re.findall("..",text[4:])
    ##also can use generator expresstion
#     listtext =[text[0:4]]+[text[i:i+2] for i in range(4, len(text),2)]
    inder = ["-","-"," ",":",":"]
    timeformat =text[0:4]
    for i in range(5):
        timeformat+=inder[i]+listtext[i]
    return datetime.datetime.strptime(timeformat,"%Y-%m-%d %H:%M:%S")
```
**code2**

```
##improve--sometext[:4]+"".join(i+j for i,j in zip(indicator,re.findall("..",sometext[4:])) )

def imptextTOtime(text):
	inder = ["","-","-"," ",":",":",""]
	timeformat ="".join(i+j for i,j in zip( re.findall("..",text),inder) )
	return datetime.datetime.strptime(timeformat,"%Y-%m-%d %H:%M:%S")
```

output

```
In [50]: textTOtime(sometext)
Out[50]: datetime.datetime(2016, 9, 24, 7, 4, 22)

In [51]: imptextTOtime(sometext)
Out[51]: datetime.datetime(2016, 9, 24, 7, 4, 22)
```

 - greadtools 代码调试工具

```
import pdb
pdb.set_trace()

def addup(a,b):
	c = a+b
	c=c+d
	return c
addup(1, 2)
```

> 上面这段蹩脚的代码，进入ipython跑之后。。。下面的s,n,q分别是进入pdb调试阶段后的"进入函数"，"跑下一行代码"，"退出"

```
PS D:\CODES\ProjectCT\Kaggle\handyfuncs> ipython.exe .\test.p


> d:\codes\projectct\kaggle\handyfuncs\test.py(4)<module>()
-> def addup(a,b):
(Pdb) s
> d:\codes\projectct\kaggle\handyfuncs\test.py(8)<module>()
-> addup(1, 2)
(Pdb) s
--Call--
> d:\codes\projectct\kaggle\handyfuncs\test.py(4)addup()
-> def addup(a,b):
(Pdb) n
> d:\codes\projectct\kaggle\handyfuncs\test.py(5)addup()
-> c = a+b
(Pdb) n
> d:\codes\projectct\kaggle\handyfuncs\test.py(6)addup()
-> c=c+d
(Pdb) n
NameError: name 'd' is not defined
> d:\codes\projectct\kaggle\handyfuncs\test.py(6)addup()
-> c=c+d
(Pdb) q
```



### python生成式
![这里写图片描述](http://www.ecohealthypets.com/writable/pet_report_photos/photo/480x/ball_python_2.jpg)

----

 - 某博客阐述生成器


![这里写图片描述](http://f.cxyblog.com/images/io1/2016/06/06/1465196806.png)

---

> 在Python中，拥有这种能力的“函数”被称为生成器，它非常的有用。生成器（以及yield语句）最初的引入是为了让程
序员可以更简单的编写用来产生值的序列的代码。 以前，要实现类似随机数生成器的东西，需要实现一个类或者一个模块，在生成数据的同时保持对每次调用之间状态的跟踪。引入生成器之后，这变得非常简单。
为了更好的理解生成器所解决的问题，让我们来看一个例子。在了解这个例子的过程中，请始终记住我们需要解决的问题：生成值的序列。
注意：在Python之外，最简单的生成器应该是被称为协程（coroutines）的东西。在本文中，我将使用这个术语。请记住，在Python的概念中，这里提到的协程就是生成器。Python正式的术语是生成器；协程只是便于讨论，在语言层面并没有正式定义。

---


   python对象有两个重要的器，一个是迭代器一个是生成器，自打学python开始就不断接触两者，可从来理解都是**生成器区别于迭代只不过前者无实际执行而是每次调用的时候遍历一个值并指针停留该处**，于是乎自然而然一直是这个想法，自从一次使用。。。。

- if else 结构 选择使用生成器：

```
def genforbrow(obj,browlen, fileORvar):
    if fileORvar =="var":
        print("var")
        for index in range(0,len(obj)-1,browlen):
            if index>len(obj):
                print("no data left")
            else:
                yield obj[index:(index+browlen)]
    elif fileORvar == "file":
        print("pandas")
        return pd.read_csv(obj, iterator= True, chunksize= browlen)
    else:
        print("no parameter called: %s"%fileORvar)
```

使用

```

kk = genforbrow("train.csv", 2,"file")

kk
Out[7]: <generator object genforbrow at 0x000000A842058308>
```

咦！！！什么情况？？？ 我的fileORvar 传入参数是 "file"为何解雇是 generator？那段代码按道理应该不执行才对，我们单独试试？

```
In [8]: def something(obj,browlen):
   ...:     return pd.read_csv(obj, iterator= True, chunksize= browlen)
   ...: 
  
In [9]:gg = something("train.csv",5)
In [9]:gg
Out[9]: <pandas.io.parsers.TextFileReader at 0xa8420612e8>

```

额，单独执行就可以返回pandas对象，好奇怪，好吧，再做一次简单的实验。。。

```
def test_yield(n):
    kk =[]
    if n<5:
        for i in range(n):
            kk.append(i)
    elif n<10:
        for i in range(n):
            kk.append(i)
    elif n<20:
        for i in range(n):
            kk.append(i)
    elif n<30:
        for i in range(n):
            kk.append(i)
    else:
        for i in range(n):
            yield kk.append(i)
            yield kk
    return kk
```

执行

```

In [11]: test = test_yield(40)

In [12]: next(test)

In [13]: next(test)
Out[13]: [0]

In [14]: next(test)

In [15]: next(test)
Out[15]: [0, 1]

In [16]: next(test)
###没有返回值
In [17]: next(test)
Out[17]: [0, 1, 2]

In [18]: next(test)
###没有返回值
In [19]: next(test)
Out[19]: [0, 1, 2, 3]

```

规律出来了吧？看起来yield就是一个暂定器，迭代被暂停了，只有你再调用的时候才会执行，等于就是一个卡顿的执行过程；那下面这种情况呢？我们把n值改成5？

```
In [20]: test = test_yield(5)

In [21]: next(test)

StopIterationTraceback (most recent call last)
<ipython-input-21-911ea584f8be> in <module>()
----> 1 next(test)

StopIteration: [0, 1, 2, 3, 4]
```

。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
。。。。。。。。。。。。。。。。。。。。。。。。。。。。。

一脸懵B，什么情况，居然报错了。。。简直，要不要人活了，不是说是个暂定作用的么？一下子跑出来输出[0,1,2,3,4]才对的呢。。。 还以为懂了呢。。。你大爷的，好吧重点来了！！！

-----

 - 什么是生成器？

我们回过头来比划比划那张图

![这里写图片描述](http://f.cxyblog.com/images/io1/2016/06/06/1465196806.png)

图在说什么：表达式是生成器，生成式函数是生成器，生成器是迭代器，list,set,dict等等是容器，容器是可迭代的，对容器进行迭代生成迭代器（总之来说生成器有个好处就是，不需要容器存储数据，不用的时候不执行，省内存，省CPU）：：：晕，什么乱七八糟的，看个例子，理解下迭代过程

```
from itertools import count
counter = count(start=10)
next(counter)
Out[3]: 10
next(counter)
Out[4]: 11
next(counter)
Out[4]: 11
```
itertools这货先是生成一个迭代器，然后next方法调用迭代器，哦，原来迭代是要有生成者和使用者的呢，而迭代器是通过next方法调用的呢（PS,这货很强大，可以偷着用→[货源](http://wklken.me/posts/2013/08/20/python-extra-itertools.html)）
呜，再加一个例子理解下

```
In [57]: x = [1, 2, 3]

In [58]: y = iter(x)

In [59]: z = iter(x)

In [60]: next(y)
Out[60]: 1

In [61]: next(y)
Out[61]: 2

In [62]: next(y)
Out[62]: 3

In [63]: next(z)
Out[63]: 1

In [64]: next(z)
Out[64]: 2

In [65]: next(y)

StopIterationTraceback (most recent call last)
<ipython-input-65-714521682ae9> in <module>()
----> 1 next(y)

StopIteration: 
```
唔，我什么也不说，只告诉你StopIteration，说明迭代器用干净了，没了。额，也大概理解了。1、迭代是next调用iter（迭代器）的过程 ；2、 容器是可迭代的，但是得用东西把他改造好了，名正言顺的迭代器，然后再迭代；3， 生成器是迭代器，但是生成器好像更懒。

可是，还是没理解yield一下函数就变生成器了。。。没办法，我只能搬出官方的东西，别怪我


翻了下官方文档，给出了这么一段话
>  Using a yield expression in a function’s body causes that function to be a generator.

注意，" in a function’s body",什么问题，就是说，你函数主体如果包含了yield，那你的函数就变成了一个生成器了。而第一次调用的时候函数内部代码不执行，而返回了这个生成器，当你使用for进行迭代的时候.第一次迭代中你的函数会执行，直到碰到 yield 关键字，然后返回 yield 后的值作为第一次迭代的返回值。等等，for是干嘛的？.

找了个例子，不好解释，大家自己看（然而还是大概：for 无非说了：给一个可迭代对象，我不断的获取next方法，迭代迭代迭代）
```
In [66]: import dis

In [67]: x = [1, 2, 3]

In [68]: dis.dis('for _ in x: pass')


  1           0 SETUP_LOOP              14 (to 17)
              3 LOAD_NAME                0 (x)
              6 GET_ITER
        >>    7 FOR_ITER                 6 (to 16)
             10 STORE_NAME               1 (_)
             13 JUMP_ABSOLUTE            7
        >>   16 POP_BLOCK
        >>   17 LOAD_CONST               0 (None)
             20 RETURN_VALUE

```

那上面那个例子yield的值是哪个值呢？很明显就是else部分咯。

```
    else:
        for i in range(n):
            yield kk.append(i)
            yield kk
```

**然而**，yield不是把整个函数对象都变成生辰器了么？那？简单的说yield返回的是地址，每次迭代使用next方法时作用于在最后那个循环，这个for上面的for的迭代器已经消耗并不返回，这个地址也无从迭代可言，反正反正反正，只要有yield（爷）在，你函数就得跟老子姓“生成”，至于你们生死我不管，我只管我待的地方，听起来有点triky，我们下面几个例子依次说明

 - 第一次函数调用不执行
 - 如果yield存在，yield以上代码都执行；yield上的代码只管跑，不返回，即使碰到return也不；不过return能终止迭代，但不妨碍生成器生成，不返回值
 - 代码止于yield
（隐藏一个知识点，生成器是迭代器，迭代器用next方法执行）

```
In [33]: def yield_return():
    ...:     return 1
    ...:     yield 2
    ...:     yield 3
    ...:     yield 4
    ...:     return 5
    ...: 

In [34]: yield_return()
Out[34]: <generator object yield_return at 0x000000A842058360>
```
似乎代码看起来第一个ruturn应该就结束掉代码了？可是。。记住**第一次并不执行**

```
In [36]: next(test_yield)

TypeErrorTraceback (most recent call last)
<ipython-input-36-e17ab9b35113> in <module>()
----> 1 next(test_yield)

TypeError: 'function' object is not an iterator
```
我们改下代码

```
In [41]: test = yield_return()

In [42]: next(test)
Out[42]: 2

In [43]: next(test)
Out[43]: 3

In [44]: next(test)
Out[44]: 4

In [45]: next(test)

StopIterationTraceback (most recent call last)
<ipython-input-45-911ea584f8be> in <module>()
----> 1 next(test)

StopIteration: 5
```
发现return作用已经丧失，代码止于yield，最后一个说明

```
In [48]: def yield_return():
    ...:     for i in range(3):
    ...:         print(i)
    ...:     yield 2
    ...:     yield 3
    ...:     yield 4
    ...:     return 5
    ...: 

In [49]: test = yield_return()

In [50]: next(test)
0
1
2
Out[50]: 2

In [51]: next(test)
Out[51]: 3

In [52]: next(test)
Out[52]: 4

In [53]: next(test)

StopIterationTraceback (most recent call last)
<ipython-input-53-911ea584f8be> in <module>()
----> 1 next(test)

StopIteration: 5
```
这个例子很显著，生成器代码包含了前面迭代代码，但是该代码不属于迭代对象，	且无法使用return返回值，使用return只会导致报错


附加几个链接

[1、完全理解Python迭代对象、迭代器、生成器](http://foofish.net/blog/109/iterators-vs-generators)
[2、Python关键字yield的解释(stackoverflow)¶](http://pyzh.readthedocs.io/en/latest/the-python-yield-keyword-explained.html#id4)
