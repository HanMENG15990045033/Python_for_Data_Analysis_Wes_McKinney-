
### 3.2.3 Function are Objects 函数即对象 

对象？那我就不客气了。

![0314xiankaiwaiyi](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0314xiankaiwaiyi.png)

所谓函数即对象，就是f(x),这个x可以是函数，而且可以是一堆函数

回头再看一下把我们带到坑里的那个例子


```python
states = ['   #Alabama?  ', 'FlOrIda', 'south   carolina##', 'West virginia?']
```


```python
import re

def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip()
        value = re.sub('[!#?]', '', value)
        value = value.title()
        result.append(value)
    return result

clean_strings(states)
```




    ['Alabama', 'Florida', 'South   Carolina', 'West Virginia']



学完正则表达式看这个就很轻松了

然而这居然只是暖场？？！！！

![0315momotou](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0315momotou.png)

作者真正想说的是对象，对象的事儿


```python
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)
## 先定义一个函数，他的功能就是删除标点

clean_ops = [str.strip, remove_punctuation, str.title]
## 定义一个列表，元素为函数！！！
## 除去前后的空格换行，删除标点，变成标题格式

def clean_strings(strings, ops):
    ## 定义函数清洗字符串列表，参数为要清洗的字符串，和实现清洗的函数
    result = []
    ## 空列表，装结果
    for value in strings:
        # 对于字符串列表里的值
        for function in ops:
            # 取出一个函数
            value = function(value)
            # 对值执行函数
        result.append(value)
    return result

clean_strings(states, clean_ops)
```




    ['Alabama', 'Florida', 'South   Carolina', 'West Virginia']



这个两个词儿中间有多个空格也要去掉

我再改一下


```python
def remove_punctuation(value):
    return re.sub('[!#? ]', '', value)#在这里加了一个空格

clean_ops = [str.strip, remove_punctuation, str.title]

def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result

clean_strings(states, clean_ops)
```




    ['Alabama', 'Florida', 'Southcarolina', 'Westvirginia']



貌似不对，应该是多个空格替换为一个空格，这个怎么搞

我试试


```python
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)

def remove_spaces(value):
    return re.sub(' +', ' ', value)
    ## 一个及以上的空格，替换为一个空格
    ## 通配符9 +：1 or more of previous expression 1个或多个前面的字符

clean_ops = [str.strip, remove_punctuation, remove_spaces, str.title]
## 这里加一个函数

def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result

clean_strings(states, clean_ops)
```




    ['Alabama', 'Florida', 'South Carolina', 'West Virginia']



耶成功！

不要高兴的太早。。。

还有map()函数

噗。。吐血

![0306](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0306.png)

没事儿，别扶我，我习惯了


```python
for x in map(remove_punctuation, states):
    print(x)
```

       Alabama  
    FlOrIda
    south   carolina
    West virginia
    

这个可以将一个函数，应用到一个序列上

remove_punctuation就是一个函数

states列表就是一个序列，做出类似于for循环的处理

### 3.2.4 Anonymous (Lambda) Functions 匿名函数Lambda

有的函数结构太简单，简单到不配拥有姓名

也有可能这个函数他就是很酷

![0316lengmo](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0316lengmo.png)


这种情况就想直接用单个语句生成函数，如：

y = lambda x: x * 2

lambda就用来声明这是个函数，不过没名

在这本书里经常要用到这种形式

有什么好处？看例子



```python
ints = [4, 0, 1, 5, 6]
ints_3 = [x * 2 for x in ints]
ints_4 = [x ** 2 for x in ints]

print(ints_3)
print(ints_4)
```

    [8, 0, 2, 10, 12]
    [16, 0, 1, 25, 36]
    


```python
ints_5 = [10, 230, 41, 55, 67]
ints_6 = [x * 2 for x in ints_5]
ints_7 = [x ** 2 for x in ints_5]

print(ints_6)
print(ints_7)
```

    [20, 460, 82, 110, 134]
    [100, 52900, 1681, 3025, 4489]
    

这个列表可以抽象出来一个函数

[f(x) for x in some_list]

其中f(x)和some_list就是两个变量


```python
def apply_to_list(some_list, f):
    ## 定义函数
    ## 列表
    ## 函数
    ## 返回经过函数处理的列表值
    return [f(x) for x in some_list]


ints = [4, 0, 1, 5, 6]
ints_2 = apply_to_list(ints, lambda x: x * 2)
## 这里做参数的函数直接用一个lambda定义
print(ints_2)
```

    [8, 0, 2, 10, 12]
    

