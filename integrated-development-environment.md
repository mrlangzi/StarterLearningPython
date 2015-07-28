# 集成开发环境(IDE)

当安装好 Python 之后，其实就已经可以进行开发了。按照惯例，第一行代码总是：Hello World

## 值得纪念的时刻：Hello world

不管你使用的是什么操作系统，总之肯定能够找到一个地方，运行 Python，进入到交互模式。

像下面一样：

    Python 2.7.6 (default, Nov 13 2013, 19:24:16) 
    [GCC 4.6.3] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

在`>>>`后面输入`print "Hello, World"`，并按回车。这就是见证奇迹的时刻。

    >>> print "Hello, World"
    Hello, World

如果你从来不懂编程，从这一刻起，就跨入了程序员行列;如果已经是程序员，那么就温习一下当初的惊喜吧！

`Hello, World`是你用代码向这个世界打招呼了。

每个程序员，都曾经经历过这个伟大时刻，不经历这个伟大时刻的程序员不是伟大的程序员。为了纪念这个伟大时刻，理解其伟大之所在，下面执行分解动作：

>说明：在下面的分解动作中，用到了一个符号：#，就是键盘上数字 ３ 上面的那个井号。这个符号，在 Python 编程中，表示注释。所谓注释，就是在计算机不执行那句话，只是为了说明某行语句表达什么意思，是给计算机前面的人看的。特别提醒，在编程实践中，注释是必须的。请牢记：程序在大多数情况下是给人看的，只是偶尔让计算机执行一下。

    # 看到“>>>”符号，表示 Python 做好了准备，等待你向她发出指令，让她做什么事情
    
    >>>

    # print，意思是打印。在这里也是这个意思，是要求 Python 打印什么东西
    
    >>> print

    #"Hello,World"是打印的内容，注意，变量的双引号，都是英文状态下的。引号不是打印内容，它相当于一个包裹，把打印的内容包起来，统一交给 Python。
    
    >>> print "Hello, World"  
    
    # 上面命令执行的结果。Python 接收到你要求她所做的事情：打印 Hello,World，于是她就老老实实地执行这个命令，丝毫不走样。
    
    Hello, World
    
在 Python 中，如果进入了上面的样式，我们称之为“交互模式”。这是非常有用而且简单的模式，她是我们进行各种学习和有关探索的好方式，随着学习的深入，你将更加觉得她魅力四射。

>笑一笑：有一个程序员，自己感觉书法太烂了，于是立志继承光荣文化传统，购买了笔墨纸砚。在某天，开始练字。将纸铺好，拿起笔蘸足墨水，挥毫在纸上写下了两个大字：Hello World

虽然进入了程序员序列，但是，如果程序员用的这个工具，也仅仅是打印 Hello,World，怎能用“伟大”来形容呢？

况且，这个工具也太简陋了？你看美工妹妹用的 Photoshop，行政妹妹用的 word，出纳妹妹用的 Excel，就连坐在老板桌后面的那个家伙还用一个 PPT 播放自己都不相信的新理念呢，难道我们伟大的程序员，就用这么简陋的工具写出旷世代码吗？

当然不是。软件是谁开发的？程序员。程序员肯定会先为自己打造好用的工具，这也叫做“近水楼台先得月”。

IDE 就是程序员的工具。

## 集成开发环境

IDE 的全称是：Integrated Development Environment，简称 IDE，也称为 Integration Design Environment、Integration Debugging Environment，翻译成中文叫做“集成开发环境”，在台湾那边叫做“整合開發環境”。它是一种辅助程序员开发用的应用软件。

