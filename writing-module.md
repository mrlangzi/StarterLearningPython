# 编写模块

在本章之前，Python 还没有显示出太突出的优势。本章开始，读者就会越来越感觉到 Python 的强大了。这种强大体现在“模块自信”上，因为 Python 不仅有很强大的自有模块（称之为标准库），还有海量的第三方模块，任何人还都能自己开发模块，正是有了这么强大的“模块自信”，才体现了 Python 的优势所在。并且这种方式也正在不断被更多其它语言所借鉴。

“模块自信”的本质是：开放。

Python 不是一个封闭的体系，是一个开放系统。开放系统的最大好处就是避免了“熵增”。

>熵的概念是由德国物理学家克劳修斯于 1865 年（这一年李鸿章建立了江南机械制造总局，美国废除奴隶制，林肯总统遇刺身亡，美国南北战争结束。）所提出。是一种测量在动力学方面不能做功的能量总数，也就是当总体的熵增加，其做功能力也下降，熵的量度正是能量退化的指标。 

>熵亦被用于计算一个系统中的失序现象，也就是计算该系统混乱的程度。 

>根据熵的统计学定义， 热力学第二定律说明一个孤立系统的倾向于增加混乱程度。换句话说就是对于封闭系统而言，会越来越趋向于无序化。反过来，开放系统则能避免无序化。

## 回忆过去

在本教程的[《语句(1)》](./121.md)中，曾经介绍了 import 语句，有这样一个例子：

    >>> import math
    >>> math.pow(3,2)
    9.0
    
这里的 math 就是一个模块，用 import 引入这个模块，然后可以使用模块里面的函数，比如这个 pow() 函数。显然，这里我们是不需要自己动手写具体函数的，我们的任务就是拿过来使用。这就是模块的好处：拿过来就用，不用自己重写。

## 模块是程序

这个标题，一语道破了模块的本质，它就是一个扩展名为 `.py` 的 Python 程序。我们能够在应该使用它的时候将它引用过来，节省精力，不需要重写雷同的代码。

但是，如果我自己写一个 `.py` 文件，是不是就能作为模块 import 过来呢？还不那么简单。必须得让 Python 解释器能够找到你写的模块。比如：在某个目录中，我写了这样一个文件：
 
    #!/usr/bin/env Python
    # coding=utf-8

    lang = "python"

并把它命名为 pm.py，那么这个文件就可以作为一个模块被引入。不过由于这个模块是我自己写的，Python 解释器并不知道，我得先告诉它我写了这样一个文件。

    >>> import sys
    >>> sys.path.append("~/Documents/VBS/StartLearningPython/2code/pm.py")

用这种方式就是告诉 Python 解释器，我写的那个文件在哪里。在这个告诉方法中，也用了一个模块 `import sys`，不过由于 sys 模块是 Python 被安装的时候就有的，所以不用特别告诉，Python 解释器就知道它在哪里了。

上面那个一长串的地址，是 ubuntu 系统的地址格式，如果读者使用的 windows 系统，请写你所保存的文件路径。

    >>> import pm
    >>> pm.lang
    'python'

本来在 pm.py 文件中，有一个变量 `lang = "Python"`，这次它作为模块引入（注意作为模块引入的时候，不带扩展名），就可以通过模块名字来访问变量 `pm.py`，当然，如果不存在的属性这么去访问，肯定是要报错的。

    >>> pm.xx
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'module' object has no attribute 'xx'

请读者回到 pm.py 文件的存储目录，是不是多了一个扩展名是 .pyc 的文件？如果不是，你那个可能是外星人用的 Python。

