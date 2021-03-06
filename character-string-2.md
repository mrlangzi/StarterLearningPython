# 字符串(2)

## raw_input 和 print

自从本课程开始以来，我们还没有感受到 computer 姑娘的智能。最简单的智能应该体现在哪里呢？想想小孩子刚刚回说话的时候情景吧。

>小孩学说话，是一个模仿的过程，孩子周围的人怎么说，她（他）往往就是重复。看官可以忘记自己当初是怎么学说话了吧？就找个小孩子观察一下吧。最好是自己的孩子。如果没有，就要抓紧了。

通过 Python 能不能实现这个简单的功能呢？当然能，要不然 Python 如何横行天下呀。

不过在写这个功能前，要了解两个函数：raw_input 和 print

>这两个都是 Python 的内建函数（built-in function）。关于 Python 的内建函数，下面这个表格都列出来了。所谓内建函数，就是能够在 Python 中直接调用，不需要做其它的操作。

Built-in Functions

|abs() |	divmod() |	input()| 	open()| 	staticmethod()|
|:-----|:------------|:--------|:---------|:------------------|
|all() |	enumerate() |	int() |	ord() |	str()|
|any() |	eval() |	isinstance()| 	pow()| 	sum()|
|basestring() |	execfile() |	issubclass() |	print() |	super()|
|bin() |	file() |	iter()| 	property()| 	tuple()|
|bool() |	filter() |	len() |	range() |	type()|
|bytearray() |	float()| 	list() |	raw_input()| 	unichr()|
|callable() |	format() |	locals() |	reduce() |	unicode()|
|chr() |	frozenset() |	long() |	reload() |	vars()|
|classmethod()| 	getattr()| 	map() |	repr() |	xrange()|
|cmp() |	globals()| 	max()| 	reversed()| 	zip()|
|compile() 	|hasattr() |	memoryview()| 	round() |	__import__()|
|complex() 	|hash() |	min()| 	set() |	apply()|
|delattr() 	|help()| 	next()| 	setattr()| 	buffer()|
|dict() |	hex() 	|object() 	|slice() |	coerce()|
|dir() |	id() 	|oct() 	|sorted() 	|intern()|

这些内建函数，怎么才能知道哪个函数怎么用，是干什么用的呢？

不知道你是否还记得我在前面使用过的方法，这里再进行演示，这种方法是学习 Python 的法宝。

    >>> help(raw_input)
	
然后就出现：

	Help on built-in function raw_input in module __builtin__:

    raw_input(...)
        raw_input([prompt]) -> string
    
        Read a string from standard input.  The trailing newline is stripped.
        If the user hits EOF (Unix: Ctl-D, Windows: Ctl-Z+Return), raise EOFError.
        On Unix, GNU readline is used if enabled.  The prompt string, if given,
        is printed without a trailing newline before reading.

从中是不是已经清晰地看到了 `raw_input()`的使用方法了。

还有第二种方法，那就是到 Python 的官方网站，查看内建函数的说明。<https://docs.Python.org/2/library/functions.html>

其实，我上面那个表格，就是在这个网页中抄过来的。

例如，对 `print()`说明如下：

     print(*objects, sep=' ', end='\n', file=sys.stdout)

        Print objects to the stream file, separated by sep and followed by end. sep, end and file, if present, must be given as keyword arguments.

        All non-keyword arguments are converted to strings like str() does and written to the stream, separated by sep and followed by end. Both sep and end must be strings; they can also be None, which means to use the default values. If no objects are given, print() will just write end.

        The file argument must be an object with a write(string) method; if it is not present or None, sys.stdout will be used. Output buffering is determined by file. Use file.flush() to ensure, for instance, immediate appearance on a screen.
		
分别在交互模式下，将这个两个函数操练一下。

    >>> raw_input("input your name:")
    input your name:python
    'python'
	
输入名字之后，就返回了输入的内容。用一个变量可以获得这个返回值。

    >>> name = raw_input("input your name:")
    input your name:python
    >>> name
    'python'
    >>> type(name)
    <type 'str'>
	
而且，返回的结果是 str 类型。如果输入的是数字呢？

    >>> age = raw_input("How old are you?")
    How old are you?10
    >>> age
    '10'
    >>> type(age)
    <type 'str'>

返回的结果，仍然是 str 类型。

再试试 `print()`，看前面对它的说明，是比较复杂的。没关系，我们从简单的开始。在交互模式下操作：

    >>> print("hello, world")
    hello, world
    >>> a = "python"
    >>> b = "good"
    >>> print a
    python
    >>> print a,b
    python good

比较简单吧。当然，这是没有搞太复杂了。

特别要提醒的是，`print()`默认是以 `\n` 结尾的，所以，会看到每个输出语句之后，输出内容后面自动带上了 `\n`，于是就换行了。

有了以上两个准备，接下来就可以写一个能够“对话”的小程序了。

    #!/usr/bin/env python
    # coding=utf-8

    name = raw_input("What is your name?")
    age = raw_input("How old are you?")

    print "Your name is:", name
    print "You are " + age + " years old."

    after_ten = int(age) + 10
    print "You will be " + str(after_ten) + " years old after ten years."
	
对这段小程序中，有几点说明

前面演示了 `print()`的使用，除了打印一个字符串之外，还可以打印字符串拼接结果。

    print "You are " + age + " years old."
	
注意，那个变量 `age` 必须是字符串，如最后的那个语句中：

    print "You will be " + str(after_ten) + " years old after ten years."
	
这句话里面，有一个类型转化，将原本是整数型 `after_ten` 转化为了 str 类型。否则，就包括，不信，你可以试试。

同样注意，在 `after_ten = int(age) + 10` 中，因为通过 `raw_input` 得到的是 str 类型，当 age 和 10 求和的时候，需要先用 `int()`函数进行类型转化，才能和后面的整数 10 相加。

这个小程序，是有点综合的，基本上把已经学到的东西综合运用了一次。请看官调试一下，如果没有通过，仔细看报错信息，你能够从中获得修改方向的信息。

## 原始字符串

所谓原始字符串，就是指字符串里面的每个字符都是原始含义，比如反斜杠，不会被看做转义符。如果在一般字符串中，比如

    >>> print "I like \npython"
    I like 
    python

这里的反斜杠就不是“反斜杠”的原始符号含义，而是和后面的 n 一起表示换行（转义了）。当然，这似乎没有什么太大影响，但有的时候，可能会出现问题，比如打印 DOS 路径（DOS，有没有搞错，现在还有人用吗？）

    >>> dos = "c:\news"
    >>> dos
    'c:\news'        # 这里貌似没有什么问题
    >>> print dos    # 当用 print 来打印这个字符串的时候，就出问题了。
    c:
    ews

如何避免？用前面讲过的转义符可以解决：

    >>> dos = "c:\\news"
    >>> print dos
    c:\news

此外，还有一种方法，如：

    >>> dos = r"c:\news"
    >>> print dos
    c:\news
    >>> print r"c:\news\python"
    c:\news\python

状如 `r"c:\news"`，由 r 开头引起的字符串，就是原始字符串，在里面放任何字符都表示该字符的原始含义。

这种方法在做网站设置网站目录结构的时候非常有用。使用了原始字符串，就不需要转义了。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：字符串(1)](./106.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：字符串（3）](./108.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。