耶。

![0317keai](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0317keai.png)

再来个例子：这书翻译的有点不好理解

这个例子想实现的功能是，根据不同字母的个数排序

如foo，fo两个字母

card，四个字母

aaaa，a一个字母


```python
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']

strings.sort(key=lambda x: len(set(list(x))))
strings
```




    ['aaaa', 'foo', 'abab', 'bar', 'card']



误：list是吧strings变成列表，本身就是列表，所以作用不大

正：x是列表的里的元素，比如字符串‘foo’

list把foo变成列表['f','o','o']

set是吧列表变为集合，去掉重复元素

len得到set元素的长度

lambda以上三步操作作为一个匿名函数，对x，也就是strings执行

key。。？？？

![0318meng](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0318meng.jpg)

额，又懵了吧，别慌，稳住

这就要补一下sort的用法了


#### 3.2.4.1 list.sort与sorted()用法补充

参考：

https://www.cnblogs.com/ShaunChen/p/6205330.html

###### list.sort

功能：对列表排序，改变原来列表

通式：list.sort(func=None, key=None, reverse=False(or True))

###### sorted()

功能：对序列排序，不改变原来列表

通式：sorted(iteralable, key=None, reverse=False)

- func为了避免混乱返回None？不管了先
- key在调用之前，对每个函数进行的函数操作
- reverse=False正序，=True反序



```python
student_tuples = [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]

sorted(student_tuples, key=lambda student: student[2])   # sort by age

## 处理对象这个列表，是个可迭代对象
## key对于列表的每个元素进行的函数处理
## 比如('john', 'A', 15)
## lambda，这个函数不def定义了，直接上
## 函数参数student就是这个('john', 'A', 15)
## 做的处理，是提出第二位元素，也就是15
## 处理后返回这个student[2]，15
## 所有元素都经过这个处理之后，变成了，15，12，10
## 再排序10，12，15
## 排出来就应该是dave，jane，john这个顺序
```




    [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]




```python
def test(a=110):
    print(a)
test()
```

    110
    

OK回头看书上的例子，就很容易了


```python
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']

strings.sort(key=lambda x: len(set(list(x))))
strings
```




    ['aaaa', 'foo', 'abab', 'bar', 'card']



key：对每个参数执行以下key对应的参数操作


```python
## 第一步list(x)

[list(x) for x in strings] 
```




    [['a', 'a', 'a', 'a'],
     ['f', 'o', 'o'],
     ['a', 'b', 'a', 'b'],
     ['b', 'a', 'r'],
     ['c', 'a', 'r', 'd']]




```python
## 第二步 set(list(x))

[set(list(x)) for x in strings]
```




    [{'a'}, {'f', 'o'}, {'a', 'b'}, {'a', 'b', 'r'}, {'a', 'c', 'd', 'r'}]




```python
## 第三步 len(set(list(x)))

[len(set(list(x))) for x in strings]
```




    [1, 2, 2, 3, 4]



按着这个排序

原来：['foo', 'card', 'bar', 'aaaa', 'abab']

排完：['aaaa', 'foo', 'abab', 'bar', 'card']

作者：和def关键字声明的函数不同，匿名函数没有__name__属性

![0319emmm](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0319emmm.png)

嗯。。。好吧

### 3.2.5 Currying: Partial Argument Application 柯里化：部分参数的应用

https://nbviewer.jupyter.org/github/pydata/pydata-book/blob/2nd-edition/ch03.ipynb
    
不要拿一个人名来吓唬我

其实就是从已有函数衍生新的函数


```python
## 把两个数字加载一起
def add_numbers(x, y): 
    return x + y

## 改编为只有y一个参数的函数
add_five = lambda y: add_numbers(5, y)

## 注意这是个函数，调用它
add_five(8)
```




    13




```python
from functools import partial
## functools函数工具包
## 导入这个函数

add_five = partial(add_numbers, 5)
## partial的功能就是调用这个add_numbers这个函数吧

## add_numbers 应该是个已经定义好的功能函数？
add_five(8)
```




    13




```python
## add_five = partial(add_numbers, 5)
## 这个相当于

def add_five_2(y):
    return add_numbers(5, y)

add_five_2(10)
```




    15




