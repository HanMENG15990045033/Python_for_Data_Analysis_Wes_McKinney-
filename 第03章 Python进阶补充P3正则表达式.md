
### 3.2.3.1 正则表达式补充

参考：

https://www.runoob.com/python/python-reg-expressions.html



折腾的目的：查找某一种字符串

- 比如看abc123里面有没有a

正则表达式：是一种格式

- 用特殊字符表达某种字符串
- 比如看abc123查有没有数字，就用\d表示
- 这是很多语言都通用的一种标准

re：是Python中的模块

- 使得在Python中可以使用所有的正则表达式规则

正则表达式对象

- http://regexlib.com/CheatSheet.aspx

正则表达式修饰符flag很简单

关于flag，修饰符，共6个

- re.I 不考虑大小写

- re.L 本地化识别匹配？？

- re.M 多行匹配，影响^he $

- re.S 影响.,包含换行符

- re.U 根据Unicode字符解析，影响\w,\W,\b,\B

- re.X ....跟利于理解


直接看函数共8点加两个例子


##### 1. re.match 匹配开头

功能：匹配开头，如果匹配返回某个值，如果不匹配返回None

通式：re.match(pattern, string, flag=0)，其中

- pattern：正则表达式如a\d

- string：要匹配的字符串如abc123

- flags：标志位，设置匹配方式，如是否区分大小写等


```python
import re

print(re.match('a', 'abc123'))
## a时abc123的开头，返回一个值
## 这个值具体是什么等会儿再说，反正不是None

print(re.match('b', 'abc123'))
## b不是abc123的开头，返回None
```

    <re.Match object; span=(0, 1), match='a'>
    None
    

好了现在的问题就是如果匹配，我们返回什么值呢？

N先生补充：

start() 返回匹配开始的位置

end() 返回匹配结束的位置

span() 返回一个元组包含匹配 (开始,结束) 的位置，span就是范围的意思life span寿命

group() 返回被 RE 匹配的字符串
    


```python
## 返回值取什么，看N先生的例子

import re

index=re.match('what','whatff i whatfffff')

if index:
    print(index.start()) ## 返回起始位置
    print(index.end()) ## 返回结束位置3+1 = 不匹配的f开始位置4
    print(index.span()) ## 返回（起始，结束）
    print(index.group(0))## 返回字符串

```

    0
    4
    (0, 4)
    what
    

那么问题又来了，这个group我可看见个数，写着0

有什么含义？

文档给的例子简直想砍人，最后看

还是N先生给的例子好
    


```python
# 关于group（）用N先生的例子更好

import re

a = "123abc456"

rgl_exprs = '([0-9]*)([a-z]*)([0-9]*)'

# 正则表达式，从左到右，有几个括号，就是几组
# group（0）：如有匹配，返回字符串整体
# group（1）：1开始，0到9的数字，取到1，*再来取到2，*再来取到3，*再来a不能取
# group（2）：a开始，a到z的字母，取到a，*再来取到b，*再来取到c，*再来4不能取
# group（3）：同理
# group（4）：没有定义会报错

print(re.match(rgl_exprs, a).group(0))  
print(re.match(rgl_exprs, a).group(1))  
print(re.match(rgl_exprs, a).group(2))
print(re.match(rgl_exprs, a).group(3)) 
print(re.match(rgl_exprs, a).group(4)) 
```

    123abc456
    123
    abc
    456
    


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-83-792069b91009> in <module>
         16 print(re.match(rgl_exprs, a).group(2))
         17 print(re.match(rgl_exprs, a).group(3))
    ---> 18 print(re.match(rgl_exprs, a).group(4))
    

    IndexError: no such group



```python
# 想知道group（）一共定义了几组

import re

a = "123abc456"
rgl_exprs = '([0-9]*)([a-z]*)([0-9]*)'

print(re.match(rgl_exprs, a).lastindex)
```

    3
    

##### 2. re.search 全文匹配

功能：扫描整个字符串，匹配成功，返回第一个匹配成功的对象，否则返回None

