
# 第03章 内建数据结构、函数及文件

这章也是Python，但操作都很6啊！

得认真过一遍了。


![0301](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0301_action.jpg)


## 3.1 数据结构和序列
### 3.1.1 元组

固定长度，不可变

原来外面的括号可以省略的是嘛，直接一个逗号分开就行了


```python
tup = 4, 5, 6
```


```python
tup
```




    (4, 5, 6)




```python
nested_up = (4, 5, 6),(7, 8)
```


```python
nested_up
```




    ((4, 5, 6), (7, 8))



tuple函数：将任意序列或迭代器转换为元组


```python
tuple([4, 0, 2])
```




    (4, 0, 2)




```python
tup = tuple('string')
```


```python
tup
```




    ('s', 't', 'r', 'i', 'n', 'g')



元组一旦创建，各个位置上的对象无法修改


```python
tup_5 = tuple(['foo', [1, 2], True])
```


```python
tup_5[2] = False
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-9-013d73566f3f> in <module>
    ----> 1 tup_5[2] = False
    

    TypeError: 'tuple' object does not support item assignment


如果元组中的一个对象是可变的，如列表，可以再列表内部进行修改


```python
tup_5[1].append(3)
```


```python
tup_5
```




    ('foo', [1, 2, 3], True)



可以使用 + 号来生成更长的元组


```python
(4, None, 'foo') + (6, 0) + ('bar',) # 这个最后的逗号必须有，不然报错
```




    (4, None, 'foo', 6, 0, 'bar')



元组 * 整数，生成含有多份的元组


```python
('foo', 'bar') * 4
```




    ('foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'bar')



列表说：俺也一样


```python
[1 , 2, 3] * 4
```




    [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]



#### 3.1.1.1 元组拆包


```python
tup_1 = (4, 5, 6)
```


```python
a, b, c = tup_1
```


```python
b
```




    5




```python
c
```




    6




```python
tup_2 = 4, 5, (6, 7) # 嵌套元组也可以拆包
```


```python
a, b, c = tup_2
```


```python
b
```




    5




```python
c
```




    (6, 7)



666啊~ 利用这个功能可以轻易地交换变量名


```python
a, b = 1, 2 # a b 分别从元组（1，2）中取得值
```


```python
a
```




    1




```python
b
```




    2




```python
b, a = a, b # a, b为一个元组，值为1，2，ba从里面取值
```


```python
a
```




    2




```python
b
```




    1




利用拆包遍历元组或列表组成的序列


```python
seq = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
```


```python
for a, b, c in seq:
    print('a={0}, b={1}, c={2}'.format(a, b, c))
```

    a=1, b=2, c=3
    a=4, b=5, c=6
    a=7, b=8, c=9
    

拆包工具*rest，从其起始位置取，rest是剩下的


```python
values = 1, 2, 3, 4, 5
```


```python
a, b, *rest = values
```


```python
a, b
```




    (1, 2)




```python
rest
```




    [3, 4, 5]



用下划线表示不想要的变量，

这样啊，之前在API取数据的时候见过

所以不用rest也行？？那你还叫什么特殊语法


```python
a, b, *_ = values
```


```python
a, b
```




    (1, 2)



![0302](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0302_haiyouzhezhongcaozuo.png)

#### 3.1.1.2 元组方法 count


```python
a = (1, 2, 3, 4, 3, 4)
```


```python
a.count(3) # 查a里有几个3
```




    2



### 3.1.2 列表

长度可变，内容可修改

[], 或list来定义，咦？！


```python
a_list = [2, 3, 7, None]
```


```python
tup_3 = ('foo', 'bar', 'baz')
```


```python
b_list = list(tup_3)
```


```python
b_list
```




    ['foo', 'bar', 'baz']



#### 3.1.2.2 链接和联合列表
+

extend 这个更快


```python
[4, None, 'foo'] + [7, 8, 9]
```




    [4, None, 'foo', 7, 8, 9]




```python
x = [4, None, 'foo']
```


```python
x.extend([7, 8, (2, 3)]) # 添加多个元素
```


```python
x
```




    [4, None, 'foo', 7, 8, (2, 3)]



#### 3.1.2.3 排序

艾玛，排序都有高级版

用key按字符串长度进行排序


```python
b = ['saw', 'small', 'he', 'foxes', 'six']
```


```python
b.sort(key=len)
```


```python
b
```




    ['he', 'saw', 'six', 'small', 'foxes']



#### 3.1.2.4 二分搜索和已排序列表的维护

bisect.bisect 按列表从大到小的顺序，看元素应当放到第几位

bisect.insort 把元素插到第几位

注意未排序的列表不会报错，但可能结果不正确


```python
import bisect
```


```python
c = [1, 2, 2, 2, 3, 4, 7]
```


```python
bisect.bisect(c, 2)
```




    4




```python
c
```




    [1, 2, 2, 2, 3, 4, 7]




```python
bisect.bisect(c, 5)
```




    6




```python
c
```




    [1, 2, 2, 2, 3, 4, 7]




```python
bisect.insort(c, 6)
```


```python
c
```




    [1, 2, 2, 2, 3, 4, 6, 7]



#### 3.1.2.5 切片

利用步长step对列表进行翻转


```python
seq = [1, 2, 5, 6, 3, 7, 8]
```


```python
seq[::-1]
```




    [8, 7, 3, 6, 5, 2, 1]



### 3.1.3 内建序列函数

#### 3.1.3.1 enumerate 列举 历数

就是把列表的索引和元素对应提出来


```python
abc_list = ['a', 'b', 'c']
mapping = {}

