
# 第2章 Python语言基础

过一遍还是有好处的，只挑一下对初学者可能有帮助的部分。

## 2.2 IPython

所有关于IPython的先不看，就看先用这个jupyter

### 2.2.4 内省：？

?：显示信息

?：函数的话，显示文档字符串

   就是函数的注释，三个双引号那个

??：显示函数源代码

星号 名称 星号?: 显示NumPy顶层函数中包含load的函数列表



```python
b = [1, 2, 3]
```


```python
b?
```

![0201](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0201.PNG)


```python
print?
```

![0202](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0202print.PNG)


```python
def add_numbers(a, b):
    """
    Add two numbers together ## 把两个数字加到一起
    Returns ## 返回的值为
    ------
    the_sum : type of arguments ## 加和，类型为输入变量的类型
    """
    return a + b
    
```


```python
add_numbers?
```

![0203](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0203add_numbers.PNG)


```python
add_numbers??
```

![0204](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0204add_number_2.PNG)


```python
import numpy as np
np.*load*?
```

![0205](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0205load.PNG)



## 2.3 Python语言基础

相对于《Python编程从入门到实践》有一定的拔高

挑几个补充点
#### 2.3.1.6 isinstance检查对象的类型


```python
a = 5; b = 4.5
```


```python
isinstance(a, int)
```




    True




```python
isinstance(b, float)
```




    True




```python
isinstance(a, (int, float))
```




    True



#### 2.1.3.7 属性和方法


```python
a = 'foo'
```


```python
a.capitalize
```




    <function str.capitalize()>




```python
a.format
```




    <function str.format>




```python
a.isupper
```




    <function str.isupper()>




```python
getattr(a, 'split')
```




    <function str.split(sep=None, maxsplit=-1)>



#### 2.3.1.8 验证是否可迭代


```python
def isiterable(obj):
    try:
        iter(obj)
        return True
    except TypeError: ## 类型错误，不可遍历
        return False
```


```python
isiterable('a string')
```




    True




```python
isiterable([1, 2, 3])
```




    True




```python
isiterable(5)
```




    False



#### 2.3.2.1 数值类型
整型和浮点型

整型可以储存任意大小的数字


```python
ival = 5211314
```


```python
ival**10
```




    14773126520894576247394034909454218428871028979977065385182498046976



浮点型float，每个浮点数都是双精度64位数,啥意思？？


```python
faval = 3.1415926
```

可以用科学计数法表示


```python
faval = 12345.67e-5
```


```python
faval+1
```




    1.1234567




```python
a = 123456.7e-6
```


```python
a+1
```




    1.1234567



e-7,就是十的负七次方

整数除法，自动转型为浮点数


```python
3 / 2
```




    1.5



如果想整数除完还是整数，C语言就是那样，可以用//


```python
3//2
```




    1



#### 2.3.2.2 字符串

多行字符串，用三引号


```python
c = """
一顿火锅
两串烧烤
三杯阔落
四盆龙虾
五香花生
六味猪蹄
七斤荔枝
八个榴莲
九足饭饱
食不我待，得抓紧
"""
```

这个字符串，包含了十行文本，换行符\n也是包含在其中的

试着用count计算回车符


```python
c.count('\n')
```




    11



不过这个count出来是11，好吧就算你11行文本

##### 下面说以下这个反斜杠，\

1. 反斜杠作为转义符号

  参考苦行僧95的博客园：

  https://www.cnblogs.com/kuxingseng95/p/9462294.html
  
  
  目前见过的\', \t, \n


```python
print('I\'am a good boy')
```

    I'am a good boy
    


```python
s = '\n'
print(s)
```

    
    
    

可以看出反斜杠是有特殊作用的

那么杠一个普通的内容会怎么样


```python
s = '\a'
print(s)
```

    
    

我不知道发生了啥，反正没报错也没打印东西


```python
s = '\ab'
print(s)
```

    b
    


```python
s = 'b\a'
print(s)
```

    b
    


```python
s = '\'
print(s)
```


      File "<ipython-input-37-036df140a79b>", line 1
        s = '\'
               ^
    SyntaxError: EOL while scanning string literal
    



```python
s = '\\'
print(s)
```

    \
    


```python
s = '\\\'
print(s)
```


      File "<ipython-input-39-b985fd4a0675>", line 1
        s = '\\\'
                 ^
    SyntaxError: EOL while scanning string literal
    



```python
s = '\\\\'
print(s)
```

    \\
    

有位N先生，

