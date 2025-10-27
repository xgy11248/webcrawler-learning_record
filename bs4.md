 # bs4

对于一个网页来说，都有一定的特殊的结构和层级关系，而且很多节点都有id或class来做区分，借此来提取。

## 1 简介

    Beautiful Soup就是Python的一个HTML或XML的解析库。
    1、bs自动将输入文档转换为Unicode编码，输出文档转换为UTF-8编码。

## 2 解析器
bs在解析时实际上依赖解析器，除了支持Python标准库中的HTML解析器外，害支持一些第三方解析器，比如lxml、xml、heml5lib等。

## 3 基本用法


```python
from bs4 import BeautifulSoup

#html为一个HTML字符串
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

soup = BeautifulSoup(html,'lxml')   #Beautiful对象初始化
print(soup.prettify())   # prettify()方法将要解析的字符串一标准的缩进形式输出
print(soup.title.string)   # html中title标签的文字
```

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
       ;
    and they lived at the bottom of a well.
      </p>
      <p class="story">
       ...
      </p>
     </body>
    </html>
    The Dormouse's story
    

## 4 节点选择器
直接调用节点的名称就可以选择节点元素，在调用string属性就可以得到节点内的文本了。

### 4.1 选择元素


```python
from bs4 import BeautifulSoup

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

soup = BeautifulSoup(html,'lxml')
print(soup.title)   #直接就可以寻找title节点
print(type(soup.title))
print(soup.title.string)   #打印title的文字
print(soup.head)   #打印head节点
print(soup.p)   #这种方式只会得到匹配到的第一个结果
```

    <title>The Dormouse's story</title>
    <class 'bs4.element.Tag'>
    The Dormouse's story
    <head><title>The Dormouse's story</title></head>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    

### 4.2 提取信息
    1、获取名称：使用name属性获取节点的名称
    2、获取属性：可以调用attrs获取所有属性，返回的是一个字典，要获取那么属性，即可attrs['name']来获取
    3、获取文本：可以调用string属性来获取节点中的文本。


```python
from bs4 import BeautifulSoup

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

soup = BeautifulSoup(html,'lxml')
#获取名称
print(soup.title.name)
#获取属性
print(soup.p.attrs)
print(soup.p.attrs['name'])
#获取属性的简单形式
print(soup.p['name'])
print(soup.p['class'])
#获取内容
print(soup.p.string)
```

    title
    {'class': ['title'], 'name': 'dromouse'}
    dromouse
    dromouse
    ['title']
    The Dormouse's story
    

### 4.3 嵌套选择
上述例子的返回的结果都是bs4.element.Tag类型的，同样可以继续调用节点进行下一步的选择，比如，获取了head节点元素，可以继续调用其内部的head节点元素。


```python
from bs4 import BeautifulSoup

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

soup = BeautifulSoup(html,'lxml')
print(soup.head.title)
print(type(soup.head.title))
print(soup.head.title.string)
```

    <title>The Dormouse's story</title>
    <class 'bs4.element.Tag'>
    The Dormouse's story
    

### 4.4 关联选择
有时候不能一步到位的选择某节点，则需要先选中某一结点，以此为基准在选取其他的节点。

#### 4.4.1 子节点和子孙节点
可以调用contents属性，获取选中节点的直接子节点。


```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'lxml')
print(soup.p.contents)   #获取p的直接子节点
```

    ['Once upon a time there were three little sisters; and their names were\n', <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, ',\n', <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, ' and\n', <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>, ';\nand they lived at the bottom of a well.']
    

返回的是一个列表，p节点里面既包含节点又包含文本，会列出p的直接子节点，包括文本和\n


```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

#调用children属性来获得子节点
soup = BeautifulSoup(html,'lxml')
print(soup.p.children)
for i,child in enumerate(soup.p.children):
    print(i,child)
```

    <list_iterator object at 0x00000256E3D14EE0>
    0 Once upon a time there were three little sisters; and their names were
    
    1 <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
    2 ,
    
    3 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    4  and
    
    5 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    6 ;
    and they lived at the bottom of a well.
    

调用了children属性后，获得是生成器类型


```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

#调用descendants属性,获取所有子孙节点
soup = BeautifulSoup(html,'lxml')
print(soup.p.descendants)
for i,child in enumerate(soup.p.descendants):
    print(i,child)
```

    <generator object Tag.descendants at 0x00000256E45B9460>
    0 Once upon a time there were three little sisters; and their names were
    
    1 <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
    2  Elsie 
    3 ,
    
    4 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    5 Lacie
    6  and
    
    7 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    8 Tillie
    9 ;
    and they lived at the bottom of a well.
    

#### 4.4.2 父节点和祖先节点
    1、获取父节点，使用parent属性
    2、获取祖先节点，使用parents属性


```python
from bs4 import BeautifulSoup

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

soup = BeautifulSoup(html,'lxml')
print(soup.p.parent)#获取直接父节点
```

    <body>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    <p class="story">...</p>
    </body>
    