for i, v in enumerate(abc_list):
    mapping[i] = v
    
mapping
```




    {0: 'a', 1: 'b', 2: 'c'}



#### 3.1.3.2 sorted 排序

sorted的使用有刷新了我的认知


```python
sorted('horse race')
```




    [' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']



其根本是，把一个序列中的元素排序

#### 3.1.3.3 zip

也是，根本是把一个序列对应位置的元素组团

既然这么说了，我肯定要搞他一下，看看

![0305](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0305_dainifei.png)



```python
a = ['a0', 'a1', 'a2']
b = ['b0', 'b1', 'b2', 'b3']
c = ('c0', 'c1', 'c2')
d = '0123456'
e = {'e1':'v1', 'e2':'v2', 'e3':'v3'}
```


```python
ab = zip(a, b) ## 长度取短
ac = zip(a, c) ## 不限类型，只要可迭代应该就行
ad = zip(a, d) ## 字符串是可迭代类型
ae = zip(a, e) ## 字典也可以
```


```python
list(ab)
```




    [('a0', 'b0'), ('a1', 'b1'), ('a2', 'b2')]




```python
list(ac)
```




    [('a0', 'c0'), ('a1', 'c1'), ('a2', 'c2')]




```python
list(ad)
```




    [('a0', '0'), ('a1', '1'), ('a2', '2')]




```python
list(ae) ##不过字典不是无序得嘛
```




    [('a0', 'e1'), ('a1', 'e2'), ('a2', 'e3')]



zip经常要用到的情况

同时遍历多个序列，与enumerate同时使用


```python
seq1 = ['a', 'b', 'c']
seq2 = ['e', 'f', 'g']

for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
```

    0: a, e
    1: b, f
    2: c, g
    

zip(*列表)可以拆分，以前见过不说了

#### 3.1.3.4 reversed

元素倒序排列


```python
list(reversed(range(10)))
```




    [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]



### 3.1.4 字典

也叫哈希表或关联数组

字典大部分功能都见过了

这里补充几个

1. del和pop+key值

   注意pop之后返回的是value值



```python
a = {0: 'a', 1: 'b', 2: 'c'}
```


```python
del a[1]
```


```python
a
```




    {0: 'a', 2: 'c'}




```python
b = a.pop(0)
```


```python
a
```




    {2: 'c'}




```python
b
```




    'a'



2. keys和values

   提供字典key值和value值得迭代器
   
   字典key值没有特定顺序
   
   而迭代输出的key和value会按照相同个的顺序


```python
a = {0: 'a', 1: 'b', 2: 'c'}
```


```python
list(a.keys())
```




    [0, 1, 2]




```python
list(a.values())
```




    ['a', 'b', 'c']



3. update

   将两个字典合并
   
   注意key值相同会被覆盖


```python
a.update({3: 'd', 4: 'e', 0:'f'})
```


```python
a
```




    {0: 'f', 1: 'b', 2: 'c', 3: 'd', 4: 'e'}



#### 3.1.4.1 序列合成字典

常规操作


```python
key_list = [0, 1, 2]
value_list = ['a', 'b', 'c']
```


```python
mapping = {}
for key, value in zip(key_list, value_list):
    mapping[key] = value
```


```python
mapping
```




    {0: 'a', 1: 'b', 2: 'c'}



但是现在可以用dict直接来


```python
mapping_2 = dict(zip(key_list, value_list))
```


```python
mapping_2
```




    {0: 'a', 1: 'b', 2: 'c'}



这里因为dict里面元素的本质

是两个元素的集合

故可以字典可以接受

以元组为元素的列表

{0: 'a'} === [(0, 'a')]


```python
mapping_3 = dict(zip(range(5), reversed(range(5))))
```


```python
mapping_3
```




    {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}



#### 3.1.4.2 默认值：get pop setdefault

1. get pop

    测试一个key在不在字典里

    在就要这个值

    不在就返回默认值，比如'不在'

    常规做法



```python
a_2 = {0: 'a', 1: 'b', 2: 'c'}
```


```python
def check_key(key, some_dict, default_value):
    if key in some_dict:
        value = some_dict[key]
    else:
        value = default_value
    print(value)
