# 文件(1)

文件，是 computer 姑娘中非常重要的东西，在 Python 里，它也是一种类型的对象，类似前面已经学习过的其它数据类型，包括文本的、图片的、音频的、视频的等等，还有不少没见过的扩展名的。事实上，在 linux 操作系统中，所有的东西都被保存到文件中。

先在交互模式下查看一下文件都有哪些属性：

    >>> dir(file)
    ['__class__', '__delattr__', '__doc__', '__enter__', '__exit__', '__format__', '__getattribute__', '__hash__', '__init__', '__iter__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'close', 'closed', 'encoding', 'errors', 'fileno', 'flush', 'isatty', 'mode', 'name', 'newlines', 'next', 'read', 'readinto', 'readline', 'readlines', 'seek', 'softspace', 'tell', 'truncate', 'write', 'writelines', 'xreadlines']

然后对部分属性进行详细说明，就是看官学习了。

特别注意观察，在上面有`__iter__`这个东西。曾经在讲述列表的时候，是不是也出现这个东西了呢？是的。它意味着这种类型的数据是可迭代的(iterable)。在下面的讲解中，你就会看到了，能够用 for 来读取其中的内容。

## 打开文件

在某个文件夹下面建立了一个文件，名曰：130.txt，并且在里面输入了如下内容：

    learn python
    http://qiwsir.github.io
    qiwsir@gmail.com

此文件一共三行。

下图显示了这个文件的存储位置：

![](./1images/12601.png)

在上面截图中，我在当前位置输入了 Python（我已经设置了环境变量，如果你没有，需要写全启动 Python 命令路径），进入到交互模式。在这个交互模式下，这样操作：

    >>> f = open("130.txt")     #打开已经存在的文件
    >>> for line in f:
    ...     print line
    ... 
    learn Python

    http://qiwsir.github.io

    qiwsir@gmail.com

提醒初学者注意，在那个文件夹输入了启动 Python 交互模式的命令，那么，如果按照上面的方法 `open("130.txt")`打开文件，就意味着这个文件 130.txt 是在当前文件夹内的。如果要打开其它文件夹内的文件，请用相对路径或者绝对路径来表示，从而让 python 能够找到那个文件。

将打开的文件，赋值给变量 f，这样也就是变量 f 跟对象文件 130.txt 用线连起来了（对象引用），本质上跟前面所讲述的其它类型数据进行赋值是一样的。

接下来，用 for 来读取文件中的内容，就如同读取一个前面已经学过的序列对象一样，如 list、str、tuple，把读到的文件中的每行，赋值给变量 line。也可以理解为，for 循环是一行一行地读取文件内容。每次扫描一行，遇到行结束符号 \n 表示本行结束，然后是下一行。

从打印的结果看出，每一行跟前面看到的文件内容中的每一行是一样的。只是行与行之间多了一空行，前面显示文章内容的时候，没有这个空行。或许这无关紧要，但是，还要深究一下，才能豁然。

在原文中，每行结束有本行结束符号 \n，表示换行。在 for 语句汇总，print line 表示每次打印完 line 的对象之后，就换行，也就是打印完 line 的对象之后会增加一个 \n。这样看来，在每行末尾就有两个 \n，即：\n\n，于是在打印中就出现了一个空行。

    >>> f = open('130.txt')
    >>> for line in f:
    ...     print line,     #后面加一个逗号，就去掉了原来默认增加的 \n 了，看看，少了空行。
    ... 
    learn Python
    http://qiwsir.github.io
    qiwsir@gmail.com

在进行上述操作的时候，有没有遇到这样的情况呢？

    >>> f = open('130.txt')
    >>> for line in f:
    ...     print line,
    ... 
    learn Python
    http://qiwsir.github.io
    qiwsir@gmail.com

    >>> for line2 in f:     #在前面通过 for 循环读取了文件内容之后，再次读取，
    ...     print line2     #然后打印，结果就什么也显示，这是什么问题？
    ... 
    >>>

如果看官没有遇到上面问题，可以试试。这不是什么错误，是因为前一次已经读取了文件内容，并且到了文件的末尾了。再重复操作，就是从末尾开始继续读了。当然显示不了什么东西，但是 Python 并不认为这是错误，因为后面就会讲到，或许在这次读取之前，已经又向文件中追加内容了。那么，如果要再次读取怎么办？就从新来一边好了。这就好比有一个指针在指着文件中的每一行，每读完一行，指针向移动一行。直到指针指向了文件的最末尾。当然，也有办法把指针移动到任何位置。

特别提醒看官，因为当前的交互模式是在该文件所在目录启动的，所以，就相当于这个实验室和文件 130.txt 是同一个目录，这时候我们打开文件 130.txt，就认为是在本目录中打开，如果文件不是在本目录中，需要写清楚路径。

比如：在上一级目录中（~/Documents/ITArticles/BasicPython），假如我进入到那个目录中，运行交互模式，然后试图打开 130.txt 文件。

![](./1images/12602.png)

    >>> f = open("130.txt")
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    IOError: [Errno 2] No such file or directory: '130.txt'
    
    >>> f = open("./codes/130.txt")     #必须得写上路径了（注意，windows的 路径是 \ 隔开，需要转义。对转义符，看官看以前讲座）
    >>> for line in f:
    ...     print line
    ... 
    learn Python

    http://qiwsir.github.io

    qiwsir@gmail.com

    >>> 

## 创建文件

上面的实验中，打开的是一个已经存在的文件。如何创建文件呢？

    >>> nf = open("131.txt","w")
    >>> nf.write("This is a file")

就这样创建了一个文件？并写入了文件内容呢？看看再说：

![](./1images/12603.png)

真的就这样创建了新文件，并且里面有那句话呢。

看官注意了没有，这次我们同样是用 open() 这个函数，但是多了个"w"，这是在告诉 Python 用什么样的模式打开文件。也就是说，用 open() 打开文件，可以有不同的模式打开。看下表：

| 模式 | 描述 |
|------|------|
| r | 以读方式打开文件，可读取文件信息。|
| w | 以写方式打开文件，可向文件写入信息。如文件存在，则清空该文件，再写入新内容 |
| a | 以追加模式打开文件（即一打开文件，文件指针自动移到文件末尾），如果文件不存在则创建 |
| r+ | 以读写方式打开文件，可对文件进行读和写操作。 |
| w+ | 消除文件内容，然后以读写方式打开文件。 |
| a+ | 以读写方式打开文件，并把文件指针移到文件尾。 |
| b | 以二进制模式打开文件，而不是以文本模式。该模式只对 Windows 或 Dos 有效，类 Unix 的文件是用二进制模式进行操作的。 |

从表中不难看出，不同模式下打开文件，可以进行相关的读写。那么，如果什么模式都不写，像前面那样呢？那样就是默认为 r 模式，只读的方式打开文件。

    >>> f = open("130.txt")
    >>> f
    <open file '130.txt', mode 'r' at 0xb7530230>
    >>> f = open("130.txt","r")
    >>> f
    <open file '130.txt', mode 'r' at 0xb750a700>
 
可以用这种方式查看当前打开的文件是采用什么模式的，上面显示，两种模式是一样的效果，如果不写那个"r"，就默认为是只读模式了。下面逐个对各种模式进行解释

**"w":以写方式打开文件，可向文件写入信息。如文件存在，则清空该文件，再写入新内容**

131.txt 这个文件是存在的，前面建立的，并且在里面写了一句话：This is a file

    >>> fp = open("131.txt")
    >>> for line in fp:         #原来这个文件里面的内容
    ...     print line
    ... 
    This is a file
    >>> fp = open("131.txt","w")    #这时候再看看这个文件，里面还有什么呢？是不是空了呢？
    >>> fp.write("My name is qiwsir.\nMy website is qiwsir.github.io")  #再查看内容
    >>> fp.close()

查看文件内容：

    $ cat 131.txt  #cat 是 linux 下显示文件内容的命令，这里就是要显示 131.txt 内容
    My name is qiwsir.
    My website is qiwsir.github.io

**"a":以追加模式打开文件（即一打开文件，文件指针自动移到文件末尾），如果文件不存在则创建**

    >>> fp = open("131.txt","a")
    >>> fp.write("\nAha,I like program\n")    #向文件中追加
    >>> fp.close()                            #这是关闭文件，一定要养成一个习惯，写完内容之后就关闭

查看文件内容：

    $ cat 131.txt
    My name is qiwsir.
    My website is qiwsir.github.io
    Aha,I like program

其它项目就不一一讲述了。看官可以自己实验。

## 使用 with

在对文件进行写入操作之后，一定要牢记一个事情：`file.close()`，这个操作千万不要忘记，忘记了怎么办，那就补上吧，也没有什么天塌地陷的后果。

有另外一种方法，能够不用这么让人揪心，实现安全地关闭文件。

    >>> with open("130.txt","a") as f:
    ...     f.write("\nThis is about 'with...as...'")
    ... 
    >>> with open("130.txt","r") as f:
    ...     print f.read()
    ... 
    learn python
    http://qiwsir.github.io
    qiwsir@gmail.com
    hello

    This is about 'with...as...'
    >>> 

这里就不用 close()了。而且这种方法更有 Python 味道，或者说是更符合 Pythonic 的一个要求。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：语句(5)](./125.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：文件(2)](./127.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。