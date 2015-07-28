# 字典(2)

## 字典方法

跟前面所讲述的其它数据类型类似，字典也有一些方法。通过这些方法，能够实现对字典类型数据的操作。这回可不是屠龙之技的。这些方法在编程实践中经常会用到。

### copy

拷贝，这个汉语是 copy 的音译，标准的汉语翻译是“复制”。我还记得当初在学 DOS 的时候，那个老师说“拷贝”，搞得我晕头转向，他没有说英文的“copy”发音，而是用标准汉语说“kao(三声)bei(四声)”，对于一直学习过英语、标准汉语和我家乡方言的人来说，理解“拷贝”是有点困难的。谁知道在编程界用的是音译呢。

在一般的理解中，copy 就是将原来的东西再搞一份。但是，在 Python 里面（乃至于很多编程语言中），copy 可不是那么简单的。

    >>> a = 5
    >>> b = a
    >>> b
    5
    
这样做，是不是就得到了两个 5 了呢？表面上看似乎是，但是，不要忘记我在前面反复提到的：**对象有类型，变量无类型**，正是因着这句话，变量其实是一个标签。不妨请出法宝：`id()`，专门查看内存中对象编号

    >>> id(a)
    139774080
    >>> id(b)
    139774080

果然，并没有两个 5，就一个，只不过是贴了两张标签而已。这种现象普遍存在于 Python 的多种数据类型中。其它的就不演示了，就仅看看 dict 类型。

    >>> ad = {"name":"qiwsir", "lang":"Python"}
    >>> bd = ad
    >>> bd
    {'lang': 'Python', 'name': 'qiwsir'}
    >>> id(ad)
    3072239652L
    >>> id(bd)
    3072239652L

是的，验证了。的确是一个对象贴了两个标签。这是用赋值的方式，实现的所谓“假装拷贝”。那么如果用 copy 方法呢？

    >>> cd = ad.copy()
    >>> cd
    {'lang': 'Python', 'name': 'qiwsir'}
    >>> id(cd)
    3072239788L

果然不同，这次得到的 cd 是跟原来的 ad 不同的，它在内存中另辟了一个空间。如果我尝试修改 cd，就应该对原来的 ad 不会造成任何影响。

    >>> cd["name"] = "itdiffer.com"
    >>> cd 
    {'lang': 'Python', 'name': 'itdiffer.com'}
    >>> ad
    {'lang': 'Python', 'name': 'qiwsir'}

真的是那样，跟推理一模一样。所以，要理解了“变量”是对象的标签，对象有类型而变量无类型，就能正确推断出 Python 能够提供的结果。
    
    >>> bd
    {'lang': 'Python', 'name': 'qiwsir'}
    >>> bd["name"] = "laoqi"
    >>> ad
    {'lang': 'Python', 'name': 'laoqi'}
    >>> bd
    {'lang': 'Python', 'name': 'laoqi'}

这是又修改了 bd 所对应的“对象”，结果发现 ad 的“对象”也变了。

然而，事情没有那么简单，看下面的，要仔细点，否则就迷茫了。

    >>> x = {"name":"qiwsir", "lang":["Python", "java", "c"]}
    >>> y = x.copy()
    >>> y
    {'lang': ['Python', 'java', 'c'], 'name': 'qiwsir'}
    >>> id(x)
    3072241012L
    >>> id(y)
    3072241284L
    
y 是从 x 拷贝过来的，两个在内存中是不同的对象。

    >>> y["lang"].remove("c")
    
为了便于理解，尽量使用短句子，避免用很长很长的复合句。在 y 所对应的 dict 对象中，键"lang"的值是一个列表，为['Python', 'java', 'c']，这里用 `remove()`这个列表方法删除其中的一个元素"c"。删除之后，这个列表变为：['Python', 'java']

    >>> y
    {'lang': ['Python', 'java'], 'name': 'qiwsir'}

果然不出所料。那么，那个x所对应的字典中，这个列表变化了吗？应该没有变化。因为按照前面所讲的，它是另外一个对象，两个互不干扰。

    >>> x
    {'lang': ['Python', 'java'], 'name': 'qiwsir'}

是不是有点出乎意料呢？我没有作弊哦。你如果不信，就按照操作自己在交互模式中试试，是不是能够得到这个结果呢？这是为什么？

但是，如果要操作另外一个键值对：
    
    >>> y["name"] = "laoqi"
    >>> y
    {'lang': ['python', 'java'], 'name': 'laoqi'}
    >>> x
    {'lang': ['python', 'java'], 'name': 'qiwsir'}

前面所说的原理是有效的，为什么到值是列表的时候就不奏效了呢？

要破解这个迷局还得用 `id()`

    >>> id(x)
    3072241012L
    >>> id(y)
    3072241284L

