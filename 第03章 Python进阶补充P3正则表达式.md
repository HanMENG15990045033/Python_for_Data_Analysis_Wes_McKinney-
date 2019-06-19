
### 3.2.3.1 正则表达式补充

参考RUNOOB.COM:

https://www.runoob.com/python/python-reg-expressions.html

![0301](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0301_action.jpg)


折腾的目的：查找某一种字符串

- 比如看abc123里面有没有a

正则表达式：是一种格式

- 用特殊字符表达某种字符串
- 这是很多语言都通用的一种标准
- 很多，我们就先记几个
- \d，0-9的数字
- a*，一个或多个a，a，aa，aaaa都行
- [0-9],0-9的数字都行

re：是Python中的模块

- 使得在Python中可以使用所有的正则表达式规则




本节内容：

1.re.match 匹配开头

2.re.search 全文匹配

3.sub 替换删除

4.re.compile 编译正则

5.findall 返回列表

6.finditer 返回迭代器

7.re.split 分割返回列表

8.(?P...) 分组匹配

9.正则表达符号、修饰符

10.一个综合的例子

![0305](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0305_dainifei.png)

##### 1. re.match 匹配开头

功能：匹配开头，如果匹配返回某个值，如果不匹配返回None

通式：re.match(pattern, string, flag=0)，其中

- pattern：正则表达式如a，如\d代表0-9的数字

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

![0304](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0304_yi.png)

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

文档给的例子简直想砍人，最最后看

![0311](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0311_laozigeiniyiquan.jpg)

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

![0311](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0311_laozigeiniyiquan.jpg)

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
    

##### 5. findall 返回列表

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

功能：分组匹配，一对儿值