```python
from bs4 import BeautifulSoup

html = """
<html>
<head>
<title>The Dormouse's story</title>
</head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'lxml')
print(soup.p.parents)
print(type(soup.p.parents))
print(list(enumerate(soup.p.parents)))
```

    <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    <p class="story">...</p>
    </body></html>
    <generator object PageElement.parents at 0x00000256E45BAF10>
    <class 'generator'>
    [(0, <body>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    <p class="story">...</p>
    </body>), (1, <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    <p class="story">...</p>
    </body></html>), (2, <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="title" name="dromouse"><b>The Dormouse's story</b></p>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>,
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    <p class="story">...</p>
    </body></html>)]
    

#### 4.4.3 兄弟节点
获得同级的节点


```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>
Hello
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'lxml')
print('Next',soup.a.next_sibling)   #下一个同级节点
print('Prev',soup.a.previous_sibling)   #上一个同级节点
print('Nexts',list(enumerate(soup.a.next_siblings)))
print('prevs',list(enumerate(soup.a.previous_siblings)))
```

    Next 
    Hello
    
    Prev Once upon a time there were three little sisters; and their names were
    
    Nexts [(0, '\nHello\n'), (1, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>), (2, ' and\n'), (3, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>), (4, ';\nand they lived at the bottom of a well.')]
    prevs [(0, 'Once upon a time there were three little sisters; and their names were\n')]
    

#### 4.4.4 提取信息
获取节点的信息，比如文本、属性等


```python
from bs4 import BeautifulSoup

html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>
Hello
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

soup = BeautifulSoup(html,'lxml')
print('Next Sibling:')
print(type(soup.a.next_sibling))
print(soup.a.next_sibling)
print(soup.a.next_sibling.string)   #获取文字
print('Parent')
print(type(soup.a.parents))
print(list(soup.a.parents)[0])
print(list(soup.a.parents)[0].attrs['class'])   #获取class属性的值
```

    Next Sibling:
    <class 'bs4.element.NavigableString'>
    
    Hello
    
    
    Hello
    
    Parent
    <class 'generator'>
    <p class="story">Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>
    Hello
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>
    ['story']
    

如果返回的是单个的节点，可以直接调用string，attrs等属性获得其文本和属性；如果返回的是多个节点的生成器，则可以转为列表后取出某个元素，然后在调用string，attrs等属性获取对用节点的文本和属性。

## 6 方法选择器
属性选择器速度快，但是对于比较复杂的选择，就不够灵活了。但bs还有一些查询的方法，比如find_all(),fing()等，传入相应的参数就可以了。

### find_all()

API是find_all(name,attrs,recursive,text,**kwargs)

#### （1）name


```python
from bs4 import BeautifulSoup

html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''

soup = BeautifulSoup(html,'lxml')
print(soup.find_all(name='ul'))   #寻找所有的ul节点
print(type(soup.find_all(name='ul')[0])) 

#Tag类型可以嵌套查询
for ul in soup.find_all(name='ul'):   
    print(ul.find_all(name='li'))
    
    
#之后可以遍历每个li，获取文本
for ul in soup.find_all(name='ul'):
    for li in ul.find_all(name='li'):
        print(li.string)
```

    [<ul class="list" id="list-1">
    <li class="element">Foo</li>
    <li class="element">Bar</li>
    <li class="element">Jay</li>
    </ul>, <ul class="list list-small" id="list-2">
    <li class="element">Foo</li>
    <li class="element">Bar</li>
    </ul>]
    <class 'bs4.element.Tag'>
    [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>]
    [<li class="element">Foo</li>, <li class="element">Bar</li>]
    Foo
    Bar
    Jay
    Foo
    Bar
    

传回的元素依然是bs4.element.Tag类型的，因为都是Tag类型的，所以可以嵌套查询。

#### （2）attrs
传入属性来查询。


