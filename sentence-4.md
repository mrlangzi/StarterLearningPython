# 语句(4)

for 循环在 Python 中应用广泛，所以，要用更多的篇幅来介绍。

## 并行迭代

关于迭代，在[《列表(2)》](./112.md)中曾经提到过“可迭代的(iterable)”这个词，并给予了适当解释，这里再次提到“迭代”，说明它在 Python 中占有重要的位置。

迭代，在 Python 中表现就是用 for 循环，从序列对象中获得一定数量的元素。

在前面一节中，用 for 循环来获得列表、字符串、元组，乃至于字典的键值对，都是迭代。

现实中迭代不都是那么简单的，比如这个问题：

**问题：**有两个列表，分别是：a = [1,2,3,4,5], b = [9,8,7,6,5]，要计算这两个列表中对应元素的和。

**解析：**

太简单了，一看就知道结果了。

很好，这是你的方法，如果是 computer 姑娘来做，应该怎么做呢？

观察发现两个列表的长度一样，都是 5。那么对应元素求和，就是相同的索引值对应的元素求和，即 a[i]+b[i],(i=0,1,2,3,4)，这样一个一个地就把相应元素和求出来了。当然，要用 for 来做这个事情了。

    >>> a = [1,2,3,4,5]
    >>> b = [9,8,7,6,5]
    >>> c = []
    >>> for i in range(len(a)):
    ...     c.append(a[i]+b[i])
    ... 
    >>> c
    [10, 10, 10, 10, 10]

看来 for 的表现还不错。不过，这种方法虽然解决问题了，但 Python 总不会局限于一个解决之道。于是又有一个内建函数 `zip()`，可以让同样的问题有不一样的解决途径。

zip 是什么东西？在交互模式下用 help(zip),得到官方文档是：

>zip(...)
>zip(seq1 [, seq2 [...]]) -> [(seq1[0], seq2[0] ...), (...)]

>Return a list of tuples, where each tuple contains the i-th element from each of the argument sequences.  The returned list is truncated in length to the length of the shortest argument sequence.

seq1, seq2 分别代表了序列类型的数据。通过实验来理解上面的文档：

    >>> a = "qiwsir"
    >>> b = "github"
    >>> zip(a,b)
    [('q', 'g'), ('i', 'i'), ('w', 't'), ('s', 'h'), ('i', 'u'), ('r', 'b')]
    
如果序列长度不同，那么就以"the length of the shortest argument sequence"为准。

    >>> c = [1,2,3]
    >>> d = [9,8,7,6]
    >>> zip(c,d)
    [(1, 9), (2, 8), (3, 7)]

    >>> m = {"name","lang"}  
    >>> n = {"qiwsir","python"}
    >>> zip(m,n)
    [('lang', 'python'), ('name', 'qiwsir')]

m，n 是字典吗？当然不是。下面的才是字典呢。
    
    >>> s = {"name":"qiwsir"}
    >>> t = {"lang":"python"}
    >>> zip(s,t)
    [('name', 'lang')]

zip 是一个内置函数，它的参数必须是某种序列数据类型，如果是字典，那么键视为序列。然后将序列对应的元素依次组成元组，做为一个 list 的元素。

下面是比较特殊的情况，参数是一个序列数据的时候，生成的结果样子：

    >>> a  
    'qiwsir'
    >>> c  
    [1, 2, 3]
    >>> zip(c)
    [(1,), (2,), (3,)]
    >>> zip(a)
    [('q',), ('i',), ('w',), ('s',), ('i',), ('r',)]
    
很好的 `zip()`！那么就用它来解决前面那个两个列表中值对应相加吧。

    >>> d = []
    >>> for x,y in zip(a,b):
    ...     d.append(x+y)
    ... 
    >>> d
    [10, 10, 10, 10, 10]

多么优雅的解决！

比较这个问题的两种解法，似乎第一种解法适用面较窄，比如，如果已知给定的两个列表长度不同，第一种解法就出问题了。而第二种解法还可以继续适用。的确如此，不过，第一种解法也不是不能修订的。

    >>> a = [1,2,3,4,5]
    >>> b = ["python","www.itdiffer.com","qiwsir"]
    
如果已知是这样两个列表，要讲对应的元素“加起来”。

    >>> length = len(a) if len(a)<len(b) else len(b)
    >>> length
    3

首先用这种方法获得两个列表中最短的那个列表的长度。看那句三元操作，这是非常 Pythonic 的写法啦。写出这句，就可以冒充高手了。哈哈。

    >>> for i in range(length):
    ...     c.append(str(a[i]) + ":" + b[i])
    ... 
    >>> c
    ['1:python', '2:www.itdiffer.com', '3:qiwsir']