通式：re.search(pattern, string, flags=0)


```python
import re

print(re.search('www', 'www.runoob.com').start())
print(re.match('www', 'www.runoob.com').start()) # 与match对比
print(re.search('com', 'www.runoob.com').span())
print(re.match('com', 'www.runoob.com')) # 与match的区别
```

    0
    0
    (11, 14)
    None
    

##### 3. sub 替换删除

功能：substitude缩写，替换匹配项，用空去替换，那就是删除

通式：re.sub(pattern, repl, string, count=0, flags=0)

repl : 替换的字符串，也可为一个函数

count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配



```python
import re

s_sub = "123 abc 456 456 456" # string字符串
p_sub = '456' # pattern 匹配的字符串
r_sub = '789' # replace替换的

s_subed = re.sub(p_sub, r_sub, s_sub, count=1, flags=0)
print("count = 1:", s_subed)
# count = 1 匹配后替换一次

s_subed_ed = re.sub(p_sub, r_sub, s_sub, count=0, flags=0)
print("count = 0:", s_subed_ed)
# count = 0 匹配后替换次数不限

print(s_subed_ed)
```

    count = 1: 123 abc 789 456 456
    count = 0: 123 abc 789 789 789
    123 abc 789 789 789
    

其中repl可以为函数

看这个文档里的例子。。



```python
import re
 
# 将匹配的数字乘以 2


def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)
 
s = 'A23G4HFD567'
s_2 = re.sub('(?P<value>\d+)', double, s)

print(s_2)

```

    A46G8HFD1134
    

。。。。想打人怎么办，在线等，挺急的

最后说这个例子

咱先看个简单的


```python
import re
 
# 将匹配的数字乘以 2
def double(x):
    value = int(x.group())
    return str(value * 2)
 
s = '12'
print(re.sub('\d', double, s))
```

    24
    

虽然例子比较傻，但是可以直观的看出这个函数的功能

##### 4. re.compile 编译正则

功能：编译正则表达式，生成一个pattern，供 match() 和 search() 使用

通式：re.compile(pattern[, flags])


```python
import re

pattern = re.compile(r'\d+') # 1或多个数字

m = pattern.match('one12twothree34four')  # 查找头部，没有匹配
n = pattern.search('one12twothree34four').group(0)

print(m)
print(n)

```

    None
    12
    


```python
m_2 = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配

print(m_2)

m_3 = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配

print(m_3)  

print(m_3.group())
```

    None
    <re.Match object; span=(3, 5), match='12'>
    12
    

##### 5.findall 返回列表

功能：全字符串找，匹配，并返回一个列表，否则返回空列表。

通式：findall(string[, pos[, endpos]])

- string : 待匹配的字符串。 
    
- pos : 可选参数，指定字符串的起始位置，默认为 0。 
    
- endpos : 可选参数，指定字符串的结束位置，默认为字符串的长度


```python
import re
 
p_findll = re.compile(r'\d+')   # 查找数字
result1 = p_findll.findall('123abc456')
# 找数字，返回一个列表
result2 = p_findll.findall('123abc456', 3, 8)
# 从3位开始，包括a，从8位结束，不包括6
 
print(result1)
print(result2)
```

    ['123', '456']
    ['45']
    

##### 6. finditer 返回迭代器

功能：类似findall，只不过返回迭代器

通式：re.finditer(pattern, string, flags=0)



```python
import re
 
it = re.finditer(r"\d+","123abc456efg789") 

for match in it: 
    print (match.group())
```

    123
    456
    789
    

##### 7. re.split 分割返回列表

功能：按照能够匹配的子串将字符串分割后返回列表、

通式：re.split(pattern, string [, maxsplit=0, flags=0])

maxsplit：分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数


```python
re.split('\W+', '，runoob, runoob,    runoob.')

# \W非字母数字及下划线
# 也就是字母数字下划线留着
# ，空格不能要，见到分隔
# .也不能要，见到分隔
# 分隔一次，列表里就有一个元素，就有一个，
# 所以开头结尾都有个空
```




    ['', 'runoob', 'runoob', 'runoob', '']