通式：((?P 《key》 \pattern) 得到一组对应的值，key：匹配的字符

使用groupdict函数可以变成字典



```python
import re

s = '1102231990xxxxxxxx'

res = re.search('(?P<province>\d{3})(?P<city>\d{3})(?P<born_year>\d{4})',s)

print(res.groupdict())
```

    {'province': '110', 'city': '223', 'born_year': '1990'}
    

再说之前那个例子

？P+group+repl函数

再循环里加几个print看看每步都发生了什么


```python
import re
 

def double(matched):
    
    print(matched) ## 匹配的字符属性
    
    v_d = matched.groupdict('key') ## 试一下匹配的字符用groupdict变成字典格式
    print(v_d) ## 说明得到的是一对儿数，如 key：23
    
    value = int(matched.group('key')) ##  匹配的字符用group（’value’）提出value值
    print(value) ## 匹配的字符串'23'
    
    return str(value * 2) ## 返回 23*2=46，将23替换为46
 
s = 'A23G4HFD567'

s_2 = re.sub('(?P<key>\d+)', double, s) 
## 匹配一个以上的数字如23
## 替换为经double函数处理得到的东西
## 要处理的字符串是s

print(s_2)
```

    <re.Match object; span=(1, 3), match='23'>
    {'key': '23'}
    23
    <re.Match object; span=(4, 5), match='4'>
    {'key': '4'}
    4
    <re.Match object; span=(8, 11), match='567'>
    {'key': '567'}
    567
    A46G8HFD1134
    

OK？先这样。。。

![0310](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0310_aini.jpg)

萌混过关~


##### 9. 正则符号、修饰符

###### Metacharacters Defined 通配符

参考：http://regexlib.com/CheatSheet.aspx

参考：https://baike.baidu.com/item/正则表达式/1700215?fr=aladdin



```python
# 通配符1 ^：Start of a string 字符串的开头

import re

list_start_with = ['abcd','abc123','123abc','efgabc','abcdefg']

for value in list_start_with:
    
    index_start_with=re.search('^abc',value)
    if index_start_with:
        print(value)
```

    abcd
    abc123
    abcdefg
    


```python
# 通配符2 $：End of a string 字符串的结尾

import re

list_end_with = ['abcd','1abc123','123abc','efgabc','defgabc']

for value in list_end_with:
    
    index_end_with = re.search('abc$',value)
    if index_end_with:
        print(value)
```

    123abc
    efgabc
    defgabc
    


```python
# 通配符3 .：Any character (except \n newline) 任何字符，除了换行

import re

list_any_character = ['abcd','afc','123abc','aec','a1c']

for value in list_any_character:
    
    index_any_character = re.search('a.c',value)
    if index_any_character:
        print(value)
```

    abcd
    afc
    123abc
    aec
    a1c
    


```python
# 通配符4 |：Alternation 或

import re

list_alternation = ['abd','afc','123abc','aec','a1c','12345']

for value in list_alternation:
    
    index_alternation = re.search('a|1',value)
    if index_alternation:
        print(value)
```

    abd
    afc
    123abc
    aec
    a1c
    12345
    


```python
# 通配符5 {}：Explicit quantifier notation 明确数量

import re

list_quantifier = ['abd','abbcfc','123abbbbc','abbec','a1c','12345abbccbb']

for value in list_quantifier:
    
    index_quantifier = re.search('ab{2}c',value)
    if index_quantifier:
        print(value)
```

    abbcfc
    12345abbccbb
    


```python
# 通配符6 []：Explicit set of characters to match 一系列符号

import re

list_set = ['Alien','alien','aLien','aliEn']

for value in list_set:
    
    index_set = re.search('[Aa][Ll]ien',value)
    if index_set:
        print(value)
```

    Alien
    alien
    aLien
    


```python
# 通配符7 ()：Logical grouping of part of an expression 一个表达部分的逻辑分组

import re

list_grouping = ['abcabc','123abcabca','abc123',]

for value in list_grouping:
    
    index_grouping = re.search('(abc){2}',value)
    if index_grouping:
        print(value)
```

    abcabc
    123abcabca
    


```python
# 通配符8 * ：0 or more of previous expression 0个或多个前面的字符，贪婪模式
# 通配符9 +：1 or more of previous expression 1个或多个前面的字符

import re

list_more = ['acd','abbb','123abbbb','123abb123']

print('ab*：')
for value in list_more:
    index_more_1 = re.search('ab*',value)    
    if index_more_1:
        print(value)

print('\nab+：')        
for value in list_more:    
    index_more_2 = re.search('ab+',value)    
    if index_more_2:
        print(value)


```

    ab*：
    acd
    abbb
    123abbbb
    123abb123
    
    ab+：
    abbb
    123abbbb
    123abb123
    


```python
# 通配符10 ？：0 or 1 of previous expression 0个或多个前面的字符
# also forces minimal matching when an expression might match several strings within a search string.
# 非贪婪模式
import re

list_more = ['acd','abbb','123abbbb','123abb123']

print('ab*贪婪模式：')
for value in list_more:
    index_more_1 = re.search('ab*',value)    
    if index_more_1:
        print(index_more_1.group(0))

print('\nab*?非贪婪模式：')        
for value in list_more:    
    index_more_3 = re.search('ab*?',value)    
    if index_more_3:
        print(index_more_3.group(0))        

```

    ab*贪婪模式：
    a
    abbb
    abbbb
    abb
    
    ab*?非贪婪模式：
    a
    a
    a
    a
    


```python
# 通配符11 \：Preceding one of the above, it makes it a literal instead of a special character. 
# 让上面的符号变为简单的字符，而不是特殊的功能符号
# Preceding a special matching character, see below
# 在一个特殊的匹配符号前，之后再说

import re

list_grouping = ['abc?123','abccc']

for value in list_grouping:  
    index_grouping = re.search('abc\?',value)
    if index_grouping:
        print(value)
```

    abc?123
    

内容有点多

![0306](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0306.png)


###### 字符组


[^aeiou]：

Matches any single character not in the specified set of characters.

匹配不在这里的单个字母

除了a e i o u的其他字母


```python
import re

list_not_in_set = ['abc?123','book','change','aaaa']

for value in list_not_in_set:  
    index_not_in_set = re.search('[^a]',value)
    if index_not_in_set:
        print(index_not_in_set.group(0))
```

    b
    b
    c
    

[0-9a-fA-F]

Use of a hyphen (–) allows specification of contiguous character ranges.

用 - 可以指明一个连续的字符范围




```python
import re

list_hyphen = ['0ab','1cd','3ab','0de','abc12abc']

for value in list_hyphen:  
    index_hyphen = re.search('[0-2][a-c]',value)
    if index_hyphen:
        print(index_hyphen.group(0))
```

    0a
    1c
    2a
    

\d 

Matches any decimal digit. 

十进制数字，相当于[0-9]，其他进制不行


```python
import re

list_decimal_digit = ['0ab','1cd','abc','\042','\x12']

for value in list_decimal_digit:  
    index_decimal_digit = re.search('\d',value)
    if index_decimal_digit:
        print(index_decimal_digit.group(0))
```

    0
    1
    


```python
import re

list_decimal_digit_2 = ['0ab','1cd','abc','\040987','\x12']
## \040987  按照贪婪法  分为 \040   9 8 7 

for value in list_decimal_digit_2:
    index_decimal_digit_2 = re.search('\d',value)
    if index_decimal_digit_2:
        print(index_decimal_digit_2.group(0))

## '\040789' 是一个字符串吧
## 按照贪婪法 \040 才是一个有效的转义字符   
## \04不够贪婪  毕竟\040 也有效    
## \0407  贪婪过度  转义的八进制最多只能三位
```

    0
    1
    9
    

\D 

Matches any nondigit.

非数字，相当于[^0-9]，其他进制也不包括


```python
import re

list_nondigit = ['0ab','1cd','12']

for value in list_nondigit:  
    index_nondigit = re.search('\D',value)
    if index_nondigit:
        print(index_nondigit.group(0))
```

    a
    c
    

\w

Matches any word character. 

任何词汇的字符，相当于[a-zA-Z_0-9]


```python
import re

list_any_word = ['abc','123','ABC','!#','*&']

for value in list_any_word:  
    index_any_word = re.search('\w',value)
    if index_any_word:
        print(index_any_word.group(0))
```

    a
    1
    A
    

\W

Matches any nonword character. 

非词汇字符，相当于[^a-zA-Z_0-9].


```python
import re

list_any_nonword = ['abc','123','ABC','!#','*&']

for value in list_any_nonword:  
    index_any_nonword = re.search('\W',value)
    if index_any_nonword:
        print(index_any_nonword.group(0))
```

    !
    *
    

\s

Matches any white-space character. 

所有的空白字符换页，换行，回车，Tab，纵向Tab

相当于[ \f\n\r\t\v].


```python
import re

list_white_space = ['ab  c','12\n3','A\tBC','!#\f？','*\v&']

for value in list_white_space:  
    index_white_space = re.search('\s',value)
    if index_white_space:
        print(value)
```

    ab  c
    12
    3
    A	BC
    !#？
    *&
    

\S

Matches any non-white-space character. 

所有的非空白字符，相当于[^ \f\n\r\t\v].


```python
import re

list_non_white_space = ['   ','\n','\t','\f','\v','abc\n']

for value in list_non_white_space:  
    index_non_white_space = re.search('\S',value)
    if index_non_white_space:
        print(value)
```

    abc
    
    

\p{name} 

扩展子集（元字符）

https://www.cnblogs.com/lcf/articles/807523.html

我的天哪。。。果断跳过

![0302](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0302_ruoxiaokelian.jpg)

\P{name}

Matches text not included in groups and block ranges specified in {name}.

不符合名字格式的

###### 修饰符

关于flag，修饰符，共6个

re.I 不考虑大小写

re.L 本地化识别匹配？？

re.M 多行匹配，影响^he $

re.S 影响.,包含换行符

re.U 根据Unicode字符解析，影响\w,\W,\b,\B

re.X ....跟利于理解



##### 10. 综合例子

参考文档里的的例子，真的挺难的


```python
import re

line = "Cats are smarter than dogs"

matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I) 

## re.M：多行匹配
## re.I：不考虑大小写

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    # 整体匹配的字符串：什么 空格are空格 什么 空格 什么
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
    


```python
# 试一下去掉group(2)后面的空格

import re

line = "Cats are smarter than dogs"

matchObj = re.match( r'(.*?) are (.*?).*', line, re.M|re.I)
## 这里从are空格到 .* 之间的东西给（.*？）
## 而.*取了smarter than dogs
## 所以（.*？）是个空
## 即group（2）是个空

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    print("matchObj.group(1) : ", matchObj.group(1))
    print("matchObj.group(2) : ", matchObj.group(2))
else:
    print ("No match!!")
```

    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  
    


```python
# 为了更清楚的看到group取值的顺序，我再改一下

import re

matchObj = re.match( r'(.*?) are (.*?)(.*)d(.*)', "Cats are smarter than dogs")

if matchObj:
    
    print("matchObj.group() : ", matchObj.group())
    print("matchObj.group(1) : ", matchObj.group(1))
    print("matchObj.group(2) : ", matchObj.group(2))
    ## 匹配are空格和group（3）之间的东西
    print("matchObj.group(3) : ", matchObj.group(3))
    ## group（3）匹配are空格are和d之间的东西，即smater than空格
    ## 所以group（2）就是空
    print("matchObj.group(4) : ", matchObj.group(4))
    ## group（4）匹配d之后的，即ogs

```

    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  
    matchObj.group(3) :  smarter than 
    matchObj.group(4) :  ogs
    

百度里还有很多实际应用的模板，先这样吧

太不易了

![0209](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0207_end.jpg)
