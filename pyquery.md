# pyquery
如果对CSS选择器和jQuery比较了解，可以选择使用这个库。



## 1 初始化
在bs中，我们需要传入HTML文本进行初始化一个bs对象，在pyquery中也是一样的，她的初始化的方法有很多，比如直接传入字符串，URL，文件名等等

### 1.1 字符串初始化


```python
from pyquery import PyQuery as pq

html = '''
<div>
  <ul>
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
'''

doc = pq(html)   #创建一个pq实例，也就是初始化一个pq对象
print(doc('li'))
```

    <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    

### 1.2URL初始化
参数不经可以以字符串的形式传递，还可以传入网页的URL，只需要指定参数为URL即可。


```python
from pyquery import PyQuery as pq

doc = pq(url= 'https://cuiqingcai.com')
print(doc('title'))  
```

    <title>静觅丨崔庆才的个人站点 - Python爬虫教程</title>
      
    

PyQuery对象会先请求这个URL，然后用得到的HTML内容完成初始化，相当于将网页源代码以字符串的形式传递给PyQuery类类初始化，下面的功能是相同的：


```python
from pyquery import PyQuery as pq
import requests

html = requests.get('https://cuiqingcai.com').text   #???为什么不加text不行呢
doc = pq(html)
print(doc('title'))
```

    <title>静觅丨崔庆才的个人站点 - Python爬虫教程</title>
      
    

### 1.3 文件初始化
可以传递本地的文件名，此时将参数指定为filename即可。


```python
from pyquery import PyQuery as pq

#这里需要有一个本地文件demo.html，其内容是待解析的字符串。
doc = pq(filename = 'demo.html')
print(doc('li'))
```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Input In [7], in <cell line: 2>()
          1 from pyquery import PyQuery as pq
    ----> 2 doc = pq(filename = 'demo.html')
          3 print(doc('li'))
    

    File D:\ruanjian\anaconda3 2023.03-1\envs\pachong\lib\site-packages\pyquery\pyquery.py:172, in PyQuery.__init__(self, *args, **kwargs)
        169 if kwargs:
        170     # specific case to get the dom
        171     if 'filename' in kwargs:
    --> 172         html = open(kwargs['filename'],
        173                     encoding=kwargs.get('encoding'))
        174     elif 'url' in kwargs:
        175         url = kwargs.pop('url')
    

    FileNotFoundError: [Errno 2] No such file or directory: 'demo.html'


## 基本CSS选择器

## 实例引入


```python
from pyquery import PyQuery as pq

html = '''
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
'''

doc = pq(html)
print(doc('#container .list li'))
print(type(doc('#container.list li')))
```

    <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <class 'pyquery.pyquery.PyQuery'>
    

#container .list li表示先选择id为container的几点，在选择其内部class为list的节点内部的所有li节点

## 4 查找节点
介绍一些常用的查询函数，和jQuery中的用法完全相同。

### 4.1 子节点
使用find（）方法，此时传入的参数是CSS选择器


```python
from pyquery import PyQuery as pq

html = '''
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
'''

doc = pq(html)
items = doc('.list')   #传入CSS选择器？？
print(type(items))
print(items)
lis = items.find('li')
print(type(lis))
print(lis)
#find（）打印出来的是所有的子孙节点，若只想查找子节点，可以使用children（）方法
lis_1 = items.children()
print(type(lis_1))
print(lis_1)

#若想要子节点中的符合条件的节点，可以向children（）中传入CSS选择器
lis_2 = items.children('.active')
print(lis_2)
```

    <class 'pyquery.pyquery.PyQuery'>
    <ul class="list">
        <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
    
    <class 'pyquery.pyquery.PyQuery'>
    <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <class 'pyquery.pyquery.PyQuery'>
    <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        
    

### 4.2 父节点
可以使用parent（）方法来获取某个节点的父节点


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
items = doc('.list')   #CSS选择器，选取class为list的节点
container = items.parent()   
print(type(container))
print(container)

