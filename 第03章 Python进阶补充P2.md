
### 3.1.6 列表、集合和字典的推导式

1. 列表的推导式


```python
# [expr for val in collection if condition]

# 与下面的for循环等价

# result = []
# for val in collection:
    #if condition:
    #result.append(expr)
```

这TM是个什么东西，看例子

![0309](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0309_zheTMshishenme.jpg)

过滤出列表里长度大于2的字符串


```python
strings = ['a', 'bcd', 'efgh', 'i']
```


```python
[x.upper() for x in strings if len(x) > 2]
```




    ['BCD', 'EFGH']



2. 字典

   dict_comp = {key-expr : value-expr for value in colleciton if condition}
   
   如列表中，索引和元素做一个字典


```python
loc_mapping = {val: index for index, val in enumerate(strings)}

loc_mapping
```




    {'a': 0, 'bcd': 1, 'efgh': 2, 'i': 3}



3. 集合的推导式

   set_comp = {expr for value in collection if condition}

   如列表中字符串的长度，提出来做一个集合


```python
strings = ['a', 'bcd', 'efgh', 'i']

unique_lengths = {len(x) for x in strings}

unique_lengths
```




    {1, 3, 4}



3. 也可以用map函数


```python
set(map(len, strings))
```




    {1, 3, 4}



小结：

都是for循环套if判断

看出三个的核心区别在哪了嘛

列表：[ ]

字典：{a:b}

集合：{ }

#### 3.1.6.1 嵌套列表推导式

如想要获得一个类表所有含有两个以上字母e的名字


```python
all_data=[['John', 'Emily', 'Michael', 'Mary', 'Steven'],
         ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar', 'Shenzaier']]
```

可以用for循环


```python
names_of_interest = [] 
# 定义空列白，感兴趣的名字

for names in all_data:
    # 如names=['John', 'Emily', 'Michael', 'Mary', 'Steven']
    
    enough_es = [name for name in names if name.count('e')>=2]
    # 有足够的e的单词组成的列表 = 对于names里面的每一个name，当e>=2的时候，取name
    # 如name=John，不行，Emily不行，Mary不行
    # Steven可以，取
    # enough_es = ['Steven']
    
    names_of_interest.extend(enough_es)
    # extend是扩展 []+[]=[]
    # append是加元素[]append[]=[[]]
    # names_of_interest = ['Steven']
print(names_of_interest)
    
```

    ['Steven', 'Shenzaier']
    

高段位操作来了

还有高段位?! 上面的我感觉已经很简洁了好吗


```python
result = [name for names in all_data for name in names if name.count('e')>=2]

# name是我们列表最终要的值
# 第一个循环for是大循环，从all_data取出names
# 第二个循环for是小循环，从names取name
# 当name满足if条件，就要这个值

result
```




    ['Steven', 'Shenzaier']



再看一个例子

含有整数元组的列表变为整数列表


```python
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]

## 我们最终要的是一个整数，比如x
## 两个循环
## 第一个for取出一个元组
## for a in some_tuples
## 第二个for取出元组里的元素
## for x in a
## 没有条件

flattened = [x for a in some_tuples for x in a]
flattened
```




    [1, 2, 3, 4, 5, 6, 7, 8, 9]




```python
## 换一种写法试试，元组直接拆包

flattened_2 = [a+b+c for a, b, c in some_tuples]
flattened_2
```




    [6, 15, 24]




```python
## 书上还有一种写法，列表推导式

[[x for x in tup] for tup in some_tuples]

## [x for x in tup]是新列表要的元素
## 大循环取出tup
## 小循环取出x
## 所以是一个包含列表的列表
```




    [[1, 2, 3], [4, 5, 6], [7, 8, 9]]



## 3.2 函数

可以有多个返回语句

也就是可以有多个return语句

如果没有，自动返回None



```python
def my_function(x, y, z=1.5):
    if z > 1:
        return z*(x + y)
    else:
        return z / (x+y)
    
my_function(5, 6, 0.7)
```




    0.06363636363636363




```python
my_function(10, 20)
```




    45.0




```python
my_function(x=5, y=6, z=7)
```




    77




```python
my_function(y=6, x=5, z=7)# 可以换位置
```




    77



### 3.2.1 局部变量与全局变量

直接按着书上的例子打我是懵的

![0304](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0304_yi.png)

还好群里的小伙伴又救了我

我们学习的课本一般都是先上结论，后给证明

而真正的学习过程是反着来的

都是先有现象和问题

后有试错和偏证

最后有结论和简洁完美的证明

我这里倾向于采用后者

所以我们来看几个情况


```python
# 情况1 外部不定义内部定义

def func():
    a_1 = [] 
    for i in range(5):
        a_1.append(i)
    print(a_1)
```


```python
func() ## 执行函数，打印内部变量
```

    [0, 1, 2, 3, 4]
    


```python
print(a_1) ## 执行函数后，打印外部变量
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-73-52e972c3959f> in <module>
    ----> 1 print(a_1)
    

    NameError: name 'a_1' is not defined


函数内部变量a_1

在函数调用时被创建

在函数退出时被销毁


```python
# 情况2
## 外部定义，内部不定义
## 函数内部直接对变量操作