```python
def minus_numbers(x, y): 
    return x - y

minus_five = lambda x: minus_numbers(x, 5)

minus_five(8)
```




    3




```python
from functools import partial

minus_five = partial(minus_numbers, 5)
## 这里5默认是x

minus_five(10)
## 10默认为y
```




    -5




```python
from functools import partial

minus_five = partial(5, minus_numbers)
## 不能换位置哦

minus_five(10)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-22-fa79d5b40f97> in <module>
          1 from functools import partial
          2 
    ----> 3 minus_five = partial(5, minus_numbers)
          4 
          5 minus_five(10)
    

    TypeError: the first argument must be callable



```python
from functools import partial

minus_five = partial(minus_numbers, y=5)

minus_five(10)
```




    5




```python
## N先生的补充：什么叫函数即对象

def add_numbers(x, y): 
    return x + y 

def test(a): 
    return a(1, 2)
   
print(add_numbers)# 这是一个函数，function
print(test(add_numbers)) # 这是函数作为另一个函数的变量
```

    <function add_numbers at 0x00000132FE52C2F0>
    3
    

哈哈哈N先生专业救场20年，跪谢！

![0320baodatui](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0320baodatui.png)


### 3.2.6 Generators 生成器

迭代器协议

迭代器就是一种在上下文中（比如for循环）向Python解释器生成对象的对象


```python
some_dict = {'a': 1, 'b': 2, 'c': 3}
for key in some_dict:
    print(key)
    
## for这种就让Python解释器尝试生成一个迭代器
```

    a
    b
    c
    


```python
dict_iterator = iter(some_dict)
dict_iterator
```




    <dict_keyiterator at 0x132fe568188>




```python
list(dict_iterator)
```




    ['a', 'b', 'c']




```python
list(dict_iterator)
## 似乎生成之后只能调用一次
```




    []




```python
dict_iterator_2 = iter(some_dict)
## 所以没办法，只能再定义
tuple(dict_iterator_2)
```




    ('a', 'b', 'c')




```python
some_list = [5, 4, 3, 2, 1]
list_iterator = iter(some_list)
```


```python
set(list_iterator)
```




    {1, 2, 3, 4, 5}




```python
list_iterator_2 = iter(some_list)
min(list_iterator_2)
```




    1




```python
list_iterator_3 = iter(some_list)
max(list_iterator_3)
```




    5




```python
l = [1, 2, 3]
a = iter(l)
b = iter(l)

print(a)
print(b)

print(list(a))
print(list(b))
print(list(b))
print(b)
## 迭代器只跟自己有关
## 迭代器有一个开始位置和结束位置
```

    <list_iterator object at 0x00000132FE544D68>
    <list_iterator object at 0x00000132FE5449B0>
    [1, 2, 3]
    [1, 2, 3]
    []
    <list_iterator object at 0x00000132FE5449B0>
    


```python
lst = [1, 2, 3]
for i in iter(lst):
     print(i)
```

    1
    2
    3
    


```python
a = [1, 2, 3]

for x in a:
    print(a.index(x))

```

    0
    1
    2
    


```python
l = [1, 2, 3, 4, 5, 6, 7, 8]

for i in range(0, len(l)):
    print(l[i])
```

    1
    2
    3
    4
    5
    6
    7
    8
    

迭代器的优势

比如要找第4位，索引有点像整个for循环，从0开始，找到4结束

迭代器有点像for循环里的一步，走到第三步，下面就是第四步

索引每次都从0开始数

就因为0数 会有多余的步骤 降低了程序性能

比如有个 100w个元素的list  如果用索引 每次都从0位开始数

不是很浪费时间吗

索引就是记住最开始位置  迭代就是记住了当前操作到的位置


```python
l = [1, 2, 3, 4, 5, 6, 7, 8]

iter_l = iter(l)
next(iter_l)
```




    1




```python
next(iter_l)
```




    2




```python
print(list(iter_l))
```

    [3, 4, 5, 6, 7, 8]
    

创建一个生成器，只需将函数的return换成yiel


```python
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i ** 2
```


```python
gen = squares()
gen
```




    <generator object squares at 0x00000132FE5552A0>




```python
for x in gen:
    print(x, end=' ')
```

    Generating squares from 1 to 100
    1 4 9 16 25 36 49 64 81 100 


```python
for x in gen:
    print(x, end=' ')