#选取的是直接父节点，若想要祖先节点，可以使用parents()方法
parents = items.parents()
print(type(container))
print(parents)

#若想要某某个特定节点，也可以向parents（）传入CSS选择器
parent = items.parents('.wrap')
print(parent)
```

    <class 'pyquery.pyquery.PyQuery'>
    <div id="container">
      <ul class="list">
        <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
    </div>
    
    <class 'pyquery.pyquery.PyQuery'>
    <div class="wrap">
    <div id="container">
      <ul class="list">
        <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
    </div>
    </div><div id="container">
      <ul class="list">
        <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
    </div>
    
    <div class="wrap">
    <div id="container">
      <ul class="list">
        <li class="item-0">first item</li>
        <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
    </div>
    </div>
    

### 4.3 兄弟节点
如果要获取兄弟节点，可以使用siblings（）方法


```python
from pyquery import PyQuery as pq 

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
#选取class为list的节点内部class为item-0和active的节点。
li = doc('.list .item-0.active')
print(li.siblings())

#同样可以向siblings（）中传入CSS选择器
li_1 = doc('.list .item-0.active')
print(li.siblings('.active'))
```

    <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-0">first item</li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        
    

## 5 遍历
pyquery的结果可能是多个节点，也可能是单个节点

1、单个节点时：


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
li = doc('.item-0.active')
print(li)
print(str(li))

```

    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    

2、多个节点时需要遍历来获取。调用items()方法


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
lis = doc('li').items()   #CSS选择器选取节点名为li的，调用items（）方法
print(type(lis))   #返回的时生成器类型的
for li in lis:
    print(li,type(li))   #遍历得到的是PyQuery类型的
```

    <class 'generator'>
    <li class="item-0">first item</li>
         <class 'pyquery.pyquery.PyQuery'>
    <li class="item-1"><a href="link2.html">second item</a></li>
         <class 'pyquery.pyquery.PyQuery'>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <class 'pyquery.pyquery.PyQuery'>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <class 'pyquery.pyquery.PyQuery'>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
      <class 'pyquery.pyquery.PyQuery'>
    

## 6 获取信息
提取到节点后，最终目的是获取节点包含的信息，重要的有两类，一种是获取属性，一种是获取文本。

### 6.1 获取属性
调用attrs（）方法获取属性


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
a = doc('.item-0.active a')   #CSS选择器，获取属性同时有item-0和active的节点的子节点中的节点名为a的节点
print(a,type(a))
print(a.attr('href'))   #获取a的href属性

#也可以调用attr属性来获取属性
print(a.attr.href)
```

    <a href="link3.html"><span class="bold">third item</span></a> <class 'pyquery.pyquery.PyQuery'>
    link3.html
    link3.html
    


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
a = doc('a')
print(a)   #直接打印有四个

#调用attr属性只会获得第一个a节点的属性
print(a.attr('href'))
print(a.attr.href)

#要获取所有的就要用到遍历的方法了，调用items（）方法
for item in a.items():
    print(item.attr('href'))
```

    <a href="link2.html">second item</a><a href="link3.html"><span class="bold">third item</span></a><a href="link4.html">fourth item</a><a href="link5.html">fifth item</a>
    link2.html
    link2.html
    link2.html
    link3.html
    link4.html
    link5.html
    

### 6.2 获取文本
获取节点之后的操作就是获取其内部的文本了，此时可以调用text（）方法


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
a = doc('.item-0.active a')
print(a)

#调用text（）方法会忽略所有HTML，只返回所有的纯文本内容
print(a.text())
```

    <a href="link3.html"><span class="bold">third item</span></a>
    third item
    

要获得节点内部的HTML文本，就要使用html（）方法


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
li = doc('.item-0.active')
print(li)