##### 8.  (?P...) 分组匹配

功能：分组匹配，返回字典

通式：((?P<key>\pattern)>>> {key：pattern_s}




```python
import re

s = '1102231990xxxxxxxx'

res = re.search('(?P<province>\d{3})(?P<city>\d{3})(?P<born_year>\d{4})',s)

print(res.groupdict())
```

    {'province': '110', 'city': '223', 'born_year': '1990'}
    

再说之前那个例子

？P+group+repl函数


```python
import re
 

def double(matched):
    print(matched)
    value = int(matched.group('value'))
    print(value)
    v_d = matched.groupdict('value')
    print(v_d)
    return str(value * 2)
 
s = 'A23G4HFD567'
s_2 = re.sub('(?P<value>\d+)', double, s)

print(s_2)
```

    <re.Match object; span=(1, 3), match='23'>
    23
    {'value': '23'}
    <re.Match object; span=(4, 5), match='4'>
    4
    {'value': '4'}
    <re.Match object; span=(8, 11), match='567'>
    567
    {'value': '567'}
    A46G8HFD1134
    


```python
import re

def double(mat):
    value = int(mat.group('id')) 
    print(value)
    return str(value * 2)  

s = 'A23G4HFD567' 
print(re.sub('(?P<id>\d+)', double, s)) 
```

    23
    4
    567
    A46G8HFD1134
    

OK？先这样。。。

看一个综合的例子


```python
# 这个算综合题，放到最后
# 关于这个group里面的数
# 看个稍微复杂点的例子

import re

line = "Cats are smarter than dogs"

matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    print("matchObj.group(1) : ", matchObj.group(1))
    # 从开头C到空格are空格，取group（1）
    print("matchObj.group(2) : ", matchObj.group(2))
    # 从are空格之后，也就是s到空格.*取group（2）
    # 空格.* 是 空格than dogs
    # 所以group（2）= smarter
else:
    print ("No match!!")
```

    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  smarter
    

函数：re.match()

正则表达式pattern：

    r：见过，把反斜杠看成是普通的斜杠，而不是转义符号
    
    .：匹配任意字符，除了换行符，一个
    
    *：匹配多个
    
    .*：匹配多个任意，也就是所有字符，除了换行符，贪婪模式，读到最长的停
    
    re?：非贪婪模式，从左到右读，读到符合条件的就停
    
    .*?：除了换行符，多个任意，非贪婪模式
    
    没加括号的.*：跟第一个一样，只是结果不计入group中

flags:
    
    re.M：多行匹配
    
    re.I：不考虑大小写
    
group()：

matchObj.group() 等同于 matchObj.group(0)，表示匹配到的完整文本字符 

matchObj.group(1) 得到第一组匹配结果，也就是(.*)匹配到的 

matchObj.group(2) 得到第二组匹配结果，也就是(.*?)匹配到的 


```python
# 不加括号的.*，去掉试试

import re

line = "Cats are smarter than dogs"

matchObj = re.match( r'(.*?) are (.*?)d.*', line, re.M|re.I)

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    print("matchObj.group(1) : ", matchObj.group(1))
    print("matchObj.group(2) : ", matchObj.group(2))
else:
    print ("No match!!")
```

    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  smarter than 
    


```python
import re

matchObj = re.match( r'(.*?) are (.*?)(.*)(.*)d(.*)', "Cats are smarter than dogs")

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    print("matchObj.group(1) : ", matchObj.group(1))
    print("matchObj.group(2) : ", matchObj.group(2))
    print("matchObj.group(3) : ", matchObj.group(3))
    print("matchObj.group(4) : ", matchObj.group(4))
    print("matchObj.group(5) : ", matchObj.group(5))
```

    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  
    matchObj.group(3) :  smarter than 
    matchObj.group(4) :  
    matchObj.group(5) :  ogs
    




