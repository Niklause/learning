### compile()

编译正则表达式模式，返回一个对象的模式

格式:

``` python
import re
re.compile(pattern,flags=0)
# pattern: 编译时用的表达式字符串
# flags 编译标志位，用于修改正则表达式的匹配方式，如：是否区分大小写，多行匹配等。
# 这个方法的主要作用是 把一个正则匹配的表达式保存起来，到时候可以直接用这个去目标文本中查找(以编译后的格式)
```


| 标识                 | 含义                                 |
| :----------------- | :--------------------------------- |
| re.S/re.DOTALL     | 使匹配包括换行在内的所有字符                     |
| re.I/re.IGNORECASE | 使匹配对大小写不敏感                         |
| re.L/re.LOCAL      | 做本地化识别匹配，法语等                       |
| re.M/re.MULTILINE  | 多行匹配，影响^和$                         |
| re.X/re.VERBOSE    | 该标志通过给与更灵活的格式以便将正则表达式写得更易于理解       |
| re.U               | 根据Unicode字符集解析字符，这个标志影响\w,\W,\b,\B |

#### re.compile(pattern, flags=0)

```python
import re
prog = re.compile(pattern)
result = prog.match(string)

# 上面这段代码同下面这段代码相同
result = re.match(pattern, string)
```

#### re.search(pattern, string, flags=0)

re.search 函数会在字符串内查找模式匹配，直到找到第一个匹配然后返回一个对象k。

k.group(0)代表整个匹配模式对应的字符串，k.group(1)代表匹配模式中的组（即括号中的对应的）对应的字符串

如果没有匹配，则返回None。

```python
import re
name = "Hello,My name is kuangl,nice to meet you…"
k = re.search(r'k(uan)gl,(nic)e', name) # k(uan)gl是整个对应的字符串 uan是组内（括号内）对应的字符串,可以匹配多个括号里面的值
if k:
  print k.group(0), k.group(1),k.group(2)
else:
  print "sorry, not search!"
# kuangl uan
```

#### re.match(pattern, string, flags=0)

字符串的开头是否能匹配正则表达式。如果能匹配返回_sre.SRE_Match对象，如果不能匹配返回None。

```python
import re
i = 1991
if re.match('\d+',i): # 匹配第一个字符串是数字 即1991
	print 'is Match'
else:
	print 'no Match'
```

#### re.findall(pattern, string, flags=0)

re.findall在目标字符串查找所有符合规则的字符串

返回的结果是一个列表（如果有括号()分组，则只返回所有分组的列表），列表中存放的是符合规则的字符串，如果没有符合规则的字符串找到，就会返回一个空值。 

```python
mail = 'user01@mail.com user02@mail.com user04@mail.com'
re.findall(r'(\w+@mail.[a-z]{3})',mail)# \w 匹配字符(a-zA-Z0-9) + 表示0-很多个 \w+就是匹配很多个字符 [a-z]表示匹配26个英文字母{3}表示一定要匹配3个 所有[a-z]{3}表示匹配3个英文字母
['user01@mail.com', 'user02@mail.com', 'user04@mail.com']
```

## Beautiful Soup

### 1.Beautiful Soup的简介

Beautiful Soup是python的一个库，最主要的功能是从网页抓取数据。

### 2.Beautiful Soup 安装

```python
pip install beautifulsoup4
```

```
pip install lxml
```



## 创建 Beautiful Soup 对象

首先必须导入bs4库

```python
from bs4 import BeautifulSoup
```

我们创建一个对象，后面的例子我们便会用它来演示

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
```

创建beautifulsoup对象

```python
soup = BeautifulSoup(html)
```

如此我们来打印下soup对象的内容，格式化输出

```python
print soup.prettify()
```

```html
<html>
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
 <body>
  <p class="title" name="dromouse">
   <b>
    The Dormouse's story
   </b>
  </p>
  <p class="story">
   Once upon a time there were three little sisters; and their names were
   <a class="sister" href="http://example.com/elsie" id="link1">
    <!-- Elsie -->
   </a>
   ,
   <a class="sister" href="http://example.com/lacie" id="link2">
    Lacie
   </a>
   and
   <a class="sister" href="http://example.com/tillie" id="link3">
    Tillie
   </a>
