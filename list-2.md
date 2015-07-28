# 列表(2)

上一节中已经谈到，list 是 Python 的苦力，那么它都有哪些函数呢？或者它或者对它能做什么呢？在交互模式下这么操作，就看到有关它的函数了。

    >>> dir(list)
    ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']

上面的结果中，以双下划线开始和结尾的暂时不管，如`__add__`（以后会管的）。就剩下以下几个了：

>'append', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort'

下面注意对这些函数进行说明和演示。这都是在编程实践中常常要用到的。

## list 函数

### append 和 extend

[《列表(1)》](./111.md)中，对 list 的基本操作提到了 list.append(x)，也就是将某个元素 x 追加到已知的一个 list 后边。

除了将元素追加到 list 中，还能够将两个 list 合并，或者说将一个 list 追加到另外一个 list 中。按照前文的惯例，还是首先看[官方文档](https://docs.Python.org/2/tutorial/datastructures.html)中的描述：

>list.extend(L)

>    Extend the list by appending all the items in the given list; equivalent to a[len(a):] = L.

**向所有正在学习本内容的朋友提供一个成为优秀程序员的必备：看官方文档，是必须的。**

官方文档的这句话翻译过来：

>通过将所有元素追加到已知 list 来扩充它，相当于 a[len(a):]= L

英语太烂，翻译太差。直接看例子，更明白

    >>> la
    [1, 2, 3]
    >>> lb
    ['qiwsir', 'python']
    >>> la.extend(lb)
    >>> la
    [1, 2, 3, 'qiwsir', 'python']
    >>> lb
    ['qiwsir', 'python']

上面的例子，显示了如何将两个 list，一个是 la，另外一个 lb，将 lb 追加到 la 的后面，也就是把 lb 中的所有元素加入到 la 中，即让 la 扩容。

学程序一定要有好奇心，我在交互环境中，经常实验一下自己的想法，有时候是比较愚蠢的想法。

    >>> la = [1,2,3]
    >>> b = "abc"
    >>> la.extend(b)
    >>> la
    [1, 2, 3, 'a', 'b', 'c']
    >>> c = 5
    >>> la.extend(c)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      TypeError: 'int' object is not iterable

从上面的实验中，看官能够有什么心得？原来，如果 extend(str)的时候，str 被以字符为单位拆开，然后追加到 la 里面。

如果 extend 的对象是数值型，则报错。

所以，extend 的对象是一个 list，如果是 str，则 Python 会先把它按照字符为单位转化为 list 再追加到已知 list。

不过，别忘记了前面官方文档的后半句话，它的意思是：

    >>> la
    [1, 2, 3, 'a', 'b', 'c']
    >>> lb
    ['qiwsir', 'python']
    >>> la[len(la):]=lb
    >>> la
    [1, 2, 3, 'a', 'b', 'c', 'qiwsir', 'python']

list.extend(L) 等效于 list[len(list):] = L，L是待并入的 list

联想到到[上一讲](./111.md)中的一个 list 函数 list.append(),有类似之处。

>extend(...)
>    L.extend(iterable) -- extend list by appending elements from the iterable

上面是在交互模式中输入 `help(list.extend)`后得到的说明。这是非常重要而且简单的获得文档帮助的方法。

从上面内容可知，extend 函数也是将另外的元素增加到一个已知列表中，其元素必须是 iterable，什么是 iterable？这个从现在开始，后面会经常遇到，所以是要搞搞清楚的。

>iterable,中文含义是“可迭代的”。在 Python 中，还有一个词，就是 iterator，这个叫做“迭代器”。这两者有着区别和联系。不过，这里暂且不说那么多，说多了就容易糊涂，我也糊涂了。

为了解释 iterable(可迭代的)，又引入了一个词“迭代”，什么是迭代呢？

>尽管我们很多文档是用英文写的，但是，如果你能充分利用汉语来理解某些名词，是非常有帮助的。因为在汉语中，不仅仅表音，而且能从词语组合中体会到该术语的含义。比如“激光”，这是汉语。英语是从"light amplification by stimulated emission of radiation"化出来的"laser"，它是一个造出来的词。因为此前人们不知道那种条件下发出来的是什么。但是汉语不然，反正用一个“光”就可以概括了，只不过这个“光”不是传统概念中的“光”，而是由于“受激”辐射得到的光，故名“激光”。是不是汉语很牛叉？

>“迭”在汉语中的意思是“屡次,反复”。如:高潮迭起。那么跟“代”组合，就可以理解为“反复‘代’”，是不是有点“子子孙孙”的意思了？“结婚-生子-子成长-结婚-生子-子成长-...”，你是不是也在这个“迭代”的过程中呢？

>给个稍微严格的定义，来自维基百科。“迭代是重复反馈过程的活动，其目的通常是为了接近并到达所需的目标或结果。”

某些类型的对象是“可迭代”(iterable)的，这类数据类型有共同的特点。如何判断一个对象是不是可迭代的？下面演示一种方法。事实上还有别的方式。

    >>> astr = "Python"
    >>> hasattr(astr,'__iter__')
    False
    
这里用内建函数 `hasattr()`判断一个字符串是否是可迭代的，返回了 False。用同样的方式可以判断：

    >>> alst = [1,2]
    >>> hasattr(alst,'__iter__')
    True
    >>> hasattr(3, '__iter__')
    False

`hasattr()`的判断本质就是看那个类型中是否有`__iter__`函数。看官可以用 `dir()`找一找，在数字、字符串、列表中，谁有`__iter__`。同样还可找一找 dict,tuple 两种类型对象是否含有这个方法。

以上穿插了一个新的概念“iterable”（可迭代的），现在回到 extend 上。这个函数需要的参数就是 iterable 类型的对象。

    >>> new = [1,2,3]
    >>> lst = ['Python','qiwsir']
    >>> lst.extend(new)
    >>> lst
    ['Python', 'qiwsir', 1, 2, 3]
    >>> new
    [1, 2, 3]

通过 extend 函数，将[1,2,3]中的每个元素都拿出来，然后塞到 lst 里面，从而得到了一个跟原来的对象元素不一样的列表，后面的比原来的多了三个元素。上面说的有点啰嗦，只不过是为了把过程完整表达出来。

还要关注一下，从上面的演示中可以看出，lst 经过 extend 函数操作之后，变成了一个貌似“新”的列表。这句话好像有点别扭，“貌似新”的，之所以这么说，是因为对“新的”可能有不同的理解。不妨深挖一下。

    >>> new = [1,2,3]
    >>> id(new)
    3072383244L

    >>> lst = ['python', 'qiwsir']
    >>> id(lst)
    3069501420L

用 `id()`能够看到两个列表分别在内存中的“窝”的编号。

    >>> lst.extend(new)
    >>> lst
    ['python', 'qiwsir', 1, 2, 3]
    >>> id(lst)
    3069501420L

看官注意到没有，虽然 lst 经过 `extend()`方法之后，比原来扩容了，但是，并没有离开原来的“窝”，也就是在内存中，还是“旧”的，只不过里面的内容增多了。相当于两口之家，经过一番云雨之后，又增加了一个小宝宝，那么这个家是“新”的还是“旧”的呢？角度不同或许说法不一了。

这就是列表的一个**重要特征：列表是可以修改的。这种修改，不是复制一个新的，而是在原地进行修改。**

其实，`append()`对列表的操作也是如此，不妨用同样的方式看看。

**说明：**虽然这里的 lst 内容和上面的一样，但是，我从新在 shell 中输入，所以 id 会变化。也就是内存分配的“窝”的编号变了。

    >>> lst = ['Python','qiwsir']
    >>> id(lst)     
    3069501388L
    >>> lst.append(new)
    >>> lst
    ['Python', 'qiwsir', [1, 2, 3]]
    >>> id(lst)
    3069501388L

显然，`append()`也是原地修改列表。

如果，对于 `extend()`，提供的不是 iterable 类型对象，会如何呢？

    >>> lst.extend("itdiffer")
    >>> lst
    ['python', 'qiwsir', 'i', 't', 'd', 'i', 'f', 'f', 'e', 'r']

它把一个字符串"itdiffer"转化为['i', 't', 'd', 'i', 'f', 'f', 'e', 'r']，然后将这个列表作为参数，提供给 extend，并将列表中的元素塞入原来的列表中。

    >>> num_lst = [1,2,3]
    >>> num_lst.extend(8)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'int' object is not iterable

这就报错了。错误提示中告诉我们，那个数字 8，是 int 类型的对象，不是 iterable 的。

这里讲述的两个让列表扩容的函数 `append()`和 `extend()`。从上面的演示中，可以看到他们有相同的地方：

- 都是原地修改列表
- 既然是原地修改，就不返回值

原地修改没有返回值，就不能赋值给某个变量。

    >>> one = ["good","good","study"]
    >>> another = one.extend(["day","day","up"])    #对于没有提供返回值的函数，如果要这样，结果是：
    >>> another                                     #这样的，什么也没有得到。
    >>> one
    ['good', 'good', 'study', 'day', 'day', 'up']
    
那么两者有什么不一样呢？看下面例子：

    >>> lst = [1,2,3]
    >>> lst.append(["qiwsir","github"])
    >>> lst
    [1, 2, 3, ['qiwsir', 'github']]  #append 的结果
    >>> len(lst)
    4
    
    >>> lst2 = [1,2,3]
    >>> lst2.extend(["qiwsir","github"])
    >>> lst2
    [1, 2, 3, 'qiwsir', 'github']   #extend 的结果
    >>> len(lst2)
    5

append 是整建制地追加，extend 是个体化扩编。

### count

上面的 len(L)，可得到 list 的长度，也就是 list 中有多少个元素。python 的 list 还有一个函数，就是数一数某个元素在该 list 中出现多少次，也就是某个元素有多少个。官方文档是这么说的：

>list.count(x)

>Return the number of times x appears in the list.

一定要不断实验，才能理解文档中精炼的表达。

    >>> la = [1,2,1,1,3]
    >>> la.count(1)
    3
    >>> la.append('a')
    >>> la.append('a')
    >>> la
    [1, 2, 1, 1, 3, 'a', 'a']
    >>> la.count('a')
    2
    >>> la.count(2)
    1
    >>> la.count(5)     #NOTE:la 中没有 5,但是如果用这种方法找，不报错，返回的是数字 0
    0

### index

[《列表(1)》](./111.md)中已经提到，这里不赘述，但是为了完整，也占个位置吧。

    >>> la
    [1, 2, 3, 'a', 'b', 'c', 'qiwsir', 'python']
    >>> la.index(3)
    2
    >>> la.index('qi')      #如果不存在，就报错
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      ValueError: 'qi' is not in list
    >>> la.index('qiwsir')
    6

list.index(x)，x 是 list 中的一个元素，这样就能够检索到该元素在 list 中的位置了。这才是真正的索引，注意那个英文单词 index。

依然是上一条官方解释：

>list.index(x)

>Return the index in the list of the first item whose value is x. It is an error if there is no such item.

是不是说的非常清楚明白了？

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：列表(1)](./111.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：列表(3)](./113.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。