#获得li节点内部的HTML文本
print(li.html())
```

    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <a href="link3.html"><span class="bold">third item</span></a>
    

同样的，如果是多个节点，text（）和html（）会返回什么


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
li = doc('li')

#调用html（）返回了第一个节点的heml文本
#要获取所有的需要用到遍历
print(li.html())

#调用text（）会返回所有li节点的文本，用空格隔开
print(li.text())
print(type(li.text()))
```

    first item
    first item second item third item fourth item fifth item
    <class 'str'>
    

## 7 节点操作
pyquery提供了一系列方法来对节点进行动态修改，比如为某个节点添加一个class，移除某个节点等

### 7.1 addClass和removeClass


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
li = doc('.item-0.active')   #选中class值为。。。的节点
print(li)
li.removeClass('active')   #删除li的active属性
print(li)
li.addClass('hhhhh')   #给li添加hhhhh属性
print(li)
```

    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <li class="item-0"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <li class="item-0 hhhhh"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    

### 7.2 attr、text和html
    用attr（）对属性进行操作。
    用text（）和html（）改变节点内部的内容。


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)
li = doc('.item-0.active')
print(li)
li.attr('name','link')   #attr()修改，第一个参数为属性名，第二个为属性值
print(li)
li.text('changed item')   #将li内部的文字修改
print(li)
li.html('<span>changed item</span>')   #将li内部的文字修改为heml文本
print(li)
```

    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <li class="item-0 active" name="link"><a href="link3.html"><span class="bold">third item</span></a></li>
        
    <li class="item-0 active" name="link">changed item</li>
        
    <li class="item-0 active" name="link"><span>changed item</span></li>
        
    

### 注意：
如果attr（）方法只传入了一个参数，则是获取节点的这个属性值，若传入两个参数，则是修改（1）的属性值为（2）。text（）和html（）方法如果不传入参数，是获取节点内部的纯文本或HTML文本，若传入参数值，则是进行赋值。

## 7.3 remove()
remove（）方法用来移除。


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
    Hello,World
<p>this is a paragraph.</p>
</div>
'''

doc = pq(html)
wrap = doc('.wrap')

#可以把节点内部的所有纯文本提取出来
print(wrap.text())

#使用remove（）方法
wrap.find('p').remove()   # 移除p节点
print(wrap.text())
```

    Hello,World
    this is a paragraph.
    Hello,World
    



还有很多方法，例如append（）、empty（）、prepend（）等，和jQuery用法完全一致。

## 7.4 after()方法

在某节点后添加一个html。例：p("div.bbb").after("""<div class="ccc">hhh</div>""")

## 7.5 其他方法

append()在节点内部添加一个节点。
remove_attr() 删除节点的某个属性

## 8 伪类选择器
CSS选择器的强大之处在于支持多种多样的伪类选择器，例如选择第一个节点、最后一个节点、奇偶数节点等。


```python
from pyquery import PyQuery as pq

html = '''
<div class="wrap">
<div id="container">
  <ul class="list">
    <li class="item-0">first item</li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a></li>
 </ul>
</div>
</div>
'''

doc = pq(html)

#选取了第一个li节点
li = doc('li:first-child')
print(li)

#选取最后一个li节点
li = doc('li:last-child')
print(li)


#选取第2个li节点
li = doc('li:nth-child(2)')
print(li)

#选取第三个li节点之后的li节点
li = doc('li:gt(2)')   #gt(2)代表第2+1之后的
print(li)

#选取偶数位置的
li = doc('li:nth-child(2n)')
print(li)

#选取包含second的节点
li = doc('li:contains(second)')
print(li)

```

    <li class="item-0">first item</li>
        
    <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <li class="item-1"><a href="link2.html">second item</a></li>
        
    <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        <li class="item-0"><a href="link5.html">fifth item</a></li>
     
    <li class="item-1"><a href="link2.html">second item</a></li>
        <li class="item-1 active"><a href="link4.html">fourth item</a></li>
        
    <li class="item-1"><a href="link2.html">second item</a></li>
        
    


```python


```
