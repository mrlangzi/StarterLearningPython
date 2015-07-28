# 练习

已经将 Python 的基础知识学习完毕，包含基本的数据类型（或者说对象类型）和语句。利用这些，加上个人的聪明才智，就能解决一些问题了。

### 练习 1

**问题描述**

有一个列表，其中包括 10 个元素，例如这个列表是[1,2,3,4,5,6,7,8,9,0],要求将列表中的每个元素一次向前移动一个位置，第一个元素到列表的最后，然后输出这个列表。最终样式是[2,3,4,5,6,7,8,9,0,1]

**解析**

或许刚看题目的读者，立刻想到把列表中的第一个元素拿出来，然后追加到最后，不就可以了吗？是的。就是这么简单。主要是联系一下已经学习过的列表操作。

看下面代码之前，不妨自己写一写试试。然后再跟我写的对照。

**注意，我在这里所写的代码不能算标准答案。只能是参考。很可能你写的比我写的还要好。在代码界，没有标准答案。**

参考代码如下，这个我保存为 12901.py 文件

    #!/usr/bin/env python
    # coding=utf-8

    raw = [1,2,3,4,5,6,7,8,9,0]
    print raw
    
    b = raw.pop(0)
    raw.append(b)
    print raw

执行这个文件：

    $ python 12901.py
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
    [2, 3, 4, 5, 6, 7, 8, 9, 0, 1]

第一行所打印的是原来的列表，第二行是需要的列表。这里用到的主要是列表的两个函数 `pop()`和 `append()`。如果读者感觉不是很熟悉，或者对这个问题，在我提供的参考之前只有一个模糊认识，但是没有明晰地写出代码，说明对前面的函数还没有烂熟于胸。唯一的方法就是多练习。

### 练习 2

**问题描述**

按照下面的要求实现对列表的操作：

1. 产生一个列表，其中有 40 个元素，每个元素是 0 到 100 的一个随机整数
2. 如果这个列表中的数据代表着某个班级 40 人的分数，请计算成绩低于平均分的学生人数，并输出
3. 对上面的列表元素从大到小排序

**解析**

这个问题中，需要几个知识点：

第一个是随机产生整数。一种方法是你做 100 个纸片，分别写上 1 到 100 的数字（每张上一个整数），然后放到一个盒子里面。抓出一个，看是几，就讲这个数字写到列表中，直到抓出第 40 个。这样得到的列表是随机了。但是，好像没有 Python 什么事情。那么久要用另外一种方法，让 Python 来做。Python 中有一个模块：random，专门提供随机事件的。

    >>> dir(random)
    ['BPF', 'LOG4', 'NV_MAGICCONST', 'RECIP_BPF', 'Random', 'SG_MAGICCONST', 'SystemRandom', 'TWOPI', 'WichmannHill', '_BuiltinMethodType', '_MethodType', '__all__', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '_acos', '_ceil', '_cos', '_e', '_exp', '_hashlib', '_hexlify', '_inst', '_log', '_pi', '_random', '_sin', '_sqrt', '_test', '_test_generator', '_urandom', '_warn', 'betavariate', 'choice', 'division', 'expovariate', 'gammavariate', 'gauss', 'getrandbits', 'getstate', 'jumpahead', 'lognormvariate', 'normalvariate', 'paretovariate', 'randint', 'random', 'randrange', 'sample', 'seed', 'setstate', 'shuffle', 'triangular', 'uniform', 'vonmisesvariate', 'weibullvariate']

在这个问题中，只需要 `random.randint()`，专门获取某个范围内的随机整数。

第二个是求平均数，方法是将所有数字求和，然后除以总人数（40）。求和方法就是 `sum()`函数。在计算平均数的时候，要注意，一般平均数不能仅仅是整数，最好保留一位小数吧。这是除法中的知识了。

第三个是列表排序。

下面就依次展开。不忙，在我开始之前，你先试试吧。

    #!/usr/bin/env Python
    # coding=utf-8

    from __future__ import division
    import random

    score = [random.randint(0,100) for i in range(40)]    #0 到 100 之间，随机得到 40 个整数，组成列表
    print score

    num = len(score)
    sum_score = sum(score)               #对列表中的整数求和
    ave_num = sum_score/num              #计算平均数
    less_ave = len([i for i in score if i<ave_num])    #将小于平均数的找出来，组成新的列表，并度量该列表的长度
    print "the average score is:%.1f" % ave_num
    print "There are %d students less than average." % less_ave

    sorted_score = sorted(score, reverse=True)    #对原列表排序
    print sorted_score

## 练习 3

**问题描述**

如果将一句话作为一个字符串，那么这个字符串中必然会有空格（这里仅讨论英文），比如"How are you."，但有的时候，会在两个单词之间多大一个空格。现在的任务是，如果一个字符串中有连续的两个空格，请把它删除。