and they lived at the bottom of a well.
  </p>;
  <p class="story">
   ...
  </p>
 </body>
</html>
```

以上便是输出结果，格式化打印出了它的内容。

### 四大对象种类

Beautiful Soup将复杂HTML文档转化成一个复杂的属性结构，每个节点都是python对象，所有的对象都可以归为4种：

* Tag
* NavigableString
* BeautifulSoup
* Comment

(1) Tag

Tag就是HTML中的标签，如

```html
<title>The Dormouse's stroy</title>
<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
```

上面的title a 等等HTML标签加上里面包括的内容就是Tag。

```python
print soup.title
#<title>The Dormouse's story</title>
print soup.head
#<head><title>The Dormouse's story</title></head>
print soup.a
#<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
print soup.p
#<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
```

(2) NavigableString

得到标签的内容，如何获取标签内部的文字怎么办？用.string即可

```python
print soup.p.string
# The Dormouse's story
```

(3) BeautifulSoup

表示一个文档的全部内容

查找所有的find_all

```python
print soup.find_all('a')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

返回所有的a标签

根据特殊标签查找比如id

```python
soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
# 查找id 为 link2的标签
```

```python
soup.find_all("a", class_="sister")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
# 查找所有的class为sister的列表
for a in soup.find_all("a"):
    print a.get("href")
# 打印所有a标签中的href的值
# http://example.com/elsie
# http://example.com/lacie
# http://example.com/tillie
```

# 附表

一些匹配规则

| 语法    | 说明                                       |  表达式实例  |        完整匹配的字符串        |
| ----- | ---------------------------------------- | :-----: | :--------------------: |
| .     | 匹配任意除换行符"\n"外的字符。在DOTALL模式中也能匹配换行符       |   a.c   | abc或acc,a1c,  a c,aMc  |
| \     | 转义符，使后一个字符改变原来的意思。如果字符串中有字符\*需要匹配，可以私用\*或者字符串集\[\*] |  a\\.c  |          a.c           |
| [...] | 字符集（字符类）。对应的位置可以是字符集中任意字符。[abc]或者[a-c]相同意思。第一个字符如果是^表示取相反意思的字符\[^abc]表示不是abc的其他字符 | a[bcd]e | abe       ace      ade |

预定义字符集（可以写在字符集[…]中）

| 语法   | 说明               | 表达式实例 | 完整匹配的字符串 |
| ---- | ---------------- | ----- | -------- |
| \d   | 数字:[0-9]         | a\dc  | a1c      |
| \D   | 非数字:\[^\d]       | a\Dc  | abc      |
| \s   | 空白字符（空格）         | a\sc  | a c      |
| \S   | 非空白字符\[^\s]      | a\Sc  | abc      |
| \w   | 单词字符:[A-Za-z0-9] | a\wc  | abc      |
| \W   | 非单词字符:\[^\w]     | a\Wc  | a c      |

数量词（用在字符或(...)之后）

| 语法    | 说明                                       | 表达式实例    | 完整匹配的字符串     |
| ----- | ---------------------------------------- | -------- | ------------ |
| *     | 匹配前一个字符0或无限次                             | abc*     | ab   abcccc  |
| +     | 匹配前一个字符1次或无限次                            | abc+     | abc   abcccc |
| ?     | 匹配前一个字符0次或1次                             | abc?     | ab  abc      |
| {m}   | 匹配前一个字符m次                                | ab{2}    | abb          |
| {m,n} | 匹配前一个字符M至n次。m和n可以省略：若省略m，则匹配0-n次；若省略n，则匹配m至无限次 | ab{1,2}c | abc abbc     |

写在最后

**1）数量词的贪婪模式与非贪婪模式**

正则表达式通常用于在文本中查找匹配的字符串。Python里数量词默认是贪婪的（在少数语言里也可能是默认非贪婪），总是尝试匹配尽可能多的字 符；非贪婪的则相反，总是尝试匹配尽可能少的字符。例如：正则表达式”ab\*”如果用于查找”abbbc”，将找到”abbb”。而如果使用非贪婪的数量 词”ab*?”，将找到”a”。

**注：我们一般使用非贪婪模式来提取。**
