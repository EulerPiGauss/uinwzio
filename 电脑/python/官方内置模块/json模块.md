<nav id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline3">1. json存储格式</a>
<ul>
<li><a href="#orgheadline1">1.1. 什么是json</a></li>
<li><a href="#orgheadline2">1.2. json存储格式语法</a></li>
</ul>
</li>
<li><a href="#orgheadline6">2. python的json模块</a>
<ul>
<li><a href="#orgheadline4">2.1. 存储字典值</a></li>
<li><a href="#orgheadline5">2.2. 定义自己的数据类型支持</a></li>
</ul>
</li>
<li><a href="#orgheadline7">3. dumps和loads函数</a></li>
<li><a href="#orgheadline8">4. 参考资料</a></li>
</ul>
</div>
</nav>


# json存储格式<a id="orgheadline3"></a>

## 什么是json<a id="orgheadline1"></a>

json全称是JavaScript Object Notation，也就是JavaScript对象表示法。json是一种基于文本的，人类易读的数据存储交互格式。json文件保存使用后缀 **.json** 。虽然json是从javascript语言衍生出来，不过其作为数据存储和交互是独立于语言的。json和xml作为数据存储和交互方案相比有更易读和读写速度更快的特点。

## json存储格式语法<a id="orgheadline2"></a>

json存储格式的语法很简单，首先是最基本的数字开始，其支持两种数字类型，整数型和浮点型，其对应于python的int和float；字符串在双引号里面，其对应于python的字符串概念；布尔值true和false，其对应于python的True和False，然后还有一个null对应于python的None；json数据用 []表示，里面的元素用逗号分隔，其对应的正是python的列表概念；然后json的object对象用{}包围，其内是key:value这样的形式，其正对应于python的字典概念。

json就可以存储如上描述的这些数据类型，当然可以无限组合下去。这相比较于sql那样单纯的用table表格的形式来描述所有数据已经很灵活了。下面开始实际讲解如何利用python的json模块来读写数据。

# python的json模块<a id="orgheadline6"></a>

python语言已经内置了json模块，所以要读写json文件只需要简单 `import json` 即可。

首先让我们小试牛刀，把[1,2,3,4,5]这组数存( **dump** )进test.json文件里面去。

```python
import json
lst = [1,2,3,4,5]

with open('test.json',mode='w',encoding='utf-8') as f:
    json.dump(lst,f)
```

json不支持元组(tuple)<sup><a id="fnr.1" class="footref" href="#fn.1">1</a></sup>和字节(bytes)类型，bytes类型一般不会去惊扰它，如果有tuple元组你需要存储，那么将其转换成列表即可。

简单的读取是使用的json的 **load** 函数，如下所示：

```python
with open('test.json', mode='r', encoding='utf-8') as f:
    lst2 = json.load(f)
```

这样lst2就被赋值[1,2,3,4,5]了，方便后面的运算。

## 存储字典值<a id="orgheadline4"></a>

上面的例子稍作修改即可以存储字典值了：

```python
import json
dict01 = {'a':1,'b':2,'c':[1,2,3]}

with open('test.json', mode='w', encoding='utf-8') as f:
    json.dump(dict01,f)

with open('test.json', mode='r', encoding='utf-8') as f:
    dict02 = json.load(f)

print(dict02)
```

上面的dump函数那句读者考虑加入 **indent** 选项：

```python
json.dump(dict01,f,indent=4)
```

这样我们的test.json文件里面的数据会更好看一些了。此外 **sort\_keys** 选项有时会很有用，默认是False，如果设置为True，则 <span class="underline">如果是字典值</span> 其将按照键来排序。

上面我们已经看到了json在引入进来的时候顶层元素一般要某是列表要某是字典值，有的程序员喜欢外围是字典值的情况，而有的喜欢外围是列表值的情况。由于受lxml模块的影响，其扫描xml文档，并将每个元素（node）视作字典，然后同一级的元素放入列表中，最大的元素是一个列表。总的来说我更喜欢这种数据表示方法。