```


```python
check_key(0, a_2, '不在呀')
```

    a
    


```python
check_key(3, a_2, '真没有')
```

    真没有
    


```python
c
```




    ('c0', 'c1', 'c2')



get和pop可以用一行代码替换这个if-else


```python
def get_check_key(some_dict, key, default_value):
    value = some_dict.get(key, default_value)
    print(value)
```


```python
get_check_key(a_2, 0, '没有')
```

    a
    


```python
get_check_key(a_2, 5, '这个真没有')
```

    这个真没有
    

2. setdefault

    一个列表有很多单词

    想把单词按首字母分组做成字典

    a对应所有a打头的单词

    b对应所有b打头的单词


```python
words = ['apple', 'bat', 'bar', 'atom','book']
```


```python
def key_in_dict(words):

    by_letter = {}
    for word in words: 
        ## 从列表里面取出一个词
        ## 如word = apple
        
        letter = word[0]
        ## apple的零位元素，就是a
        ## 注意字符串是可迭代的
        
        if letter not in by_letter:
            ## by_letter现在是空字典
            ## a 不在里面
            
            by_letter[letter] = [word]
            ## 字典里面加入{'a':['apple']}
            ## value是个列表，word是列表中的一个元素
            
        else:
            by_letter[letter].append(word)
            ## 循环到atom的时候，a在字典里了
            ## 所以执行else
            ## a的键对应的元素，用append往里加atom
            ## {'a':['apple', 'atom']}
        
    print(by_letter)
```


```python
key_in_dict(words)
```

    {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
    


```python
words_2 = ['apple', 'bat', 'bar']
```


```python
key_in_dict(words_2) 

## 用这个方法默认加入的value是一个列表
```

    {'a': ['apple'], 'b': ['bat', 'bar']}
    

setdefault就是实现上面这个功能




```python
words = ['apple', 'bat', 'bar', 'atom','book']
by_letter_2={}

for word in words:
    ## 列表，单词
    letter = word[0]
    ## 首字母
    by_letter_2.setdefault(letter, []).append(word)
    ## 字典.setdefaul(首字母，[]).append(单词)
    ## 记住这个模板

print(by_letter_2)
```

    {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
    

这么说是不是元素是元组也可以？

作死试一下


```python
by_letter_3 = {}
for word in words:
    letter = word[0]
    by_letter_3.setdefault(letter, ()).append(word)
    
print(by_letter_3)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-102-f2be3bf5e39f> in <module>
          2 for word in words:
          3     letter = word[0]
    ----> 4     by_letter_3.setdefault(letter, ()).append(word)
          5 
          6 print(by_letter_3)
    

    AttributeError: 'tuple' object has no attribute 'append'


参考：加油我是最胖的博客园 元组基本操作

https://www.cnblogs.com/cooled/p/8093218.html

元组不行，元组不支持增删改，只支持查



咦？那是不是支持增的就可以用？

那我试一下字符串

参考：字符串添加元素，append和+的区别

https://www.cnblogs.com/lordage/p/5700337.html


```python
words = ['apple', 'bat', 'bar', 'atom','book']
by_letter_4 = {}
for word in words:
    letter = word[0]
    by_letter_4.setdefault(letter, str).append(word)
    
print(by_letter_4)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-103-f32bfec54056> in <module>
          3 for word in words:
          4     letter = word[0]
    ----> 5     by_letter_4.setdefault(letter, str).append(word)
          6 
          7 print(by_letter_4)
    

    AttributeError: type object 'str' has no attribute 'append'


失败！

不死心，字典呢？

参考：增加删除字典元素

https://www.cnblogs.com/volcao/p/8695371.html


```python
words = ['apple', 'bat', 'bar', 'atom','book']
by_letter_5 = {}
for word in words:
    letter = word[0]
    by_letter_5.setdefault(letter, {}).update(hhha=word)
    
print(by_letter_5)
```

    {'a': {'hhha': 'atom'}, 'b': {'hhha': 'book'}}
    

成功！！太成功了


![0303](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0303_chenggong.jpg)



说明这个setdefault里面的东西理论上都是可以替换的

空列表，生成的元素，使用的方法

都可以换，这么强大嘛

只是用起来脑阔疼

我怀疑字符串也是可以的，只是我不会做

就这样吧 

不求甚解也许真不是个贬义词

额，我发现后面还有一个终极大boss



3. defaultdict

    这是内建的集合模块，的一个类



```python
from collections import defaultdict # 导入