我还是用第一个思路做的，经过这么修正一下，也还能用。要注意一个细节，在“加”的时候，不能直接用 `a[i]`，因为它引用的对象是一个 int 类型，不能跟后面的 str 类型相加，必须转化一下。

当然，`zip()`也是能解决这个问题的。

    >>> d = []
    >>> for x,y in zip(a,b):
    ...     d.append(x + y)
    ... 
    Traceback (most recent call last):
      File "<stdin>", line 2, in <module>
    TypeError: unsupported operand type(s) for +: 'int' and 'str'

报错！看错误信息，我刚刚提醒的那个问题就冒出来了。所以，应该这么做：

    >>> for x,y in zip(a,b):
    ...     d.append(str(x) + ":" + y)
    ... 
    >>> d
    ['1:python', '2:www.itdiffer.com', '3:qiwsir']

这才得到了正确结果。

切记：**computer 是一个姑娘，她非常秀气，需要敲代码的小伙子们耐心地、细心地跟她相处。**

以上两种写法那个更好呢？前者？后者？哈哈。我看差不多了。

    >>> result
    [(2, 11), (4, 13), (6, 15), (8, 17)]
    >>> zip(*result)
    [(2, 4, 6, 8), (11, 13, 15, 17)]
    
`zip()`还能这么干，是不是有点意思？

下面延伸一个问题：

**问题**：有一个 dictionary，myinfor = {"name":"qiwsir","site":"qiwsir.github.io","lang":"python"},将这个字典变换成：infor = {"qiwsir":"name","qiwsir.github.io":"site","python":"lang"}

**解析：**

解法有几个，如果用 for 循环，可以这样做（当然，看官如果有方法，欢迎贴出来）。

    >>> infor = {}
    >>> for k,v in myinfor.items():
    ...     infor[v]=k
    ... 
    >>> infor
    {'python': 'lang', 'qiwsir.github.io': 'site', 'qiwsir': 'name'}

下面用 zip() 来试试：

    >>> dict(zip(myinfor.values(),myinfor.keys()))
    {'python': 'lang', 'qiwsir.github.io': 'site', 'qiwsir': 'name'}

呜呼，这是什么情况？原来这个 zip() 还能这样用。是的，本质上是这么回事情。如果将上面这一行分解开来，看官就明白其中的奥妙了。

    >>> myinfor.values()    #得到两个 list
    ['Python', 'qiwsir', 'qiwsir.github.io']
    >>> myinfor.keys()
    ['lang', 'name', 'site']
    >>> temp = zip(myinfor.values(),myinfor.keys())     #压缩成一个 list，每个元素是一个 tuple
    >>> temp
    [('python', 'lang'), ('qiwsir', 'name'), ('qiwsir.github.io', 'site')]

    >>> dict(temp)                          #这是函数 dict() 的功能，将上述列表转化为 dictionary
    {'Python': 'lang', 'qiwsir.github.io': 'site', 'qiwsir': 'name'}

至此，是不是明白 zip()和循环的关系了呢？有了它可以让某些循环简化。

## enumerate

这是一个有意思的内置函数，本来我们可以通过 `for i in range(len(list))`的方式得到一个 list 的每个元素索引，然后在用 list[i]的方式得到该元素。如果要同时得到元素索引和元素怎么办？就是这样了:

    >>> for i in range(len(week)):
    ...     print week[i]+' is '+str(i)     #注意，i 是 int 类型，如果和前面的用 + 连接，必须是 str 类型
    ... 
    monday is 0
    sunday is 1
    friday is 2

Python 中提供了一个内置函数 enumerate，能够实现类似的功能

    >>> for (i,day) in enumerate(week):
    ...     print day+' is '+str(i)
    ... 
    monday is 0
    sunday is 1
    friday is 2

官方文档是这么说的：

>Return an enumerate object. sequence must be a sequence, an iterator, or some other object which supports iteration. The next() method of the iterator returned by enumerate() returns a tuple containing a count (from start which defaults to 0) and the values obtained from iterating over sequence:

