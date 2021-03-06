# 多态和封装

前面讲过的“继承”，是类的一个重要特征，在编程中用途很多。这里要说两个在理解和实践上有争议的话题：多态和封装。所谓争议，多来自于对同一个现象不同角度的理解，特别是有不少经验丰富的程序员，还从其它语言的角度来诠释 Python 的多态等。

## 多态

在网上搜索一下，发现对 Python 的多态问题，的确是仁者见仁智者见智。

作为一个初学者，不一定要也没有必要、或者还没有能力参与这种讨论。但是，应该理解 Python 中关于多态的基本体现，也要对多态有一个基本的理解。

    >>> "This is a book".count("s")
    2
    >>> [1,2,4,3,5,3].count(3)
    2

上面的 `count()` 的作用是数一数某个元素在对象中出现的次数。从例子中可以看出，我们并没有限定 count 的参数。类似的例子还有：

    >>> f = lambda x,y:x+y

还记得这个 lambda 函数吗？如果忘记了，请复习[函数(4)](https://github.com/qiwsir/StarterLearningPython/blob/master/204.md)中对此的解释。
    
    >>> f(2,3)
    5
    >>> f("qiw","sir")
    'qiwsir'
    >>> f(["python","java"],["c++","lisp"])
    ['python', 'java', 'c++', 'lisp']

在那个 lambda 函数中，我们没有限制参数的类型，也一定不能限制，因为如果限制了，就不是 Pythonic 了。在使用的时候，可以给参数任意类型，都能到的不报错的结果。当然，这样做之所以合法，更多的是来自于 `+` 的功能强悍。

以上，就体现了“多态”。当然，也有人就此提出了反对意见，因为本质上是在参数传入值之前，Python 并没有确定参数的类型，只能让数据进入函数之后再处理，能处理则罢，不能处理就报错。例如：

    >>> f("qiw", 2)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 1, in <lambda>
    TypeError: cannot concatenate 'str' and 'int' objects

本教程由于不属于这种概念争论范畴，所以不进行这方面的深入探索，仅仅是告诉各位读者相关信息。并且，本教程也是按照“人云亦云”的原则，既然大多数程序员都在讨论多态，那么我们就按照大多数人说的去介绍（尽管有时候真理掌握在少数人手中）。

“多态”，英文是:Polymorphism，在台湾被称作“多型”。维基百科中对此有详细解释说明。

>多型（英语：Polymorphism），是指物件导向程式执行时，相同的讯息可能会送給多个不同的类別之物件，而系统可依剧物件所属类別，引发对应类別的方法，而有不同的行为。简单来说，所谓多型意指相同的讯息給予不同的物件会引发不同的动作称之。

再简化的说法就是“有多种形式”，就算不知道变量（参数）所引用的对象类型，也一样能进行操作，来者不拒。比如上面显示的例子。在 Python 中，更为 Pthonic 的做法是根本就不进行类型检验。

例如著名的 `repr()` 函数，它能够针对输入的任何对象返回一个字符串。这就是多态的代表之一。

    >>> repr([1,2,3])
    '[1, 2, 3]'
    >>> repr(1)
    '1'
    >>> repr({"lang":"python"})
    "{'lang': 'Python'}"

使用它写一个小函数，还是作为多态代表的。

    >>> def length(x):
    ...     print "The length of", repr(x), "is", len(x)
    ... 

    >>> length("how are you")
    The length of 'how are you' is 11
    >>> length([1,2,3])
    The length of [1, 2, 3] is 3
    >>> length({"lang":"python","book":"itdiffer.com"})
    The length of {'lang': 'python', 'book': 'itdiffer.com'} is 2

不过，多态也不是万能的，如果这样做：
    
    >>> length(7)
    The length of 7 is
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 2, in length
    TypeError: object of type 'int' has no len()

报错了。看错误提示，明确告诉了我们 `object of type 'int' has no len()`。

在诸多介绍多态的文章中，都会有这样关于猫和狗的例子。这里也将代码贴出来，读者去体会所谓多态体现。其实，如果你进入了 Python 的语境，有时候是不经意就已经在应用多态特性呢。

    #!/usr/bin/env Python
    # coding=utf-8

    "the code is from: http://zetcode.com/lang/python/oop/"

    __metaclass__ = type

    class Animal:
        def __init__(self, name=""):
            self.name = name
    
        def talk(self):
            pass

    class Cat(Animal):
        def talk(self):
            print "Meow!"

    class Dog(Animal):
        def talk(self):
            print "Woof!"

    a = Animal()
    a.talk()

    c = Cat("Missy")
    c.talk()

    d = Dog("Rocky")
    d.talk()

保存后运行之：

    $ python 21101.py 
    Meow!
    Woof!

代码中有 Cat 和 Dog 两个类，都继承了类 Animal，它们都有 `talk()` 方法，输入不同的动物名称，会得出相应的结果。

关于多态，有一个被称作“鸭子类型”(duck typeing)的东西，其含义在维基百科中被表述为：

>在程序设计中，鸭子类型（英语：duck typing）是动态类型的一种风格。在这种风格中，一个对象有效的语义，不是由继承自特定的类或实现特定的接口，而是由当前方法和属性的集合决定。这个概念的名字来源于由 James Whitcomb Riley 提出的鸭子测试（见下面的“历史”章节），“鸭子测试”可以这样表述：“当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”

对于鸭子类型，也是有争议的。这方面的详细信息，读者可以去看有关维基百科的介绍。

对于多态问题，最后还要告诫读者，类型检查是毁掉多态的利器，比如 type、isinstance 以及 isubclass 函数，所以，一定要慎用这些类型检查函数。

## 封装和私有化

在正式介绍封装之前，先扯个笑话。

>某软件公司老板，号称自己懂技术。一次有一个项目要交付给客户，但是他有不想让客户知道实现某些功能的代码，但是交付的时候要给人家代码的。于是该老板就告诉程序员，“你们把那部分核心代码封装一下”。程序员听了之后，迷茫了。

不知道你有没有笑。

“封装”，是不是把代码写到某个东西里面，“人”在编辑器中打开，就看不到了呢？除非是你的显示器坏了。

在程序设计中，封装(Encapsulation)是对 object 的一种抽象，即将某些部分隐藏起来，在程序外部看不到，即无法调用（不是人用眼睛看不到那个代码，除非用某种加密或者混淆方法，造成现实上的困难，但这不是封装）。

要了解封装，离不开“私有化”，就是将类或者函数中的某些属性限制在某个区域之内，外部无法调用。

Python 中私有化的方法也比较简单，就是在准备私有化的属性（包括方法、数据）名字前面加双下划线。例如：

    #!/usr/bin/env Python
    # coding=utf-8

    __metaclass__ = type

    class ProtectMe:
        def __init__(self):
            self.me = "qiwsir"
            self.__name = "kivi"

        def __python(self):
            print "I love Python."

        def code(self):
            print "Which language do you like?"
            self.__python()

    if __name__ == "__main__":
        p = ProtectMe()
        print p.me
        print p.__name

运行一下，看看效果：

    $ python 21102.py
    qiwsir
    Traceback (most recent call last):
      File "21102.py", line 21, in <module>
        print p.__name
    AttributeError: 'ProtectMe' object has no attribute '__name'

查看报错信息，告诉我们没有`__name` 那个属性。果然隐藏了，在类的外面无法调用。再试试那个函数，可否？

    if __name__ == "__main__":
        p = ProtectMe()
        p.code()
        p.__python()
        
修改这部分即可。其中 `p.code()` 的意图是要打印出两句话：`"Which language do you like?"`和`"I love Python."`，`code()` 方法和`__python()` 方法在同一个类中，可以调用之。后面的那个 `p.__Python()` 试图调用那个私有方法。看看效果：

    $ python 21102.py 
    Which language do you like?
    I love Python.
    Traceback (most recent call last):
      File "21102.py", line 23, in <module>
        p.__python()
    AttributeError: 'ProtectMe' object has no attribute '__python'

如愿以偿。该调用的调用了，该隐藏的隐藏了。

用上面的方法，的确做到了封装。但是，我如果要调用那些私有属性，怎么办？

可以使用 `property` 函数。

    #!/usr/bin/env Python
    # coding=utf-8

    __metaclass__ = type

    class ProtectMe:
        def __init__(self):
            self.me = "qiwsir"
            self.__name = "kivi"

        @property
        def name(self):
            return self.__name
    
    if __name__ == "__main__":
        p = ProtectMe()
        print p.name

运行结果：

    $ python 21102.py 
    kivi

从上面可以看出，用了 `@property` 之后，在调用那个方法的时候，用的是 `p.name` 的形式，就好像在调用一个属性一样，跟前面 `p.me` 的格式相同。

看来，封装的确不是让“人看不见”。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：类(5)](./210.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：更多属性(1)](./212.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。