# 为做网站而准备

作为一个程序猿一定要会做网站。这也不一定吧，貌似是，但是，如果被人问及此事，如果说自己不会，的确羞愧难当呀。所以，本教程要讲一讲如何做网站。

>推荐阅读：[History of the World Wide Web](http://en.wikipedia.org/wiki/History_of_the_World_Wide_Web)

首先，为自己准备一个服务器。这个要求似乎有点过分，作为一个普通的穷苦聊到的程序员，哪里有铜钿来购买服务器呢？没关系，不够买服务器也能做网站，可以购买云服务空间或者虚拟空间，这个在网上搜搜，很多。如果购买这个的铜钿也没有，还可以利用自己的电脑（这总该有了）作为服务服务器。我就是利用一台装有 ubuntu 操作系统的个人电脑作为本教程的案例演示服务器。

然后，要在这个服务器上做一些程序配置。一些必备的网络配置这里就不说了，比如我用的 ubuntu 系统，默认情况都有了。如果读者遇到一些问题，可以搜一下，网上资料多多。另外的配置就是 Python 开发环境，这个应该也有了，前面已经在用了。

接下来，要安装一个框架。本教程中制作网站的案例采用 tornado 框架。

在安装这个框架之前，先了解一些相关知识。

## 开发框架

对框架的认识，由于工作习惯和工作内容的不同，有很大差异，这里姑且截取[维基百科中的一种定义](http://zh.wikipedia.org/wiki/%E8%BB%9F%E9%AB%94%E6%A1%86%E6%9E%B6)，之所以要给出一个定义，无非是让看官有所了解，但是是否知道这个定义，丝毫不影响后面的工作。

>软件框架（Software framework），通常指的是为了实现某个业界标准或完成特定基本任务的软件组件规范，也指为了实现某个软件组件规范时，提供规范所要求之基础功能的软件产品。

>框架的功能类似于基础设施，与具体的软件应用无关，但是提供并实现最为基础的软件架构和体系。软件开发者通常依据特定的框架实现更为复杂的商业运用和业务逻辑。这样的软件应用可以在支持同一种框架的软件系统中运行。

>简而言之，框架就是制定一套规范或者规则（思想），大家（程序员）在该规范或者规则（思想）下工作。或者说就是使用别人搭好的舞台，你来做表演。

我比较喜欢最后一句的解释，别人搭好舞台，我来表演。这也就是说，如果在做软件开发的时候，能够减少工作量。就做网站来讲，其实需要做的事情很多，但是如果有了开发框架，很多底层的事情就不需要做了（都有哪些底层的事情呢？读者能否回答？）。

有高手工程师鄙视框架，认为自己编写的才是王道。这方面不争论，框架是开发中很流行的东西，我还是固执地认为用框架来开发，更划算。

## Python 框架

有人说 php(什么是 php，严肃的说法，这是另外一种语言，更高雅的说法，是某个活动的汉语拼音简称）框架多，我不否认，php 的开发框架的确很多很多。不过，Python 的 web 开发框架，也足够使用了，列举几种常见的 web 框架：

- Django:这是一个被广泛应用的框架。在网上搜索，会发现很多公司在招聘的时候就说要会这个。框架只是辅助，真正的程序员，用什么框架，都应该是根据需要而来。当然不同框架有不同的特点，需要学习一段时间。
- Flask：一个用 Python 编写的轻量级 Web 应用框架。基于 Werkzeug WSGI 工具箱和 Jinja2 模板引擎。
- Web2py：是一个为 Python 语言提供的全功能 Web 应用框架，旨在敏捷快速的开发 Web 应用，具有快速、安全以及可移植的数据库驱动的应用，兼容 Google App Engine。
- Bottle: 微型 Python Web 框架，遵循 WSGI，说微型，是因为它只有一个文件，除 Python 标准库外，它不依赖于任何第三方模块。
- Tornado：全称是 Tornado Web Server，从名字上看就可知道它可以用作 Web 服务器，但同时它也是一个 Python Web 的开发框架。最初是在 FriendFeed 公司的网站上使用，FaceBook 收购了之后便开源了出来。
- webpy: 轻量级的 Python Web 框架。webpy 的设计理念力求精简（Keep it simple and powerful），源码很简短，只提供一个框架所必须的东西，不依赖大量的第三方模块，它没有 URL 路由、没有模板也没有数据库的访问。

说明：以上信息选自：<http://blog.jobbole.com/72306/>，这篇文章中还有别的框架，由于不是 web 框架，我没有选摘，有兴趣的去阅读。

## Tornado

本教程中将选择使用 Tornado 框架。此前有朋友建议我用 Django，首先它是一个好东西。但是，我更愿意用 Tornado,为什么呢？因为......，看下边或许是理由，或许不是。

Tornado 全称 Tornado Web Server，是一个用 Python 语言写成的 Web 服务器兼 Web 应用框架，由 FriendFeed 公司在自己的网站 FriendFeed 中使用，被 Facebook 收购以后框架以开源软件形式开放给大众。看来 Tornado 的出身高贵呀，对了，某国可能风闻有 Facebook，但是要一睹其芳容，还要努力。

用哪个框架，一般是要结合项目而定。我之选用 Tornado 的原因，就是看中了它在性能方面的优异表现。

Tornado 的性能是相当优异的，因为它试图解决一个被称之为“C10k”问题，就是处理大于或等于一万的并发。一万呀，这可是不小的量。(关于 C10K 问题，看官可以浏览：[C10k problem](http://en.wikipedia.org/wiki/C10k_problem))

下表是和一些其他 Web 框架与服务器的对比，供看官参考（数据来源： [https://developers.facebook.com/blog/post/301](https://developers.facebook.com/blog/post/301) ）

条件：处理器为 AMD Opteron, 主频 2.4GHz, 4 核

|服务| 	部署 |	请求/每秒|
|----|-------|-----------|
|Tornado| nginx, 4 进程|8213|
|Tornado|1 个单线程进程|3353|
|Django|Apache/mod_wsgi|2223|
|web.py|Apache/mod_wsgi|2066|
|CherryPy|独立|785|

看了这个对比表格，还有什么理由不选择 Tornado 呢？

就是它了——**Tornado**

## 安装 Tornado

Tornado 的官方网站：[http://www.tornadoweb.org](http://www.tornadoweb.org/en/latest/)

我在自己电脑中（是我目前使用的服务器），用下面方法安装，只需要一句话即可：

    pip install tornado

这是因为 Tornado 已经列入 PyPI，因此可以通过 pip 或者 easy_install 来安装。

如果不用这种方式安装，下面的页面中有可以供看官下载的最新源码版本和安装方式：[https://pypi.python.org/pypi/tornado/](https://pypi.Python.org/pypi/tornado/)

此外，在 github 上也有托管，看官可以通过上述页面进入到 github 看源码。

我没有在 windows 操作系统上安装过这个东西，不过，在官方网站上有一句话，可能在告诉读者一些信息：

>Tornado will also run on Windows, although this configuration is not officially supported and is recommended only for development use.

特别建议，在真正的工程中，网站的服务器还是用 Linux 比较好，你懂得（吗？）。

## 技术准备

除了做好上述准备之外，还要有点技术准备：

- HTML
- CSS
- JavaScript

我们在后面实例中，不会搞太复杂的界面和 JavaScript(JS) 操作，所以，只需要基本知识即可。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：实战-引](./300.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：分析 Hello](./302.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。