## 还是只能用一次
```

yeild a 相当于return a 只不过return很知足，得到a就结束

yeild得到a之后，继续执行代码，还可以继续得到b c d

a独白：早知道你是这样的yeild，当初啥也不让你得到我




```python
def yield_test_1(n):
    a = n
    yield a
    b = n * 2
    yield b
    c = n * 3
    yield c
    d = n * 4
    yield d

print(yield_test_1(10))
for i in yield_test_1(10):
    print(i)
```

    <generator object yield_test_1 at 0x00000132FE555A20>
    10
    20
    30
    40
    


```python
def yield_test(n):
    for i in range(n):
        yield change(i) 
        ## 功能生成一串鞭炮，每个小鞭儿调用一次change
        ## 并保存，注意保存了
        #print("i = ",i)
    print("end.")

def change(i):
    return i*2

print(yield_test(5)) # 生成了一串鞭炮，change（0），change（1）...change(5)

for i in yield_test(5):#使用for循环点鞭炮
    print(i, "in for")

sum(yield_test(6)) # 用sum点鞭炮
```

    <generator object yield_test at 0x00000132FE555480>
    0 in for
    2 in for
    4 in for
    6 in for
    8 in for
    end.
    end.
    




    30




```python
def yield_test(n):
    for i in range(n):
        yield change(i)
        print("函数的for循环：",i)
    print("end.")

def change(i):
    return i*2

#使用for循环
for i in yield_test(5):
    print("迭代器的结果：",i)
```

    迭代器的结果： 0
    函数的for循环： 0
    迭代器的结果： 2
    函数的for循环： 1
    迭代器的结果： 4
    函数的for循环： 2
    迭代器的结果： 6
    函数的for循环： 3
    迭代器的结果： 8
    函数的for循环： 4
    end.
    


```python
def yield_test(n):
    for i in range(n):
        return change(i)
        print("函数的for循环：",i)
    print("end.")

def change(i):
    return i*2

yield_test(5)
```




    0



#### 3.2.6.1 Generator expresssions 生成器的表达式

列表推导式的[  ]换为(  )


```python
gen = (x ** 2 for x in range(100))
gen

```




    <generator object <genexpr> at 0x00000132FE555408>




```python
# 相当于
def _make_gen(): 
    for x in range(100): 
        yield x ** 2 
gen = _make_gen()
```


```python
sum(x ** 2 for x in range(100))
```




    328350




```python
dict((i, i **2) for i in range(5))
```




    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}



我去，太好用了吧

![0321wanmei](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0321wanmei.png)

#### 3.2.6.2 itertools module 模块

还记得groupby吗，以前按周数吧数据分组的时候用过一次

groupby(iterable[，keyfunc分组依据])


```python
from itertools import groupby 

test = [(1, 5), (1, 4), (1, 3), (1, 2), (2, 4), (2, 3), (3, 5)]
temp = groupby(test, key=lambda x: x[0])
## 得到一个迭代器
## x就是一个元组如（1，5）
## key=x[0]根据元组的0位元素分组，1是一组，2一组，3一组
print(temp)
## 得到一组数，分类的标准，分类的结果是个列表1 []
for a, b in temp:
    print(a, list(b))
```

    groupby的结果
    <itertools.groupby object at 0x00000132FE57D518>
    1 [(1, 5), (1, 4), (1, 3), (1, 2)]
    2 [(2, 4), (2, 3)]
    3 [(3, 5)]
    


```python
import itertools
first_letter = lambda x: x[0]
names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']

for letter, names in itertools.groupby(names, first_letter):
    print(letter, list(names)) # names is a generator
```

    A ['Alan', 'Adam']
    W ['Wes', 'Will']
    A ['Albert']
    S ['Steven']
    

其他

combinations (iterable, k) 组合

permutations (iterable, k) 排列

product (*iterables, repeat=1) 多组之间组合


```python
from itertools import combinations
test = combinations([1, 2, 3, 4], 3)
for n in test:
    print(n)
```

    (1, 2, 3)
    (1, 2, 4)
    (1, 3, 4)
    (2, 3, 4)
    


```python
from itertools import permutations
test = permutations([1, 2, 3, 4], 3)
for n in test:
    print(n)