a_2 = []
def func():
    for i in range(5):
        a_2.append(i)
    print(a_2)
    print(id(a_2))
    
    
func()
print(a_2)
print(id(a_2))
```

    [0, 1, 2, 3, 4]
    1835081160520
    [0, 1, 2, 3, 4]
    1835081160520
    

当函数内部内没有定义变量

函数内部的操作直接会

调用外部的变量进行操作


```python
# 情况3
## 外部内部都定义
## 变量名称虽然相同
## 但id地址不同
## 说明本质上不是同一个变量

a_3 = []
def func():
    a_3 = []
    for i in range(5):
        a_3.append(i)
    print(a_3)
    print(id(a_3))
    
    
func()
print(a_3)
print(id(a_3))
```

    [0, 1, 2, 3, 4]
    1847530358856
    []
    1847530359112
    

小结：函数内部操作是否可以改变函数外部变量，也就是全局变量

情况1 外部没有内部有：不能

情况2 内部没有外部有：可以

情况3 内部外部都有：不能

那么针对情况1、情况2，如果想改变外部变量怎么办？

这就引出了global声明


```python
# 情况1 + global

def func():
    global a_1_g
    a_1_g = 1
    print(a_1_g)
    print(id(a_1_g))

func()
print(a_1_g)
print(id(a_1_g))
```

    1
    140708479406912
    1
    140708479406912
    


```python
# 情况3 + global

a_3_g = 1

def func():
    global a_3_g
    a_3_g = 5
    print(a_3_g)
    print(id(a_3_g))

func()
print(a_3_g)
print(id(a_3_g))
```

    5
    140708479407040
    5
    140708479407040
    

#### 关于全局变量与局部变量的总结：

函数里自己定义了变量

- 属于函数内部局部变量

- 调用函数创建，退出函数销毁

函数里没有定义变量

- 会默认调用外部的变量

函数内部定义了变量，还想对应改变外部的全局变量

- 则需要用global声明

同时注意：变量是否是同一个变量，根本区别是在于id，而不是变量名

![0303](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0303_chenggong.jpg)

#### 从global反思类的意义

这本书的作者真的很好啊

他说这里只是简单讲一下global的用法

通常全局变量是用来储存系统中的某些状态

当你发现你在函数中需要大量使用global的时候

可能表面你需要面向对象编程（使用类）


也就是我们从这个角度可以反思一下

类与函数有什么本质上的区别

想想一旦调用一个类创建一个对象

实际上是一下子创建了好多的全局变量

调用一次创建一堆，调用一次创建一堆

函数就不行

这应该就是类这种概念存在的重要意义之一

### 3.2.2 返回多个值

这个看作者辣么开心的介绍这个功能

可见Java和C++应该都不能这么搞


```python
def f():
    a = 5
    b = 6
    c = 7
    return a, b, c #实质上是返回一个元组

a, b, c = f() #元组又被拆包
abc = f()

print(a, b, c)
print(abc)
```

    5 6 7
    (5, 6, 7)
    


```python
# 以此类推也可以返回成别的

def f():
    a = 5
    b = 6
    c = 7
    return {'a': a, 'b': b, 'c': c}' '

abc_d = f()
print(abc_d)
```

    {'a': 5, 'b': 6, 'c': 7}
    

### 3.2.3 函数是对象

这个也是Python的优点

这个作者是真的很喜欢Python啊

假设做一个数据清洗


```python
states = ['   ?alabama!  ', 'FlOrIda', 'south  carolina##', 'West virginia?']
```

感到凌乱？我脑袋嗡一下的好吧

取出空格，移除标点符号，调整适当的大小写

内建字符串

正则表达式re,regular expression

![0309](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0309_zheTMshishenme.jpg)


```python
import re

def clean_strings(strings):
    result = []
    for value in strings:
        print('0 : ' + value)
        value = value.strip() ##删除开头和结尾的空格换行
        print('1 strip: ' + value)
        value = re.sub('[!#?]', '', value) # 将！或# 或？替换为空，即删除
        print('2 re: '+ value)
        value = value.title()
        print('3 title: '+ value)
        result.append(value)
        
    return result
```


```python
clean_strings(states)
```

    0 :    ?alabama!  
    1 strip: ?alabama!
    2 re: alabama
    3 title: Alabama
    0 : FlOrIda
    1 strip: FlOrIda
    2 re: FlOrIda
    3 title: Florida
    0 : south  carolina##
    1 strip: south  carolina##
    2 re: south  carolina
    3 title: South  Carolina
    0 : West virginia?
    1 strip: West virginia?
    2 re: West virginia
    3 title: West Virginia
    




    ['Alabama', 'Florida', 'South  Carolina', 'West Virginia']



行吧，啥也憋说了，补一下正则表达式

https://www.runoob.com/python/python-reg-expressions.html

妈呀有点长，长也得掌握

![0308](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0308_wenzhu.png)