## 定义自己的数据类型支持<a id="orgheadline5"></a>

diveintopython3中介绍的time.struct\_time类型失效了，最近版本的（python3.4）默认将类型tuple都转成了列表，而似乎time.struct\_time实际上就是一个元组类型。

下面是关于一个关于复数类型的演示：

```python
def to_json(obj):
    if isinstance(obj, complex):
        return {'__class__': 'complex',
                '__value__': [obj.real, obj.imag]}                       
    raise TypeError(repr(obj) + ' is not JSON serializable')  

def from_json(obj):                                  
    if '__class__' in obj:                            
        if obj['__class__'] == 'complex':
            return complex(obj['__value__'][0],obj['__value__'][1])          
    return obj
```

```python
import json

from wanze.json import to_json, from_json

lst = [{'a':1,'f':7+8j,'c':[1,2,3]},1+1,3+5j]

with open('test.json', mode='w', encoding='utf-8') as f:
    json.dump(lst,f,indent=4,sort_keys=True, default=to_json)

with open('test.json', mode='r', encoding='utf-8') as f:
    lst2 = json.load(f,object_hook=from_json)

print(lst2)
```

**default** 选项连接前面定义的函数，这个函数对额外的对象类型进行转变——变成json可以支持的类型。比如这里的复数类型变成了列表输出，然后读取用 **object\_hook** 函数连接的函数对obj进行额外的处理，这里的 `__class__` 还是你自己加上去的标记，然后return成某个python的值，如果不是则直接 `return obj` ，估计json模块还会有进一步的操作。

然后自定义对象按照上面的方法加入额外的条件判断语句即可，经过测试是可行的，目前还没有实用需求，最默认的这些数据类型觉得就听不错了，所以下面的讨论略过。

# dumps和loads函数<a id="orgheadline7"></a>

此外json模块还有dumps函数其对应dump函数不过没有文件操作，然后loads函数对应load函数不过没有文件操作。

dumps和loads函数可以简单理解为：dumps函数能够简单将python对象字符串化，而loads函数可以简单理解为将某一字符串python对象化。和dump还有load函数比较，其对应的文本数据风格更偏向一行行的相似数据，这些一行行的数据之间彼此并无什么关系（包括简单的顺序关系）；而dump和load函数其对应的文本数据风格更倾向于整个文本数据是一个整体，包括[1,2,3&#x2026;]这样的列表风格数据，其内都暗含顺序关系。

对于上面谈论的一行行独立数据的json文件，推荐使用dumps和loads函数来管理。写入json文件如下所示：

```python
import json

data1 = {'color':'red','count':200}
data2 = {'color':'yellow','count':100}
data3 = {'color':'blue','count':500}

with open('test.json','w') as f:
    for data in [data1,data2,data3]:
        print(json.dumps(data),file=f)
```

然后一行行读取如下所示：

```python
with open('test.json','r') as f:
    data = [json.loads(line) for line in f]
    print(data)
```

之后你可以使用列表解析来单独对每一行的每个元素执行某种操作。

# 参考资料<a id="orgheadline8"></a>

1.  [w3school的json教程](http://www.w3school.com.cn/json/index.asp)
2.  [diveintopython3-serializing](http://www.diveintopython3.net/serializing.html)
3.  [json模块官方文档](https://docs.python.org/3.4/library/json.html)

<div id="footnotes">
<h2 class="footnotes">Footnotes: </h2>
<div id="text-footnotes">

<div class="footdef"><sup><a id="fn.1" class="footnum" href="#fnr.1">1</a></sup> <div class="footpara">原则上不支持，如果你一定要使用也不会报错，其会对应生成json的数组，但如果你再读取json文件，则又会返回列表，也就是元组默认被当作列表处理了。</div></div>


</div>
</div>