```

    (1, 2, 3)
    (1, 2, 4)
    (1, 3, 2)
    (1, 3, 4)
    (1, 4, 2)
    (1, 4, 3)
    (2, 1, 3)
    (2, 1, 4)
    (2, 3, 1)
    (2, 3, 4)
    (2, 4, 1)
    (2, 4, 3)
    (3, 1, 2)
    (3, 1, 4)
    (3, 2, 1)
    (3, 2, 4)
    (3, 4, 1)
    (3, 4, 2)
    (4, 1, 2)
    (4, 1, 3)
    (4, 2, 1)
    (4, 2, 3)
    (4, 3, 1)
    (4, 3, 2)
    


```python
import itertools
test = itertools.product('abc', 'xy')
for n in test:
    print(n)
```

    ('a', 'x')
    ('a', 'y')
    ('b', 'x')
    ('b', 'y')
    ('c', 'x')
    ('c', 'y')
    

### Errors and Exception Handling 错误和异常处理

之前见过，但还是要深化一下

本段主要帮你解决，跟Python表白：

不确定能不能成功怎么办

被拒绝想知道为什么怎么办

被拒绝也不想气氛尴尬怎么办

破天荒成功了怎么办

不管成不成功都想干点啥怎么办

![0322chengyi](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0322chengyi.png)


```python
float('1.2345')
float('something')
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-146-6d335c618d25> in <module>
          1 float('1.2345')
    ----> 2 float('something')
    

    ValueError: could not convert string to float: 'something'


1. except 回避所有错误


```python
def attempt_float(x):
    try:
        return float(x)
    except:
        return x

attempt_float('哈哈,我真秀')
```




    '哈哈,我真秀'



2. except AError回避指定错误


```python
float((1, 2))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-155-82f777b0e564> in <module>
    ----> 1 float((1, 2))
    

    TypeError: float() argument must be a string or a number, not 'tuple'



```python
def attempt_float_2(x):
    try:
        return float(x)
    except TypeError:
        return x

attempt_float_2((1, 2))
```




    (1, 2)




```python
attempt_float_2('蒂花之秀') ## 没有指定的类型还是会照常报错
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-165-a90db91675cb> in <module>
    ----> 1 attempt_float_2('蒂花之秀') ## 没有指定的类型还是会照常报错
    

    <ipython-input-157-c9bb0f8deb8d> in attempt_float_2(x)
          1 def attempt_float_2(x):
          2     try:
    ----> 3         return float(x)
          4     except TypeError:
          5         return x
    

    ValueError: could not convert string to float: '蒂花之秀'



```python
def attempt_float_3(x):
    try:
        return float(x)
    except (ValueError, TypeError):
        return x

attempt_float_3((1, 2))
```




    (1, 2)




```python
attempt_float_3('造化钟神秀')
```




    '造化钟神秀'



这书说的太委婉了

直接看N先的远程支援

![0323wo](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0323wo.png)


```python
try:
     1 / 0
except ZeroDivisionError: # 指定错误，多个用元组
     print("Get AError")
except:
     print("exception")  # 所有报错
else:
     print("else") # try 成功执行的
finally:
     print("finally") # 成不成功都执行的
```

    Get AError
    finally
    

N先生友情提醒：

else语句的存在必须以except X或者except语句为前提

如果在没有except语句的try block中使用else语句

会引发语法错误


```python
try:
     1 / 0
else:
     print("else")
```


      File "<ipython-input-150-fd8e4bf958ce>", line 3
        else:
           ^
    SyntaxError: invalid syntax
    


## 3.3 Files and the Operating System 文件与操作系统




这段，书上简单的一页，知道我走了多少坑嘛！！！

![0324bufangqi](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0324bufangqi.jpg)

这部分大体分三步

第一步把文件存好，第二步把文件读出来，第三步把文件关上

最后看一眼Python文件读取模式

文件方法或属性

### 第一步：存文件

文件在作者的github里，链接如下

https://github.com/wesm/pydata-book/blob/2nd-edition/examples/segismundo.txt

【二维码，作者github3.3文件】


文件里面大概是这么个东西 西班牙语，大概是一首诗

Sueña el rico en su riqueza,

que más cuidados le ofrece;

...



建立文件的过程也有一点曲折

然后我在自己的文件里建了txt复制进去

![0329unicode](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0329Unicode.PNG)

保存的时候该文件含有Unicode字符？？然后我点了取消

![0339unicode](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0330Unicode.PNG)

我选了unicode

### 第二步：读文件