x,y 对应着两个不同对象，的确如此。但这个对象（字典）是由两个键值对组成的。其中一个键的值是列表。

    >>> id(x["lang"])
    3072243276L
    >>> id(y["lang"])
    3072243276L
    
发现了这样一个事实，列表是同一个对象。

但是，作为字符串为值得那个键值对，是分属不同对象。

    >>> id(x["name"])
    3072245184L
    >>> id(y["name"])
    3072245408L

这个事实，就说明了为什么修改一个列表，另外一个也跟着修改；而修改一个的字符串，另外一个不跟随的原因了。

但是，似乎还没有解开深层的原因。深层的原因，这跟 Python 存储的数据类型特点有关，Python 只存储基本类型的数据，比如 int,str，对于不是基础类型的，比如刚才字典的值是列表，Python 不会在被复制的那个对象中重新存储，而是用引用的方式，指向原来的值。如果读者没有明白这句话的意思，我就只能说点通俗的了（我本来不想说通俗的，装着自己有学问），Python 在所执行的复制动作中，如果是基本类型的数据，就在内存中重新建个窝，如果不是基本类型的，就不新建窝了，而是用标签引用原来的窝。这也好理解，如果比较简单，随便建立新窝简单；但是，如果对象太复杂了，就别费劲了，还是引用一下原来的省事。（这么讲有点忽悠了）。

所以，在编程语言中，把实现上面那种拷贝的方式称之为“浅拷贝”。顾名思义，没有解决深层次问题。言外之意，还有能够解决深层次问题的方法喽。

的确是，在 Python 中，有一个“深拷贝”(deep copy)。不过，要用下一 `import` 来导入一个模块。这个东西后面会讲述，前面也遇到过了。

    >>> import copy
    >>> z = copy.deepcopy(x)
    >>> z
    {'lang': ['python', 'java'], 'name': 'qiwsir'}

用 `copy.deepcopy()`深拷贝了一个新的副本，看这个函数的名字就知道是深拷贝(deepcopy)。用上面用过的武器 id()来勘察一番：   
    
    >>> id(x["lang"])
    3072243276L
    >>> id(z["lang"])
    3072245068L

果然是另外一个“窝”，不是引用了。如果按照这个结果，修改其中一个列表中的元素，应该不影响另外一个了。
    
    >>> x
    {'lang': ['Python', 'java'], 'name': 'qiwsir'}
    >>> x["lang"].remove("java")
    >>> x
    {'lang': ['Python'], 'name': 'qiwsir'}
    >>> z
    {'lang': ['Python', 'java'], 'name': 'qiwsir'}

果然如此。再试试，才过瘾呀。
    
    >>> x["lang"].append("c++")
    >>> x
    {'lang': ['Python', 'c++'], 'name': 'qiwsir'}
  
这就是所谓浅拷贝和深拷贝。

### clear

在交互模式中，用 help 是一个很好的习惯

    >>> help(dict.clear)

    clear(...)
        D.clear() -> None.  Remove all items from D.

这是一个清空字典中所有元素的操作。

    >>> a = {"name":"qiwsir"}
    >>> a.clear()
    >>> a
    {}
    
这就是 `clear` 的含义，将字典清空，得到的是“空”字典。这个上节说的 `del` 有着很大的区别。`del` 是将字典删除，内存中就没有它了，不是为“空”。

    >>> del a
    >>> a
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'a' is not defined

果然删除了。

另外，如果要清空一个字典，还能够使用 `a = {}`这种方法，但这种方法本质是将变量 a 转向了`{}`这个对象，那么原来的呢？原来的成为了断线的风筝。这样的东西在 Python 中称之为垃圾，而且 Python 能够自动的将这样的垃圾回收。编程者就不用关心它了，反正 Python 会处理了。

### get,setdefault

get 的含义是：

    get(...)
        D.get(k[,d]) -> D[k] if k in D, else d.  d defaults to None.

注意这个说明中，“if k in D”，就返回其值，否则...(等会再说)。

    >>> d
    {'lang': 'python'}
    >>> d.get("lang")
    'python'

`dict.get()`就是要得到字典中某个键的值，不过，它不是那么“严厉”罢了。因为类似获得字典中键的值得方法，上节已经有过，如 `d['lang']`就能得到对应的值`"Python"`，可是，如果要获取的键不存在，如：
    
    >>> print d.get("name")
    None
    
    >>> d["name"]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    KeyError: 'name'

这就是 `dict.get()`和 `dict['key']`的区别。

前面有一个半句话，如果键不在字典中，会返回 None，这是一种情况。还可以这样：

    >>> d = {"lang":"Python"}
    >>> newd = d.get("name",'qiwsir')
    >>> newd
    'qiwsir'
    >>> d
    {'lang': 'Python'}

