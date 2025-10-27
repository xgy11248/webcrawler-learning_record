# 1、XPath初步使用

    提供了简介的路径选择表达式。
    XPath常用规则：
    1.nodename  选取此节点的所有子节点。
    2./  从当前节点选取直接子节点。
    3.//  从当前系欸点选取子孙节点。
    4. .  选取当前节点。
    5. ..  选取当前节点的父节点。
    6. @  选取属性。

#### 实例引入


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''
html = etree.HTML(text)#构成一个XPath解析对象
#html = etree.parse('./test.html', etree.HTMLParser())   #直接读取文本文件进行解析
#result= html.xpath('//*')    #匹配所有节点
result = etree.tostring(html)    #使用tostring()进行修正
print(type(result))    #result是一个bytes类型的变量
print(result.decode('utf-8'))    #用decode()方法将其改为str型的
```

    <class 'bytes'>
    <html><body><div>
    <ul>
    <li class="item-0"><a href="link1.html">first item</a></li>
    <li class="item-1"><a href="link2.html">second item</a></li>
    <li class="item-inactive"><a href="link3.html">third item</a></li>
    <li class="item-1"><a href="link4.html">fourth item</a></li>
    <li class="item-0"><a href="link5.html">fifth item</a>
    </li></ul>
    </div>
    </body></html>
    

#### 获取所有节点


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

#*表示匹配所有节点
html = etree.HTML(text)
result = html.xpath('//*')
print(result)

# 也可以匹配所有的指定的节点
result_2 = html.xpath('//li')
print(result_2)
```

    [<Element html at 0x196af999800>, <Element body at 0x196af99a340>, <Element div at 0x196af999480>, <Element ul at 0x196af99aa00>, <Element li at 0x196af99a380>, <Element a at 0x196af99a980>, <Element li at 0x196af998580>, <Element a at 0x196af99b640>, <Element li at 0x196af99b140>, <Element a at 0x196af99b680>, <Element li at 0x196af99aec0>, <Element a at 0x196af99ae40>, <Element li at 0x196af99b0c0>, <Element a at 0x196af99bf00>]
    [<Element li at 0x196af99a380>, <Element li at 0x196af998580>, <Element li at 0x196af99b140>, <Element li at 0x196af99aec0>, <Element li at 0x196af99b0c0>]
    

#### 匹配子节点


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

#/用来获取直接子节点
# /a可以获取li的所有“直接”a子节点
html = etree.HTML(text)
result = html.xpath('//li/a')
print(result)

#//可以用来获取所有子孙节点
result_1 = html.xpath('//ul//a')
print(result)

#此处的result_2打印为空，因为ul标签没有直接的a子节点
result_2 = html.xpath('//ul/a')
print(result_2)
```

    [<Element a at 0x201385b4d00>, <Element a at 0x201385b4dc0>, <Element a at 0x201385b4d80>, <Element a at 0x201385b4cc0>, <Element a at 0x201385b53c0>]
    [<Element a at 0x201385b4d00>, <Element a at 0x201385b4dc0>, <Element a at 0x201385b4d80>, <Element a at 0x201385b4cc0>, <Element a at 0x201385b53c0>]
    []
    

#### 匹配父节点
可以使用 .. 来匹配已知节点的父节点。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

# ..为选取当前节点的父节点，@用来选属性
html = etree.HTML(text)
result = html.xpath('//a[@href="link4.html"]/../@class')
print(result)

#同样也可以用parent::来获取父节点
result_3 = html.xpath('//a[@href="link4.html"]/parent::*/@class')
print(result_3)

```

    ['item-1']
    ['item-1']
    

#### 属性匹配
在选取时，可以使用@进行属性匹配。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

#使用@来选取带有特定属性的某节点
html = etree.HTML(text)
result = html.xpath('//li[@class="item-0"]')
print(result)
```

    [<Element li at 0x201385fddc0>, <Element li at 0x201385fccc0>]
    

#### 文本获取
使用XPath的text()方法获取节点中的文本


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result = html.xpath('//li[@class="item-0"]/text()')
print(result)
```

    ['\n']
    

   只获得了一“\n”，因为text()的前面是一个“\”，意思是获取直接子节点，因为li的子节点是a节点，而文字都在a节点的内部，所以匹配的是理解点修正后的空白，并没有选中a节点，当然不会获得a节点内部的文字。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result = html.xpath('//li[@class="item-0"]/a/text()')    #逐层选取