顺便抄录几个例子，供看官欣赏，最好实验一下。

    >>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
    >>> list(enumerate(seasons))
    [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
    >>> list(enumerate(seasons, start=1))
    [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

对于这样一个列表：

    >>> mylist = ["qiwsir",703,"python"]
    >>> enumerate(mylist)
    <enumerate object at 0xb74a63c4>    
    
出现这个结果，用 list 就能实现转换，显示内容.意味着可迭代。

    >>> list(enumerate(mylist))
    [(0, 'qiwsir'), (1, 703), (2, 'python')]
    
再设计一个小问题，练习一下这个函数。

**问题：**将字符串中的某些字符替换为其它的字符串。原始字符串"Do you love Canglaoshi? Canglaoshi is a good teacher."，请将"Canglaoshi"替换为"PHP".

**解析：**

    >>> raw = "Do you love Canglaoshi? Canglaoshi is a good teacher."

这是所要求的那个字符串，当时，不能直接对这个字符串使用 `enumerate()`，因为它会变成这样：

    >>> list(enumerate(raw))
    [(0, 'D'), (1, 'o'), (2, ' '), (3, 'y'), (4, 'o'), (5, 'u'), (6, ' '), (7, 'l'), (8, 'o'), (9, 'v'), (10, 'e'), (11, ' '), (12, 'C'), (13, 'a'), (14, 'n'), (15, 'g'), (16, 'l'), (17, 'a'), (18, 'o'), (19, 's'), (20, 'h'), (21, 'i'), (22, '?'), (23, ' '), (24, 'C'), (25, 'a'), (26, 'n'), (27, 'g'), (28, 'l'), (29, 'a'), (30, 'o'), (31, 's'), (32, 'h'), (33, 'i'), (34, ' '), (35, 'i'), (36, 's'), (37, ' '), (38, 'a'), (39, ' '), (40, 'g'), (41, 'o'), (42, 'o'), (43, 'd'), (44, ' '), (45, 't'), (46, 'e'), (47, 'a'), (48, 'c'), (49, 'h'), (50, 'e'), (51, 'r'), (52, '.')]

这不是所需要的。所以，先把 raw 转化为列表：

    >>> raw_lst = raw.split(" ")

然后用 `enumerate()`

    >>> for i, string in enumerate(raw_lst):
    ...     if string == "Canglaoshi":
    ...         raw_lst[i] = "PHP"
    ... 

没有什么异常现象，查看一下那个 raw_lst 列表，看看是不是把"Canglaoshi"替换为"PHP"了。

    >>> raw_lst
    ['Do', 'you', 'love', 'Canglaoshi?', 'PHP', 'is', 'a', 'good', 'teacher.']

只替换了一个，还有一个没有替换。为什么？仔细观察发现，没有替换的那个是'Canglaoshi?',跟条件判断中的"Canglaoshi"不一样。

修改一下，把条件放宽：

    >>> for i, string in enumerate(raw_lst):
    ...     if "Canglaoshi" in string:
    ...         raw_lst[i] = "PHP"
    ... 
    >>> raw_lst
    ['Do', 'you', 'love', 'PHP', 'PHP', 'is', 'a', 'good', 'teacher.']

好的。然后呢？再转化为字符串？留给读者试试。

## list 解析

先看下面的例子，这个例子是想得到 1 到 9 的每个整数的平方，并且将结果放在 list 中打印出来

    >>> power2 = []
    >>> for i in range(1,10):
    ...     power2.append(i*i)
    ... 
    >>> power2
    [1, 4, 9, 16, 25, 36, 49, 64, 81]

Python 有一个非常有意思的功能，就是 list 解析，就是这样的：

    >>> squares = [x**2 for x in range(1,10)]
    >>> squares
    [1, 4, 9, 16, 25, 36, 49, 64, 81]

看到这个结果，看官还不惊叹吗？这就是 Python，追求简洁优雅的 Python！

其官方文档中有这样一段描述，道出了 list 解析的真谛：

>List comprehensions provide a concise way to create lists. Common applications are to make new lists where each element is the result of some operations applied to each member of another sequence or iterable, or to create a subsequence of those elements that satisfy a certain condition.

这就是 Python 有意思的地方，也是计算机高级语言编程有意思的地方，你只要动脑筋，总能找到惊喜的东西。

其实，不仅仅对数字组成的 list，所有的都可以如此操作。请在平复了激动的心之后，默默地看下面的代码，感悟一下 list 解析的魅力。

    >>> mybag = [' glass',' apple','green leaf ']   #有的前面有空格，有的后面有空格
    >>> [one.strip() for one in mybag]              #去掉元素前后的空格
    ['glass', 'apple', 'green leaf']

上面的问题，都能用 list 解析来重写。读者不妨试试。

在很多情况下，list 解析的执行效率高，代码简洁明了。是实际写程序中经常被用到的。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：语句(3)](./123.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：语句(5)](./125.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。