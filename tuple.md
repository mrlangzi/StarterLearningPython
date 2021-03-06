# 元组

## 定义

先看一个例子：

    >>># 变量引用 str
    >>> s = "abc"
    >>> s
    'abc'

    >>>#如果这样写，就会是...
    >>> t = 123,'abc',["come","here"]
    >>> t
    (123, 'abc', ['come', 'here'])

上面例子中看到的变量 t，并没有报错，也没有“最后一个有效”，而是将对象做为一个新的数据类型：tuple（元组），赋值给了变量 t。

**元组是用圆括号括起来的，其中的元素之间用逗号隔开。（都是英文半角）**

元组中的元素类型是任意的 Python 数据。

>这种类型，可以歪着想，所谓“元”组，就是用“圆”括号啦。

>其实，你不应该对元组陌生，还记得前面讲述字符串的格式化输出时，有这样一种方式：

    >>> print "I love %s, and I am a %s" % ('python', 'programmer')
    I love Python, and I am a programmer

>这里的圆括号，就是一个元组。

显然，tuple 是一种序列类型的数据，这点上跟 list/str 类似。它的特点就是其中的元素不能更改，这点上跟 list 不同，倒是跟 str 类似；它的元素又可以是任何类型的数据，这点上跟 list 相同，但不同于 str。

    >>> t = 1,"23",[123,"abc"],("python","learn")   #元素多样性，近 list
    >>> t
    (1, '23', [123, 'abc'], ('python', 'learn'))

    >>> t[0] = 8　                                  #不能原地修改，近 str
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment

    >>> t.append("no")  
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'tuple' object has no attribute 'append'
        >>> 

从上面的简单比较似乎可以认为，tuple 就是一个融合了部分 list 和部分 str 属性的杂交产物。此言有理。

## 索引和切片

因为前面有了关于列表和字符串的知识，它们都是序列类型，元组也是。因此，元组的基本操作就和它们是一样的。

例如：

    >>> t
    (1, '23', [123, 'abc'], ('python', 'learn'))
    >>> t[2]
    [123, 'abc']
    >>> t[1:]
    ('23', [123, 'abc'], ('python', 'learn'))
    
    >>> t[2][0]     #还能这样呀，哦对了，list 中也能这样
    123
    >>> t[3][1]
    'learn'

关于序列的基本操作在 tuple 上的表现，就不一一展示了。看官可以去试试。

但是这里要特别提醒，如果一个元组中只有一个元素的时候，应该在该元素后面加一个半角的英文逗号。

    >>> a = (3)
    >>> type(a)
    <type 'int'>
    
    >>> b = (3,)
    >>> type(b)
    <type 'tuple'>

以上面的例子说明，如果不加那个逗号，就不是元组，加了才是。这也是为了避免让 Python 误解你要表达的内容。

顺便补充：如果要想看一个对象是什么类型，可以使用 `type()`函数，然后就返回该对象的类型。

**所有在 list 中可以修改 list 的方法，在 tuple 中，都失效。**

分别用 list()和 tuple()能够实现两者的转化:

    >>> t         
    (1, '23', [123, 'abc'], ('python', 'learn'))
    >>> tls = list(t)                           #tuple-->list
    >>> tls
    [1, '23', [123, 'abc'], ('python', 'learn')]

    >>> t_tuple = tuple(tls)                    #list-->tuple
    >>> t_tuple
    (1, '23', [123, 'abc'], ('python', 'learn'))

## tuple 用在哪里？

既然它是 list 和 str 的杂合，它有什么用途呢？不是用 list 和 str 都可以了吗？

在很多时候，的确是用 list 和 str 都可以了。但是，看官不要忘记，我们用计算机语言解决的问题不都是简单问题，就如同我们的自然语言一样，虽然有的词汇看似可有可无,用别的也能替换之,但是我们依然需要在某些情况下使用它们.

一般认为,tuple 有这类特点,并且也是它使用的情景:

- Tuple 比 list 操作速度快。如果您定义了一个值的常量集，并且唯一要用它做的是不断地遍历它，请使用 tuple 代替 list。
- 如果对不需要修改的数据进行 “写保护”，可以使代码更安全。使用 tuple 而不是 list 如同拥有一个隐含的 assert 语句，说明这一数据是常量。如果必须要改变这些值，则需要执行 tuple 到 list 的转换 (需要使用一个特殊的函数)。
- Tuples 可以在 dictionary（字典，后面要讲述） 中被用做 key，但是 list 不行。Dictionary key 必须是不可变的。Tuple 本身是不可改变的，但是如果您有一个 list 的 tuple，那就认为是可变的了，用做 dictionary key 就是不安全的。只有字符串、整数或其它对 dictionary 安全的 tuple 才可以用作 dictionary key。
- Tuples 可以用在字符串格式化中。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：回顾 list 和 str](./114.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：字典(1)](./116.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。