以 `d.get("name",'qiwsir')`的方式，如果不能得到键"name"的值，就返回后面指定的值"qiwsir"。这就是文档中那句话：`D[k] if k in D, else d.`的含义。这样做，并没有影响原来的字典。

另外一个跟 get 在功能上有相似地方的 `D.setdefault(k)`，其含义是：

    setdefault(...)
        D.setdefault(k[,d]) -> D.get(k,d), also set D[k]=d if k not in D

首先，它要执行 `D.get(k,d)`,就跟前面一样了，然后，进一步执行另外一个操作，如果键k不在字典中，就在字典中增加这个键值对。当然，如果有就没有必要执行这一步了。

    >>> d
    {'lang': 'Python'}
    >>> d.setdefault("lang")
    'Python'

在字典中，有"lang"这个键，那么就返回它的值。

    >>> d.setdefault("name","qiwsir")
    'qiwsir'
    >>> d
    {'lang': 'Python', 'name': 'qiwsir'}
    
没有"name"这个键，于是返回 `d.setdefault("name","qiwsir")`指定的值"qiwsir"，并且将键值对`'name':"qiwsir"`添加到原来的字典中。

如果这样操作：
  
    >>> d.setdefault("web")
    
什么也没有返回吗？不是，返回了，只不过没有显示出来，如果你用 print 就能看到了。因为这里返回的是一个 None.不妨查看一下那个字典。

    >>> d
    {'lang': 'Python', 'web': None, 'name': 'qiwsir'}
    
是不是键"web"的值成为了 None


### items/iteritems, keys/iterkeys, values/itervalues

这个标题中列出的是三组 dict 的函数，并且这三组有相似的地方。在这里详细讲述第一组，其余两组，我想凭借读者的聪明智慧是不在话下的。

    >>> help(dict.items)

    items(...)
        D.items() -> list of D's (key, value) pairs, as 2-tuples

这种方法是惯用的伎俩了，只要在交互模式中鼓捣一下，就能得到帮助信息。从中就知道 `D.items()`能够得到一个关于字典的列表，列表中的元素是由字典中的键和值组成的元组。例如：

    >>> dd = {"name":"qiwsir", "lang":"python", "web":"www.itdiffer.com"}
    >>> dd_kv = dd.items()
    >>> dd_kv
    [('lang', 'Python'), ('web', 'www.itdiffer.com'), ('name', 'qiwsir')]

显然，是有返回值的。这个操作，在后面要讲到的循环中，将有很大的作用。

跟 `items` 类似的就是 `iteritems`，看这个词的特点，是由 iter 和 items 拼接而成的，后部分 items 就不用说了，肯定是在告诉我们，得到的结果跟 `D.items()`的结果类似。是的，但是，还有一个 iter 是什么意思？在[《列表(2)](./112.md)中，我提到了一个词“iterable”，它的含义是“可迭代的”，这里的 iter 是指的名词 iterator 的前部分，意思是“迭代器”。合起来，"iteritems"的含义就是：

    iteritems(...)
        D.iteritems() -> an iterator over the (key, value) items of D

你看，学习 Python 不是什么难事，只要充分使用帮助文档就好了。这里告诉我们，得到的是一个“迭代器”（关于什么是迭代器，以及相关的内容，后续会详细讲述），这个迭代器是关于“D.items()”的。看个例子就明白了。

    >>> dd
    {'lang': 'Python', 'web': 'www.itdiffer.com', 'name': 'qiwsir'}
    >>> dd_iter = dd.iteritems()
    >>> type(dd_iter)
    <type 'dictionary-itemiterator'>
    >>> dd_iter
    <dictionary-itemiterator object at 0xb72b9a2c>
    >>> list(dd_iter)
    [('lang', 'Python'), ('web', 'www.itdiffer.com'), ('name', 'qiwsir')]

得到的 dd_iter 的类型，是一个'dictionary-itemiterator'类型，不过这种迭代器类型的数据不能直接输出，必须用 `list()`转换一下，才能看到里面的真面目。

另外两组，含义跟这个相似，只不过是得到 key 或者 value。下面仅列举一下例子，具体内容，读者可以自行在交互模式中看文档。

    >>> dd
    {'lang': 'Python', 'web': 'www.itdiffer.com', 'name': 'qiwsir'}
    >>> dd.keys()
    ['lang', 'web', 'name']
    >>> dd.values()
    ['Python', 'www.itdiffer.com', 'qiwsir']

这里先交代一句，如果要实现对键值对或者键或者值的循环，用迭代器的效率会高一些。对这句话的理解，在后面会给大家进行详细分析。

### pop, popitem

在[《列表(3)》](./113.md)中，有关于删除列表中元素的函数 `pop` 和 `remove`，这两个的区别在于 `list.remove(x)`用来删除指定的元素，而 `list.pop([i])`用于删除指定索引的元素，如果不提供索引值，就默认删除最后一个。