result_2 = html.xpath('//li[@class="item-0"]//text()')    #含有一个li结点的空白
print(result)
print(result_2)
```

    ['first item', 'fifth item']
    ['first item', 'fifth item', '\n']
    

    1、使用//加text()可以全面的获得所有节点的文本，但可能会获得到一些特殊字符。
    2、使用特定子孙节点的方法可以保证获得的文本的整洁度。

#### 属性获取
还是使用@符号来获取属性。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result = html.xpath('//li/a/@href')
print(result)
```

    ['link1.html', 'link2.html', 'link3.html', 'link4.html', 'link5.html']
    

上文中[@---]使用来获取特定属性的节点，但此处的@是获得某节点的属性。

#### 属性多值匹配


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0 li li-first"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
# 这里使用前先的匹配是获取不到first item的。
result = html.xpath('//li[@class="item-0"]/a/text()')
print(result)

# contains()函数，第一个传入属性名称，第二个传入属性值。
result1 = html.xpath('//li[contains(@class, "item-0")]/a/text()')
print(result1)
```

    ['fifth item']
    ['first item', 'fifth item']
    

#### 多属性匹配
用多个属性来确定一个点，同时匹配多个属性，使用and来连接。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0 li li-first" name="item"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result = html.xpath('//li[contains(@class,"li") and @name="item"]/a/text()')
print(result)
```

    ['first item']
    

不光能使用and，or等运算符也可以使用。

#### 按序选择
当同时匹配了多个节点时，可以使用中括号传入索引的方法来获取特定的节点。


```python
from lxml import etree

text = '''
<div>
<ul>
<li class="item-0"><a href="link1.html">first item</a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result1 = html.xpath('//li[1]/a/text()')   #表示获得第一个li节点
print(result1)
result2 = html.xpath('//li[last()]/a/text()')   #表示获得最后一个li节点
print(result2)
result3 = html.xpath('//li[position()<3]/a/text()')   #表示获得位置小于3的节点
print(result3)
result4 = html.xpath('//li[last()-2]/a/text()')   #表示获得倒数第三个li节点，last()表示最后一个
print(result4)
```

    ['first item']
    ['fifth item']
    ['first item', 'second item']
    ['third item']
    

#### 节点轴选择



```python
from lxml import etree

text = '''
<div>
<ul class='ul'>
<li class="item-0" name="laowang"><a href="link1.html"><span>first item</span></a></li>
<li class="item-1"><a href="link2.html">second item</a></li>
<li class="item-inactive"><a href="link3.html">third item</a></li>
<li class="item-1"><a href="link4.html">fourth item</a></li>
<li class="item-0"><a href="link5.html">fifth item</a>
</ul>
</div>
'''

html = etree.HTML(text)
result1 = html.xpath('//li[1]/ancestor::*')
print(result1)
result2 = html.xpath('//li[1]/ancestor::div')
print(result2)
result3 = html.xpath('//li[1]/attribute::*')
print(result3)
result4 = html.xpath('//li[1]/child::a[@href="link1.html"]')
print(result4)
result5 = html.xpath('//li[1]/descendant::span')
print(result5)
result6 = html.xpath('//li[1]//following::*[2]')
print(result6)
result7 = html.xpath('//li[1]//following-sibling::*')
print(result7)
```

    [<Element html at 0x20138896e00>, <Element body at 0x20138896f00>, <Element div at 0x20138896f80>, <Element ul at 0x201388974c0>]
    [<Element div at 0x20138896f80>]
    ['item-0', 'laowang']
    [<Element a at 0x20138897200>]
    [<Element span at 0x20138896ec0>]
    [<Element a at 0x2013889c480>]
    [<Element li at 0x2013889c700>, <Element li at 0x2013889c6c0>, <Element li at 0x2013889c740>, <Element li at 0x2013889c780>]
    

    第一次选择使用ancesotor轴，获取所有的祖先节点。需要使用两个冒号，节点选择器采用*，表示匹配所有节点。
    第二次选择为匹配祖先节点的div节点。
    第三次选择为attribute轴，可以获取属性值，*表示获取所有属性值。
    第四次选择为child轴，可以获取所有直接子节点。
    第五次选择为descendant轴，可以获取所有子孙节点,span为限制条件，获取span节点。
    第六次选择为following轴，获取当前节点之后的所有节点。数字2为第二个后续节点。
    第七次使用following-sibling轴，获取当前节点之后的所有同级节点。


```python

```
