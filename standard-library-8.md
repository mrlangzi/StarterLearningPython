# 标准库 (8)

## json

就传递数据而言，xml 是一种选择，还有另外一种，就是 json，它是一种轻量级的数据交换格式，如果读者要做 web 编程，是会用到它的。根据维基百科的相关内容，对 json 了解一二：

>JSON（JavaScript Object Notation）是一种由道格拉斯·克罗克福特构想设计、轻量级的资料交换语言，以文字为基础，且易于让人阅读。尽管 JSON 是 Javascript 的一个子集，但 JSON 是独立于语言的文本格式，並且采用了类似于 C 语言家族的一些习惯。

关于 json 更为详细的内容，可以参考其官方网站：<http://www.json.org>

从官方网站上摘取部分，了解一下 json 的结构：

>JSON 建构于两种结构：

- “名称/值”对的集合（A collection of name/value pairs）。不同的语言中，它被理解为对象（object），纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。
- 值的有序列表（An ordered list of values）。在大部分语言中，它被理解为数组（array）。

python 标准库中有 json 模块，主要是执行序列化和反序列化功能：

- 序列化：encoding，把一个 Python 对象编码转化成 json 字符串
- 反序列化：decoding，把 json 格式字符串解码转换为 Python 数据对象

### 基本操作

json 模块相对 xml 单纯了很多：

    >>> import json
    >>> json.__all__
    ['dump', 'dumps', 'load', 'loads', 'JSONDecoder', 'JSONEncoder']

**encoding: dumps()**

    >>> data = [{"name":"qiwsir", "lang":("python", "english"), "age":40}]
    >>> print data
    [{'lang': ('python', 'english'), 'age': 40, 'name': 'qiwsir'}]
    >>> data_json = json.dumps(data)
    >>> print data_json
    [{"lang": ["python", "english"], "age": 40, "name": "qiwsir"}]

encoding 的操作是比较简单的，请注意观察 data 和 data_json 的不同——lang 的值从元组编程了列表，还有不同：
    
    >>> type(data_json)
    <type 'str'>
    >>> type(data)
    <type 'list'>
    
将 Python 对象转化为 json 类型，是按照下表所示对照关系转化的：

|Python==>|json|
|------|----|
|dict|object|
|list, tuple|array|
|str, unicode|string|
|int, long, float|number|
|True|true|
|False|false|
|None|null|

**decoding: loads()**

decoding 的过程也像上面一样简单：

    >>> new_data = json.loads(data_json)
    >>> new_data
    [{u'lang': [u'python', u'english'], u'age': 40, u'name': u'qiwsir'}]

需要注意的是，解码之后，并没有将元组还原。

解码的数据类型对应关系：

|json==>|Python|
|-------|------|
|object|dict|
|array|list|
|string|unicode|
|number(int)|int, long|
|number(real)|float|
|true|True|
|false|False|
|null|None|

**对人友好**

上面的 data 都不是很长，还能凑合阅读，如果很长了，阅读就有难度了。所以，json 的 dumps() 提供了可选参数，利用它们能在输出上对人更友好（这对机器是无所谓的）。

    >>> data_j = json.dumps(data, sort_keys=True, indent=2)
    >>> print data_j
    [
      {
        "age": 40, 
        "lang": [
          "python", 
          "english"
        ], 
        "name": "qiwsir"
      }
    ]

`sort_keys=True` 意思是按照键的字典顺序排序，`indent=2` 是让每个键值对显示的时候，以缩进两个字符对齐。这样的视觉效果好多了。

### 大 json 字符串

如果数据不是很大，上面的操作足够了。但是，上面操作是将数据都读入内存，如果太大就不行了。怎么办？json 提供了 `load()` 和 `dump()` 函数解决这个问题，注意，跟上面已经用过的函数相比，是不同的，请仔细观察。

    >>> import tempfile    #临时文件模块
    >>> data
    [{'lang': ('Python', 'english'), 'age': 40, 'name': 'qiwsir'}]
    >>> f = tempfile.NamedTemporaryFile(mode='w+')
    >>> json.dump(data, f)
    >>> f.flush()
    >>> print open(f.name, "r").read()
    [{"lang": ["Python", "english"], "age": 40, "name": "qiwsir"}]

### 自定义数据类型

一般情况下，用的数据类型都是 Python 默认的。但是，我们学习过类后，就知道，自己可以定义对象类型的。比如：

以下代码参考：[Json 概述以及 Python 对 json 的相关操作](http://www.cnblogs.com/coser/archive/2011/12/14/2287739.html)

    #!/usr/bin/env Python
    # coding=utf-8

    import json

    class Person(object):
        def __init__(self,name,age):
            self.name = name
            self.age = age
    
        def __repr__(self):
            return 'Person Object name : %s , age : %d' % (self.name,self.age)


    def object2dict(obj):    #convert Person to dict
        d = {}
        d['__class__'] = obj.__class__.__name__
        d['__module__'] = obj.__module__
        d.update(obj.__dict__)
        return d

    def dict2object(d):     #convert dict ot Person
        if '__class__' in d:
            class_name = d.pop('__class__')
            module_name = d.pop('__module__')
            module = __import__(module_name)
            class_ = getattr(module, class_name)
            args = dict((key.encode('ascii'), value) for key,value in d.items())    #get args
            inst = class_(**args)    #create new instance
        else:
            inst = d
        return inst
    

    if __name__  == '__main__':
        p = Person('Peter',40)
        print p
        d = object2dict(p)
        print d
        o = dict2object(d)
        print type(o), o

        dump = json.dumps(p, default=object2dict)
        print dump
        load = json.loads(dump, object_hook=dict2object)
        print load

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：标准库(7)](./226.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：第三方库](./228.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。