```python
# path = 'examples/segismundo.txt'

path = r'C:\Users\Huawei\Desktop\data_analysis\ch0302.txt'
# r不写会报错
# SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3: truncated \UXXXXXXXX escape


# f = open(path)
# 直接这么开，会报错
# UnicodeDecodeError: 'gbk' codec can't decode byte 0xff in position 0: illegal multibyte sequence

# f = open(path,encoding='UTF-8')
# encoding = 'UTF-8也会报错'
# UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte


f = open(path,encoding='unicode_escape')
# 唯一成功不报错的方法
```

文件里面大概是这么个东西 西班牙语，大概是一首诗

Sueña el rico en su riqueza,

que más cuidados le ofrece;



sueña el pobre que padece

su miseria y su pobreza;



sueña el que a medrar empieza,

sueña el que afana y pretende,

sueña el que agravia y ofende,



y en el mundo, en conclusión,

todos sueñan lo que son,

aunque ninguno lo entiende.


```python
path = r'C:\Users\Huawei\Desktop\data_analysis\ch0302.txt'
f = open(path,encoding='unicode_escape')
```

默认情况下以只读模式打开，相当于r

生成的f是一个可迭代的东西

就是可以用for循环读出来


```python
for line in f:
    print(line)
```

    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
    
     
    
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
    
     
    
     
    


```python
lines_1 = [line for line in f]
lines_1
```




    []



咦？这个为什么是空？？

![0325meng](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0325meng.jpg)

呼叫基地请求远程支援

N先生给了个例子


引出了seek()功能


```python
f_2 = open('ch0302.txt', encoding='unicode_escape')

print('第1次：')
for line in f_2:
    print(line)

print('第2次：\n')
for line in f_2:
    print(line)

print('第3次：')
f_2.seek(0)
for line in f_2:
    print(line)
```

    第1次：
    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
    
     
    
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
    
     
    
     
    第2次：
    
    第3次：
    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
    
     
    
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
    
     
    
     
    

啊，大概是这么回事儿

这个文件存在变量f里

f的读取类似于迭代器

但读完可以用seek（0）恢复到最开始

我先这么理解着

![0326zan](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0326zan.jpg)


```python
f.seek(0)
lines_2 = [line for line in f]
lines_2
```




    ['ÿþS\x00u\x00e\x00ñ\x00a\x00 \x00e\x00l\x00 \x00r\x00i\x00c\x00o\x00 \x00e\x00n\x00 \x00s\x00u\x00 \x00r\x00i\x00q\x00u\x00e\x00z\x00a\x00,\x00\n',
     '\x00\n',
     '\x00q\x00u\x00e\x00 \x00m\x00á\x00s\x00 \x00c\x00u\x00i\x00d\x00a\x00d\x00o\x00s\x00 \x00l\x00e\x00 \x00o\x00f\x00r\x00e\x00c\x00e\x00;\x00\n',
     '\x00\n',
     '\x00']



额，一堆码，我有点慌，是我又错了嘛

场外支援：

我的理解是t作为一个list  是一个整体

把列表元素打印出来

才能编解码看到正常的文字


```python
for x in lines_2:
    print(x)
```

    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
    
     
    
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
    
     
    
     
    

欧乐\(^o^)/~

![0327nice](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0327nice.jpg)

往下，看我们打印出来的空格是不是感觉间隔有点大

看一眼那一对乱码，有\n

用rstrip去掉


```python
f.seek(0)
lines_3 = [x.rstrip() for x in f]
lines_3
```




    ['ÿþS\x00u\x00e\x00ñ\x00a\x00 \x00e\x00l\x00 \x00r\x00i\x00c\x00o\x00 \x00e\x00n\x00 \x00s\x00u\x00 \x00r\x00i\x00q\x00u\x00e\x00z\x00a\x00,\x00',
     '\x00',
     '\x00q\x00u\x00e\x00 \x00m\x00á\x00s\x00 \x00c\x00u\x00i\x00d\x00a\x00d\x00o\x00s\x00 \x00l\x00e\x00 \x00o\x00f\x00r\x00e\x00c\x00e\x00;\x00',
     '\x00',
     '\x00']




```python
for x in lines_3:
    print(x)
```

    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
     
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
     
     
    

这个文件读取模式，句柄的概念为啥不提前说，这样后面这几个也能理解了

seek()将句柄位置改变到特定字节

read()根据字节数推进文件句柄的位置

tell()句柄当前的位置