>解释器，英文是：interpreter，港台翻译为：直译器。在 Python 中，它的作用就是将 .py 的文件转化为 .pyc 文件，而 .pyc 文件是由字节码(bytecode)构成的，然后计算机执行 .pyc 文件。关于这方面的详细解释，请参阅维基百科的词条：[直译器](http://zh.wikipedia.org/zh/%E7%9B%B4%E8%AD%AF%E5%99%A8)

不少人喜欢将这个世界简化简化再简化。比如人，就分为好人还坏人，比如编程语言就分为解释型和编译型，不但如此，还将两种类型的语言分别贴上运行效率高低的标签，解释型的运行速度就慢，编译型的就快。一般人都把 Python 看成解释型的，于是就得出它运行速度慢的结论。不少人都因此上当受骗了，认为 Python 不值得学，或者做不了什么“大事”。这就是将本来复杂的多样化的世界非得划分为“黑白”的结果。这种喜欢用“非此即彼”的思维方式考虑问题的现象可以说在现在很常见，比如一提到“日本人”，都该杀，这基本上是小孩子的思维方法，可惜在某个过度内大行其道。

世界是复杂的，“敌人的敌人就是朋友”是幼稚的，“一分为二”是机械的。

就如同刚才看到的那个 .pyc 文件一样，当 Python 解释器读取了 .py 文件，先将它变成由字节码组成的 .pyc 文件，然后这个 .pyc 文件交给一个叫做 Python 虚拟机的东西去运行（那些号称编译型的语言也是这个流程，不同的是它们先有一个明显的编译过程，编译好了之后再运行）。如果 .py 文件修改了，Python 解释器会重新编译，只是这个编译过程不是完全显示给你看的。

我这里说的比较笼统，要深入了解 Python 程序的执行过程，可以阅读这篇文章：[说说 Python 程序的执行过程](http://www.cnblogs.com/kym/archive/2012/05/14/2498728.html)

总之，有了 .pyc 文件后，每次运行，就不需要从新让解释器来编译 .py 文件了，除非 .py 文件修改了。这样，Python 运行的就是那个编译好了的 .pyc 文件。

是否还记得，我们在前面写有关程序，然后执行，常常要用到 `if __name__ == "__main__"`。那时我们写的 .py 文件是来执行的，这时我们同样写了 .py 文件，是作为模块引入的。这就得深入探究一下，同样是 .py 文件，它是怎么知道是被当做程序执行还是被当做模块引入？

为了便于比较，将 pm.py 文件进行改造，稍微复杂点。

    #!/usr/bin/env Python
    # coding=utf-8

    def lang():
        return "Python"

    if __name__ == "__main__":
        print lang()

如以前做的那样，可以用这样的方式：

    $ Python pm.py
    python

但是，如果将这个程序作为模块，导入，会是这样的：

    >>> import sys
    >>> sys.path.append("~/Documents/VBS/StarterLearningPython/2code/pm.py")
    >>> import pm
    >>> pm.lang()
    'python'

因为这时候 pm.py 中的函数 lang() 就是一个属性：

    >>> dir(pm)
    ['__builtins__', '__doc__', '__file__', '__name__', '__package__', 'lang']

同样一个 .py 文件，可以把它当做程序来执行，还可以将它作为模块引入。

    >>> __name__
    '__main__'
    >>> pm.__name__
    'pm'

如果要作为程序执行，则`__name__ == "__main__"`；如果作为模块引入，则 `pm.__name__ == "pm"`，即变量`__name__`的值是模块名称。

用这种方式就可以区分是执行程序还是作为模块引入了。 

在一般情况下，如果仅仅是用作模块引入，可以不写 `if __name__ == "__main__"`。

## 模块的位置

为了让我们自己写的模块能够被 Python 解释器知道，需要用 `sys.path.append("~/Documents/VBS/StarterLearningPython/2code/pm.py")`。其实，在 Python 中，所有模块都被加入到了 sys.path 里面了。用下面的方法可以看到模块所在位置：

    >>> import sys
    >>> import pprint
    >>> pprint.pprint(sys.path)
    ['',
     '/usr/local/lib/python2.7/dist-packages/autopep8-1.1-py2.7.egg',
     '/usr/local/lib/python2.7/dist-packages/pep8-1.5.7-py2.7.egg',
     '/usr/lib/python2.7',
     '/usr/lib/python2.7/plat-i386-linux-gnu',
     '/usr/lib/python2.7/lib-tk',
     '/usr/lib/python2.7/lib-old',
     '/usr/lib/python2.7/lib-dynload',
     '/usr/local/lib/python2.7/dist-packages',
     '/usr/lib/python2.7/dist-packages',
     '/usr/lib/python2.7/dist-packages/PILcompat',
     '/usr/lib/python2.7/dist-packages/gtk-2.0',
     '/usr/lib/python2.7/dist-packages/ubuntu-sso-client',
     '~/Documents/VBS/StarterLearningPython/2code/pm.py']

从中也发现了我们自己写的那个文件。凡在上面列表所包括位置内的 .py 文件都可以作为模块引入。不妨举个例子。把前面自己编写的 pm.py 文件修改为 pmlib.py，然后把它复制到`'/usr/lib/Python2.7/dist-packages` 中。（这是以 ubuntu 为例说明，如果是其它操作系统，读者用类似方法也能找到。）

    $ sudo cp pm.py /usr/lib/python2.7/dist-packages/pmlib.py
    [sudo] password for qw: 

    $ ls /usr/lib/python2.7/dist-packages/pm*
    /usr/lib/Python2.7/dist-packages/pmlib.py

文件放到了指定位置。看下面的：

    >>> import pmlib
    >>> pmlib.lang
    <function lang at 0xb744372c>
    >>> pmlib.lang()
    'python'

也就是，要将模块文件放到合适的位置——就是 sys.path 包括位置——就能够直接用 import 引入了。

## PYTHONPATH 环境变量

将模块文件放到指定位置是一种不错的方法。当程序员都喜欢自由，能不能放到别处呢？当然能，用 `sys.path.append()` 就是不管把文件放哪里，都可以把其位置告诉 Python 解释器。但是，这种方法不是很常用。因为它也有麻烦的地方，比如在交互模式下，如果关闭了，然后再开启，还得从新告知。

比较常用的告知方法是设置 PYTHONPATH 环境变量。

>环境变量，不同操作系统的设置方法略有差异。读者可以根据自己的操作系统，到网上搜索设置方法。

我以 ubuntu 为例，建立一个 Python 的目录，然后将我自己写的 .py 文件放到这里，并设置环境变量。

    :~$ mkdir Python
    :~$ cd python
    :~/Python$ cp ~/Documents/VBS/StarterLearningPython/2code/pm.py mypm.py
    :~/Python$ ls
    mypm.py

然后将这个目录 `~/Python`，也就是 `/home/qw/Python` 设置环境变量。

    vim /etc/profile

提醒要用 root 权限，在打开的文件最后增加 `export PATH = /home/qw/python:$PAT`，然后保存退出即可。

注意，我是在 `~/Python` 目录下输入 `Python`，进入到交互模式：

    :~$ cd Python
    :~/python$ Python
    
    >>> import mypm
    >>> mypm.lang()
    'Python'

如此，就完成了告知过程。

## `__init__.py` 方法

`__init__.py` 是一个空文件，将它放在某个目录中，就可以将该目录中的其它 .py 文件作为模块被引用。这个具体应用参见[用 tornado 做网站(2)](./304.md)

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：错误和异常(3)](./218.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：标准库(1)](./220.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。