by_letter_6 = defaultdict(list)
## 默认是个空字典？
## value设置成列表

for word in words:
    by_letter_6[word[0]].append(word)
    ## 空字典[key值].append(value列表的一个元素)
    
print(by_letter_6)
```

    defaultdict(<class 'list'>, {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']})
    

#### 3.1.4.3 有效的字典键类型

妈呀，第三章对我来说真是全程高能

字典，{key: value}

value可以值任何Python对象

key必须是不可变的对象

如int, float, str, tup

其中tup的对象也必须是不可变

为了检验一个东西是否能作为字典的键

引入hash函数，也叫是否能够哈希化



```python
hash('string')
```




    -3246335748821269447




```python
hash((1, 2, (2, 3)))
```




    1097636502276347782



输出的结果不仅与对象的值有关

也与对象的id地址，也就是存放的位置有关

不论如和，他是一个固定长度的数

只要不报错 应该就可以哈希化

https://www.runoob.com/python/python-func-hash.html


```python
hash((1, 2, [2, 3]))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-108-8ffc25aff872> in <module>
    ----> 1 hash((1, 2, [2, 3]))
    

    TypeError: unhashable type: 'list'


那想用列表得值做key

就得转换为元组


```python
d = {}
d[tuple([1, 2, 3])] = 5
d
```




    {(1, 2, 3): 5}



### 3.1.5 集合

集合也是一种装东西的容器

跟数学上的集合是一个概念

这就很简单了，记住集合就是两个圈

特点：无序、元素唯一

类似于没有key值得字典

创建：set 或 {}


```python
set([1, 2, 3, 4, 5, 3, 3])
```




    {1, 2, 3, 4, 5}




```python
{1, 2, 3, 4, 5, 3, 3}
```




    {1, 2, 3, 4, 5}



卧。。集合支持数学上的集合操作

有两种方式 看一下


```python
a = {1, 2, 3, 4, 5}
```


```python
b = {1, 3, 5, 7, 9}
```


```python
a.union(b) ## 并
```




    {1, 2, 3, 4, 5, 7, 9}




```python
a | b ## 并
```




    {1, 2, 3, 4, 5, 7, 9}




```python
a.intersection(b) ## 交
```




    {1, 3, 5}




```python
a & b ## 交
```




    {1, 3, 5}




```python
a.difference(b)## 在a不在b
```




    {2, 4}




```python
a - b ## 在a不在b
```




    {2, 4}




```python
a.symmetric_difference(b) ## 对称的意思，不同时在
```




    {2, 4, 7, 9}




```python
a ^ b ## 不同时在ab
```




    {2, 4, 7, 9}



等等，自己用到的时候再查

另外 符号后面有个等号

a |= b 这种形式，将ab的并集赋给a

名称也是在原来的后面加一个_update

书上推荐的格式

说是效率更高


```python
c = a.copy()
c.update(b)  ## 并集赋给c
c
```




    {1, 2, 3, 4, 5, 7, 9}




```python
c = a.copy()
c |= b  ## 并集赋给c
c
```




    {1, 2, 3, 4, 5, 7, 9}




```python
d = a.copy()
d.intersection_update(b)## 交并更新
d
```




    {1, 3, 5}




```python
d = a.copy()
d &= b ## 交并更新
d
```




    {1, 3, 5}




```python
e = a.copy()
e.difference_update(b) ## 在a不在b
e
```




    {2, 4}




```python
e = a.copy()
e -= b ## 在a不在b
e
```




    {2, 4}




```python
f = a.copy()
f.symmetric_difference_update(b) ## 不同时在ab
f
```




    {2, 4, 7, 9}




```python
f = a.copy()
f ^= b ## 不同时在ab
f
```




    {2, 4, 7, 9}



除了交并补这些操作

还有用来判断包含关系的语句


```python
a = {1, 2, 3, 4, 5}
b = {1, 3}
c = {9, 8}
```

集合相等，元素一样


```python
b == {3, 1} ## 所谓无序，顺序不同，元素相同就是相等
```




    True




```python
a.issubset(b) ## a是b的子集
```




    False




```python
b.issubset(a) ## b是a的子集
```




    True




```python
a.issuperset(b) ## a包含b
```




    True




```python
b.issuperset(a) ## b包含a
```




    False




```python
a.isdisjoint(b) ## ab无交集
```




    False




```python
a.isdisjoint(c) ## ac无交集
```




    True



点播一首你和我和他之间

![0304](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0304_nihewoheta.jpg)





爱你（づ￣3￣）づ╭❤～