```python
f_5 = open(path,encoding='unicode_escape')

t0 = f_5.tell() ## 当前句柄位置
print(t0)

r1 = f_5.read(1) ## read往前推一个，读一个
t1 = f_5.tell()

print(r1,t1)

r2 = f_5.read(10) ## read往前推10个字节，读出来
t2 = f_5.tell()
print(r2,t2)

f_5.seek(0)

t3 = f_5.tell()
print(t3)
```

    0
    ÿ 1
    þS u e ñ a 11
    0
    

N先生友情提醒：

seek() 只有三个

seek(0)代表从文件开头开始算起

seek(1)代表从当前位置开始算起

seek(2)代表从文件末尾算起。



```python
f_5.close()
```

书上说请牢记关闭问文件,可见关闭还挺重要的

### 第三步：关文件

1. close()关闭文件


```python
f.close()
```

试一下关了没


```python
f.seek(0)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-75-16754355c5bd> in <module>
    ----> 1 f.seek(0)
    

    ValueError: I/O operation on closed file.


2. 用with打开，with代码结束后自动关闭


```python
with open(path, encoding='unicode_escape') as f_4:
    lines_4 = [x.rstrip() for x in f_4]
```


```python
f_4.seek(0)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-86-285464532cad> in <module>
    ----> 1 f_4.seek(0)
    

    ValueError: I/O operation on closed file.



```python
for x in lines_4:
    print(x)
```

    ÿþS u e ñ a   e l   r i c o   e n   s u   r i q u e z a , 
     
     q u e   m á s   c u i d a d o s   l e   o f r e c e ; 
     
     
    

可见f_4被关了，lines留下了

### 检验文件的默认编码



```python
import sys
sys.getdefaultencoding()
```




    'utf-8'



### Python文件模式

r 只读

w 只写，创建新文件，覆盖原文件

x 只写，创建新文件，不覆盖原文件，同名报错

a 添加到已存在文件，不存在就创建

r+ 读写

b 二进制模式，跟其他结合使用，rb，wb，xb等

t 文件的文本模式，自动解码为Unicode，可以单独使用，也可以跟其他结合使用，rt，xt等


```python
## 有一个ch0303，文件取出来，放到ch0304

path_2 = r'C:\Users\Huawei\Desktop\data_analysis\ch0303.txt'

with open('ch0304.txt', 'w') as handle:
    handle.writelines(x for x in open(path_2))
                      
with open('ch0304.txt') as f:
    lines = f.readlines()

for x in lines:
    print(x.strip())
```

    啊白云
    
    黑土向你道歉
    
    来到你们前
    
    请你睁开眼瞅我多可怜
    

![0331baiyunheitu](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0331baiyunheitu.jpg)


### 重要的Python方法或属性

read([size]) 将文件数据作为字符串返回，可选参数size控制读取的字节数

readlines([size]) 返回文件中行内容的列表，size参数可选

write(str) 将字符串写入文件

writelines(strings) 将字符串序列写入文件

close() 关闭文件

flush() 将内部I/O缓冲器内容刷新到硬盘

seek(pos) 移动到指定的位置(整数)

tell() 返回当前位置

closed 如果文件已关闭，则为True


```python
import os
os.remove('ch0304.txt')
```

删除文件

### 3.1.1 Bytes and Unicode with Files 字节与Unicode文件


战略放弃，之后有需要再深入了解这个编码的事儿




```python
with open(path, encoding = '') as f:
    chars = f.read(10)
chars
```


    ---------------------------------------------------------------------------

    UnicodeDecodeError                        Traceback (most recent call last)

    <ipython-input-134-b3e5d16127fa> in <module>
          1 with open(path) as f:
    ----> 2     chars = f.read(10)
          3 chars
    

    UnicodeDecodeError: 'gbk' codec can't decode byte 0xff in position 0: illegal multibyte sequence



```python
with open(path, 'rb') as f:
    data = f.read(10)
data
```


```python
data.decode('utf8')
data[:4].decode('utf8')
```


```python
sink_path = 'sink.txt'
with open(path) as source:
    with open(sink_path, 'xt', encoding='iso-8859-1') as sink:
        sink.write(source.read())
with open(sink_path, encoding='iso-8859-1') as f:
    print(f.read(10))
```

如果常常需要在非ASCII文本数据上进行数据分析

精通Python的Unicode功能是很有必要的

参见Python官方文档

http://docs.python.ord/

https://blog.csdn.net/aisq2008/article/details/6298170


![0328lue](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0328lue.jpg)