**解析**

对于一个字符串中有空格，可以使用[《字符串(4)》](./109.md)中提到的 `strip()`等。但是，它不是仅仅去掉一个空格，而是把字符串两遍的空格都去掉。都去掉似乎也没有什么关系，再用空格把单词拼起来就好了。

按照这个思路，我这样写代码，供你参考（更建议你先写出一段来，然后我们两个对照）。

    #!/usr/bin/env Python
    # coding=utf-8

    string = "I love  code."    #在 code 前面有两个空格，应该删除一个
    print string                #为了能够清楚看到每步的结果，把过程中的量打印出来

    str_lst = string.split(" ")    #以空格为分割，得到词汇的列表
    print str_lst

    words = [s.strip() for s in str_lst]    #去除单词两边的空格
    print words

    new_string = " ".join(words)    #以空格为连接符，将单词链接起来
    print new_string

保存之后，运行这个代码，结果是：

    I love  code.
    ['I', 'love', '', 'code.']
    ['I', 'love', '', 'code.']
    I love  code.

结果是令人失望的。经过一番折腾，空格根本就没有被消除。最后的输出和一开始的字符串完全一样。泪奔！

查找原因。

从输出中已经清楚表示了。当执行 `string.split(" ")`的时候，是以空格为分割符，将字符串分割，并返回列表。列表中元素是由单词组成。原来字符串中单词之间的空格已经被作为分隔符，那么列表中单词两遍就没有空格了。所以，前面代码中就无需在用 `strip()`去删除空格。另外，特别要注意的是，有两个空格连着呢，其中一个空格作为分隔符，另外一个空格就作为列表元素被返回了。这样一来，分割之后的操作都无作用了。

看官是否明白错误原因了？

如何修改？显然是分割之后，不能用 `strip()`，而是要想办法把那个返回列表中的空格去掉，得到只含有单词的列表。再用空格连接之，就应该对了。所以，我这样修正它。

    #!/usr/bin/env Python
    # coding=utf-8

    string = "I love  code."
    print string

    str_lst = string.split(" ")
    print str_lst

    words = [s for s in str_lst if s!=""]    #利用列表解析，将空格检出
    print words

    new_string = " ".join(words)
    print new_string

将文件保存，名为 12903.py，运行之得到下面结果：

    I love  code.
    ['I', 'love', '', 'code.']
    ['I', 'love', 'code.']
    I love code.

OK！完美地解决了问题，去除了 code 前面的一个空格。

## 练习 4

**问题描述**

>根剧高德纳（Donald Ervin Knuth）的《计算机程序设计艺术》（The Art of Computer Programming），1150 年印度数学家 Gopala 和金月在研究箱子包装物件长宽刚好为 1 和 2 的可行方法数目时，首先描述这个数列。 在西方，最先研究这个数列列的人是比萨的李奥纳多（意大利人斐波那契 Leonardo Fibonacci），他描述兔子生長的数目時用上了这数列。

>第一个月初有一对刚诞生的兔子;第二个月之后（第三个月初）他们可以生育,每月每对可生育的兔子会诞生下一对新兔子;兔子永不死去

>假设计 n 月有可生育的兔子总共 a 对，n+1 月就总共有 b 对。在 n+2 月必定总共有 a+b 对： 因为在 n+2 月的时候，前一月（n+1 月）的 b 对兔子可以存留至第 n+2 月（在当月属于新诞生的兔子尚不能生育）。而新生育出的兔子對数等于所有在 n 月就已存在的 a 对

上面故事是一个著名的数列——斐波那契数列——的起源。斐波那契数列用数学方式表示就是：
 
    a0 = 0                (n=0)
    a1 = 1                (n=1)
    a[n] = a[n-1] + a[n-2]  (n>=2)

我们要做的事情是用程序计算出 n=100 是的值。

在解决这个问题之前，你可以先观看一个[关于斐波那契数列数列的视频](http://swf.ws.126.net/openplayer/v02/-0-2_M9HKRT25D_M9HNA0UNO-vimg1_ws_126_net//image/snapshot_movie/2014/1/6/L/M9HNA8H6L-.swf)，注意，请在墙内欣赏。

**解析**

斐波那契数列是各种编程语言中都要秀一下的东西，通常用在阐述“递归”中。什么是递归？后面的 Python 中也会讲到。不过，在这里不准备讲。

其实，如果用递归来写，会更容易明白。但是，这里我给出一个用 for 循环写的，看看是否能够理解之。

    #!/usr/bin/env Python
    # coding=utf-8

    a, b = 0, 1

    for i in range(4):    #改变这里的数，就能得到相应项的结果
        a, b = b, a+b

    print a

保存运行之，看看结果和你推算的是否一致。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：迭代](./128.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：自省](./130.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。