指点我说这个应该是“贪心法”

每个字符应该包含更多的字符。

编译器将程序分解成符号的方法是，

从左到右一个字符一个字符的读入，

如果该字符可能组成一个符号，

就再读入下一个字符，

判断已经读入的两个字符组成的字符串

是否可能是一个符号的组成部分；

如果可能，继续读入下一个字符，

重复上述判断，

直到读入的字符组成的字符串

不再可能组成一个有意义的符号。

这种处理方法，又称为“贪心法”，或者“大嘴法””。


```python
s = '\\\a'
print(s)
```

    \
    

2. 再说一下杠数字的问题


```python
s = '12\\24'
print(s)
```

    12\24
    


```python
s = '\24'
print(s)
```

    
    


```python
s ## 注意这种形式是查看变量本身
```




    '\x14'



发生了什么。。

还是群里的N先生厉害

指点我说

这里jupyter似乎默认做了一个进制转化


\24 是八进制 \024，0 省略了    

\x14是16进制       

2*8 + 4 = 1 * 16 +4 = 20

也就是

八进制的24=十六进制的14=十进制的20

![0206](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0206_01_02.PNG)

```python
s = '12\\\24'
print(s)
```

    12\
    


```python
s
```




    '12\\\x14'



但是问题又来了

为什么会默认转换呢

十六进制八进制又是怎么回事？


```python
a = 01 + 024
a
```


      File "<ipython-input-47-226217199005>", line 1
        a = 01 + 024
             ^
    SyntaxError: invalid token
    


以0开头的数字是八进制，3.7会报错，2.7可以运行


不管了先这样，那么想要轻松打出\

就要使用r'

raw表示原生的，肉，生肉


```python
s = r'人\生\不\易\\且\吃\且\珍\惜'
```


```python
s ## 注意这是在查看s本身
```




    '人\\生\\不\\易\\\\且\\吃\\且\\珍\\惜'




```python
print(s)
```

    人\生\不\易\\且\吃\且\珍\惜
    

#### 字节与Unicode

字符串，字节与Unicode的转换

咱也不知道咋回事儿，咱也不敢问呀

跟着做吧


```python
val = 'Joyeux Noël'
```


```python
val
```




    'Joyeux Noël'



endode将Unicode字符串转换为UTF-8字符


```python
val_utf8 = val.encode('utf-8')
```


```python
val_utf8
```




    b'Joyeux No\xc3\xabl'



问题在于这个特殊字符ë

如果这样，汉字岂不也是？？我试试


```python
val = '''
茕茕孑立
沆瀣一气
踽踽(jǔ)独行
醍醐灌顶
绵绵瓜瓞(dié)
奉为圭(guī)臬(niè)
龙行龘龘(dá)
犄(jī)角旮(gā)旯(lá)
'''
```


```python
val
```




    '\n茕茕孑立\n沆瀣一气\n踽踽(jǔ)独行\n醍醐灌顶\n绵绵瓜瓞(dié)\n奉为圭(guī)臬(niè)\n龙行龘龘(dá)\n犄(jī)角旮(gā)旯(lá)\n'



难为他一下


```python
val_utf8 = val.encode('utf-8')
```


```python
val_utf8
```




    b'\n\xe8\x8c\x95\xe8\x8c\x95\xe5\xad\x91\xe7\xab\x8b\n\xe6\xb2\x86\xe7\x80\xa3\xe4\xb8\x80\xe6\xb0\x94\n\xe8\xb8\xbd\xe8\xb8\xbd(j\xc7\x94)\xe7\x8b\xac\xe8\xa1\x8c\n\xe9\x86\x8d\xe9\x86\x90\xe7\x81\x8c\xe9\xa1\xb6\n\xe7\xbb\xb5\xe7\xbb\xb5\xe7\x93\x9c\xe7\x93\x9e(di\xc3\xa9)\n\xe5\xa5\x89\xe4\xb8\xba\xe5\x9c\xad(gu\xc4\xab)\xe8\x87\xac(ni\xc3\xa8)\n\xe9\xbe\x99\xe8\xa1\x8c\xe9\xbe\x98\xe9\xbe\x98(d\xc3\xa1)\n\xe7\x8a\x84(j\xc4\xab)\xe8\xa7\x92\xe6\x97\xae(g\xc4\x81)\xe6\x97\xaf(l\xc3\xa1)\n'




```python
type(val_utf8)
```




    bytes




```python
type(val)
```




    str



类型嘛

decode，对Unicode解码


