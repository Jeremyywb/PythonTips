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