[维基百科](http://zh.wikipedia.org/zh/%E9%9B%86%E6%88%90%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)这样对 IDE 定义：

>>IDE 通常包括程式语言编辑器、自动建立工具、通常还包括除错器。有些 IDE 包含编译器／直译器，如微软的 Microsoft Visual Studio，有些则不包含，如 Eclipse、SharpDevelop 等，这些 IDE 是通过调用第三方编译器来实现代码的编译工作的。有时 IDE 还会包含版本控制系统和一些可以设计圆形用戶界面的工具。许多支援物件导向的现代化 IDE 还包括了类別浏览器、物件检视器、物件结构图。虽然目前有一些IDE支援多种程式语言（例如 Eclipse、NetBeans、Microsoft Visual Studio），但是一般而言，IDE 主要还是针对特定的程式语言而量身打造（例如 Visual Basic）。

看不懂，没关系，看图，认识一下，混个脸熟就好了。所谓有图有真相。

![](./1images/10101.png)

上面的图显示的是微软的提供的名字叫做 Microsoft Visual Studio 的 IDE。用 C# 进行编程的程序员都用它。

![](./1images/10102.png)

上图是在苹果电脑中出现的名叫 XCode 的 IDE。

要想了解更多 IDE 的信息，推荐阅读维基百科中的词条

- 英文词条：[Integrated development environment](http://en.wikipedia.org/wiki/Integrated_development_environment)
- 中文词条：[集成开发环境](http://zh.wikipedia.org/zh/%E9%9B%86%E6%88%90%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)

## Python 的 IDE

google 一下：Python IDE，会发现，能够进行 Python 编程的 IDE 还真的不少。东西一多，就开始无所适从了。所有，有不少人都问用哪个 IDE 好。可以看看[这个提问，还列出了众多 IDE 的比较](http://stackoverflow.com/questions/81584/what-ide-to-use-for-Python)。

>顺便向列位看客推荐一个非常好的开发相关网站：[stackoverflow.com](http://stackoverflow.com/)

>在这里可以提问，可以查看答案。一般如果有问题，先在这里查找，多能找到非常满意的结果，至少有很大启发。

>在某国有时候有些地方可能不能访问，需要科学上网。好东西，一定不会让你轻易得到，也不会让任何人都得到。

那么做为零基础的学习者，用什么好呢？

既然是零基础，就别瞎折腾了，就用 Python 自带的 IDLE。原因就是：简单。

Windows 的朋友操作：“开始”菜单->“所有程序”->“Python 2.x”->“IDLE（Python GUI）”来启动 IDLE。启动之后，大概看到这样一个图

![](./1images/10103.png)

注意：看官所看到的界面中显示版本跟这个图不同，因为安装的版本区别。大致模样差不多。

其它操作系统的用户，也都能在找到 idle 这个程序，启动之后，跟上面一样的图。

后面我们所有的编程，就在这里完成了。这就是伟大程序员用的第一个 IDE。

除了这个自带的 IDE，还有很多其它的 IDE，列出来，供喜欢折腾的朋友参考

- PythonWin: 是 Python Win32 Extensions(半官方性质的 Python for win32 增强包)的一部分，也包含在 ActivePython 的 windows 发行版中。如其名字所言，只针对 win32 平台。
- MacPython IDE: MacPythonIDE 是 Python 的 Mac OS 发行版内置的 IDE，可以看作是 PythonWin 的 Mac 对应版本，由 Guido 的哥哥 Just van Rossum 编写。(哥俩都很牛)
- Emacs 和 Vim: Emacs 和 Vim 号称是这个星球上最强大(以及第二强大)的文本编辑器，对于许多程序员来说是万能 IDE 的不二(三?)选择。
- Eclipse + PyDev: Eclipse 是新一代的优秀泛用型 IDE，虽然是基于 Java 技术开发的，但出色的架构使其具有不逊于 Emacs 和 Vim 的可扩展性，现在已经成为了许多程序员最爱的瑞士军刀。

简单列几个，供参考，要找别的 IDE，网上搜一下，五花八门，不少呢。

磨刀不误砍柴工。IDE 已经有了，伟大程序员就要开始从事伟大的编程工作了。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：安装 Python 的开发环境](./03.md)|[&nbsp;&nbsp;&nbsp;下节](./102.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。