```python
val_utf8.decode('utf-8')
```




    '\n茕茕孑立\n沆瀣一气\n踽踽(jǔ)独行\n醍醐灌顶\n绵绵瓜瓞(dié)\n奉为圭(guī)臬(niè)\n龙行龘龘(dá)\n犄(jī)角旮(gā)旯(lá)\n'



用b来定义字符文本为byte类型


```python
bytes_val = b'破马张飞'
```


      File "<ipython-input-62-6ecc638cc6eb>", line 1
        bytes_val = b'破马张飞'
                   ^
    SyntaxError: bytes can only contain ASCII literal characters.
    



```python
bytes_val = b'po ma zhang fei'
```


```python
bytes_val
```




    b'po ma zhang fei'




```python
decoded = bytes_val.decode('utf8')
```


```python
decoded
```




    'po ma zhang fei'




```python
type(decoded)
```




    str



#### 2.3.2.5 类型转换
str、bool、int、float


```python
s = '3.1415926'
```


```python
fs = float(s)  ## 字符串也可以直接转换为浮点型的嘛
```


```python
type(s)
```




    str




```python
type(fs)
```




    float




```python
fs
```




    3.1415926




```python
int(fs)
```




    3




```python
bool(fs)
```




    True




```python
bool(0) ## 0的布尔值是False
```




    False




```python
bool(s) ## 字符串的布尔值也是true
```




    True




```python
bool('0') ## 字符串是0也是true
```




    True




```python
bool('') ## 字符串为空是 False
```




    False



#### 2.3.2.6 None

None可以作为一个常用的函数参数默认值


```python
def add_and_maybe_multiply(a, b, c=None):
    result = a + b
    
    if c is not None:
        result = result * c
        
    return result
```

None 不仅是一个关键字，还是NoneType类型的唯一实例？？？

嘛意思


```python
type(None)
```




    NoneType



#### 2.3.2.7 日期和时间

做数据，时间是一个非常重要的因素

这个得好好整一下


```python
from datetime import datetime, date, time
```


```python
dt = datetime(2011, 10, 29, 20, 30, 21)
```


```python
dt.day
```




    29




```python
dt.minute
```




    30




```python
dt.date()
```




    datetime.date(2011, 10, 29)




```python
dt.time()
```




    datetime.time(20, 30, 21)




```python
dt.strftime('%m/%d/%Y %H:%M')
```




    '10/29/2011 20:30'




```python
datetime.strptime('20091031','%Y%m%d') ## 字符串转换为datetime对象
```




    datetime.datetime(2009, 10, 31, 0, 0)



替换时间中的一些值，这个经常会用到


```python
dt ## dt是啥来着，看一眼
```




    datetime.datetime(2011, 10, 29, 20, 30, 21)




```python
dt.replace(minute=0, second=0)
```




    datetime.datetime(2011, 10, 29, 20, 0)



datetime.datetime是不可变类型，以上都是产生新的对象

如果想算两个时间点之间隔了多长时间怎么办


```python
dt1 = datetime(2019, 5, 8, 10, 12, 30)
```


```python
dt2 = datetime(2024, 6, 7, 8, 28, 57)
```


```python
delta = dt2 - dt1
```


```python
delta ## 产生一种新的类型
```




    datetime.timedelta(days=1856, seconds=80187)



如果知道今天，想知道过十天是哪天，怎么办


```python
dt2 + delta
```



    datetime.datetime(2029, 7, 8, 6, 45, 24)



Datetime格式化的详细说明(ISO C89兼容)

%Y  四位年份

%y  两位年份

%m  两位的月份 [01,12]

%d  两位的天数 [01,31]

%H  24小时制 [00, 23]

%I  12小时制 [01,12]

%M  两位分钟 [00,59]

%S  秒值 [00,61] 60、61用于区分闰秒？？还有闰秒？

%w  星期值 [0, 6] 0星期天

%U  一年中第几个星期 [00,53]，星期天是每周第一天，第一个星期天前一周为第0个星期

%W  一年中第几个星期 [00,53]，星期一是每周第一天，第一个星期一前一周为第0个星期

%z  UTC时区骗纸，格式为 +HHMM 或 -HHMM，简单时区则为空？？

%F  %Y-%y-%d的简写，如2014-4-18

%d  %m/%d/%y的简写，如04/18/12

-------------------------------------------

差不多搞定啦，很不易啊，开心(*^▽^*)


![0207](https://github.com/HanMENG15990045033/photos-for-document/blob/master/0207_end.jpg)