```python
from bs4 import BeautifulSoup

html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''

soup = BeautifulSoup(html,'lxml')
print(soup.find_all(attrs={'id':'list-1'}))
print(soup.find_all(attrs={'class':'element'}))

#常用属性的简单形式
print(soup.find_all(id='list-1'))
print(soup.find_all(class_='element'))
```

    [<ul class="list" id="list-1">
    <li class="element">Foo</li>
    <li class="element">Bar</li>
    <li class="element">Jay</li>
    </ul>]
    [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
    [<ul class="list" id="list-1">
    <li class="element">Foo</li>
    <li class="element">Bar</li>
    <li class="element">Jay</li>
    </ul>]
    [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
    

#### （3）text
text参数可以用来匹配节点的文本，传入的形式可以是字符串，可以是正则表达式。


```python
from bs4 import BeautifulSoup
import re

html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''

#传入的可以是字符串，也可以是正则表达式。
soup = BeautifulSoup(html,'lxml')
print(soup.find_all(text = 'Hello'))   
#print(soup.find_all(text = re.compile('link')))
```

    ['Hello']
    

### find()
返回的是单个的元素，而前者是所有匹配元素组成的列表

    还有许多查询方法：
    find_parents(), find_parent(): 前者返回所有祖先节点，后者返回直接父节点。
    find_next_siblings(), find_next_sibling(): 前者返回后面所有的兄弟节点，后者返回后面的第一个兄弟节点。
    find_previous_siblings(), find_previous_sibling(): 前者返回前面的所有兄弟节点，后者返回前面的第一个兄弟节点。
    find_all_next(), find_next(): 前者返回节点后所有符合条件的节点，后者返回第一个符合条件的节点。
    find_all_previous(), find_previou(): 前者返回节点前所有符合条件的节点，后者返回第一个符合条件的节点。


```python
import requests
from bs4 import BeautifulSoup


class Baidu_photo_get(object):
    def __init__(self):
        self.number = 1
        self.url = "https://so.toutiao.com/search?keyword=%E9%A3%8E%E6%99%AF&pd=atlas&dvpf=pc&aid=4916&page_num=0&search_json=%7B%22from_search_id%22%3A%22202311012132564BC1AED711837A44558F%22%2C%22origin_keyword%22%3A%22%E7%BE%8E%E5%A5%B3%22%2C%22image_keyword%22%3A%22%E7%BE%8E%E5%A5%B3%22%7D&source=input"
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36 Edg/118.0.2088.76',
            'Cookie': 'msToken=inSIYQCMcgQBS_WUQq9eZGwZYXJ3aJH4oisRFRvTPXjWLetIE2Fgy0gygqV4YwjZyVchLEh6ublkb_9a1EB_ZTaFa52rRtlNlBJPIYc7; tt_webid=7295968954378421798; _ga_QEHZPBE5HH=GS1.1.1698725159.1.0.1698725159.0.0.0; _ga=GA1.1.4784048.1698725160; ttwid=1%7CZHZf2H84ODU8HGvNgE6R7ItycHQGup5OoD9-LskKIik%7C1698725160%7C91e339d248bafc5260a492036ed670625bbd96ea3d17febdcb9ccc16dfb39bb1; __ac_nonce=065407d2c00b3483a557b; __ac_signature=_02B4Z6wo00f017yueGAAAIDA6Szz.5tt6XO8jnzAAIp01c; __ac_referer=https://www.toutiao.com/; _tea_utm_cache_4916=undefined; _S_WIN_WH=1488_742; _S_DPR=1.25; _S_IPAD=0; s_v_web_id=verify_lodt3rdk_jZ1cBKNS_em8n_4wCG_8hSi_foqXnR2uODsR'
        }

    def data_get_index(self):
        resp = requests.get(url=self.url, headers=self.headers)
        if resp.status_code == 200:
            return resp.text
        else:
            return None

    def parse_data_index(self, response):
        soup = BeautifulSoup(response, 'lxml')
        html = soup.find_all('img')
        for data in html:
            url = data.get('src')
            self.save_photo_data(url)

    def save_photo_data(self, url):
        file_data = f'第{self.number}张照片'
        with open('./photo/' + file_data + '.jpeg', 'wb') as f:
            img = requests.get(url).content
            f.write(img)
            print(f'{file_data}图片--保存完毕！')
            self.number += 1

    def run(self):
        response = self.data_get_index()
        # print(response)
        self.parse_data_index(response)


if __name__ == '__main__':
    Photo = Baidu_photo_get()
    Photo.run()

```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Cell In[3], line 44
         42 if __name__ == '__main__':
         43     Photo = Baidu_photo_get()
    ---> 44     Photo.run()
    

    Cell In[3], line 39, in Baidu_photo_get.run(self)
         37 response = self.data_get_index()
         38 # print(response)
    ---> 39 self.parse_data_index(response)
    

    Cell In[3], line 26, in Baidu_photo_get.parse_data_index(self, response)
         24 for data in html:
         25     url = data.get('src')
    ---> 26     self.save_photo_data(url)
    

    Cell In[3], line 30, in Baidu_photo_get.save_photo_data(self, url)
         28 def save_photo_data(self, url):
         29     file_data = f'第{self.number}张照片'
    ---> 30     with open('./photo/' + file_data + '.jpeg', 'wb') as f:
         31         img = requests.get(url).content
         32         f.write(img)
    

    File D:\ruanjian\anaconda3 2023.03-1\lib\site-packages\IPython\core\interactiveshell.py:282, in _modified_open(file, *args, **kwargs)
        275 if file in {0, 1, 2}:
        276     raise ValueError(
        277         f"IPython won't let you open fd={file} by default "
        278         "as it is likely to crash IPython. If you know what you are doing, "
        279         "you can use builtins' open."
        280     )
    --> 282 return io_open(file, *args, **kwargs)
    

    FileNotFoundError: [Errno 2] No such file or directory: './photo/第1张照片.jpeg'



```python

```