在字典中，也有删除键值对的函数。

    pop(...)
        D.pop(k[,d]) -> v, remove specified key and return the corresponding value.
        If key is not found, d is returned if given, otherwise KeyError is raised

`D.pop(k[,d])`是以字典的键为参数，删除指定键的键值对，当然，如果输入对应的值也可以，那个是可选的。

    >>> dd
    {'lang': 'Python', 'web': 'www.itdiffer.com', 'name': 'qiwsir'}
    >>> dd.pop("name")
    'qiwsir'
    
要删除指定键"name"，返回了其值"qiwsir"。这样，在原字典中，“'name':'qiwsir'”这个键值对就被删除了。

    >>> dd
    {'lang': 'Python', 'web': 'www.itdiffer.com'}

值得注意的是，pop 函数中的参数是不能省略的，这跟列表中的那个 pop 有所不同。

    >>> dd.pop()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: pop expected at least 1 arguments, got 0

如果要删除字典中没有的键值对，也会报错。
    
    >>> dd.pop("name")
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    KeyError: 'name'

有意思的是 `D.popitem()`倒是跟 `list.pop()`有相似之处，不用写参数（list.pop 是可以不写参数），但是，`D.popitem()`不是删除最后一个，前面已经交代过了，dict 没有顺序，也就没有最后和最先了，它是随机删除一个，并将所删除的返回。

    popitem(...)
        D.popitem() -> (k, v), remove and return some (key, value) pair as a 
        2-tuple; but raise KeyError if D is empty.

如果字典是空的，就要报错了

    >>> dd
    {'lang': 'Python', 'web': 'www.itdiffer.com'}
    >>> dd.popitem()
    ('lang', 'Python')
    >>> dd
    {'web': 'www.itdiffer.com'}

成功地删除了一对，注意是随机的，不是删除前面显示的最后一个。并且返回了删除的内容，返回的数据格式是 tuple
    
    >>> dd.popitems()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'dict' object has no attribute 'popitems'

错了？！注意看提示信息，没有那个...，哦，果然错了。注意是 popitem，不要多了 s，前面的 `D.items()`中包含 s，是复数形式，说明它能够返回多个结果（多个元组组成的列表），而在 `D.popitem()`中，一次只能随机删除一对键值对，并以一个元组的形式返回，所以，要单数形式，不能用复数形式了。

    >>> dd.popitem()
    ('web', 'www.itdiffer.com')
    >>> dd 
    {}
    
都删了，现在那个字典成空的了。如果再删，会怎么样？

    >>> dd.popitem()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    KeyError: 'popitem(): dictionary is empty'

报错信息中明确告知，字典已经是空的了，没有再供删的东西了。

### update

`update()`,看名字就猜测到一二了，是不是更新字典内容呢？的确是。

    update(...)
        D.update([E, ]**F) -> None.  Update D from dict/iterable E and F.
        If E present and has a .keys() method, does:     for k in E: D[k] = E[k]
        If E present and lacks .keys() method, does:     for (k, v) in E: D[k] = v
        In either case, this is followed by: for k in F: D[k] = F[k]

不过，看样子这个函数有点复杂。不要着急，通过实验可以一点一点鼓捣明白的。

首先，这个函数没有返回值，或者说返回值是 None,它的作用就是更新字典。其参数可以是字典或者某种可迭代的数据类型。

    >>> d1 = {"lang":"python"}
    >>> d2 = {"song":"I dreamed a dream"}
    >>> d1.update(d2)
    >>> d1
    {'lang': 'Python', 'song': 'I dreamed a dream'}
    >>> d2
    {'song': 'I dreamed a dream'}

这样就把字典 d2 更新入了 d1 那个字典，于是 d1 中就多了一些内容，把 d2 的内容包含进来了。d2 当然还存在，并没有受到影响。

还可以用下面的方法更新：

    >>> d2
    {'song': 'I dreamed a dream'}
    >>> d2.update([("name","qiwsir"), ("web","itdiffer.com")])
    >>> d2
    {'web': 'itdiffer.com', 'name': 'qiwsir', 'song': 'I dreamed a dream'}

列表的元组是键值对。

### has_key

这个函数的功能是判断字典中是否存在某个键

    has_key(...)
        D.has_key(k) -> True if D has a key k, else False

跟前一节中遇到的 `k in D` 类似。

    >>> d2
    {'web': 'itdiffer.com', 'name': 'qiwsir', 'song': 'I dreamed a dream'}
    >>> d2.has_key("web")
    True
    >>> "web" in d2
    True

关于 dict 的函数，似乎不少。但是，不用着急，也不用担心记不住，因为根本不需要记忆。只要会用搜索即可。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：字典(1)](./116.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：集合(1)](./118.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。
