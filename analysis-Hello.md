# 分析 Hello

打开你写 Python 代码用的编辑器，不要问为什么，把下面的代码一个字不差地录入进去，并命名保存为 hello.py(目录自己任意定)。

	#!/usr/bin/env Python
	#coding:utf-8
	
	import tornado.httpserver
	import tornado.ioloop
	import tornado.options
	import tornado.web
	
	from tornado.options import define, options
	define("port", default=8000, help="run on the given port", type=int)
	
	class IndexHandler(tornado.web.RequestHandler):
	    def get(self):
	        greeting = self.get_argument('greeting', 'Hello')
	        self.write(greeting + ', welcome you to read: www.itdiffer.com')
	
	if __name__ == "__main__":
	    tornado.options.parse_command_line()
	    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
	    http_server = tornado.httpserver.HTTPServer(app)
	    http_server.listen(options.port)
	    tornado.ioloop.IOLoop.instance().start()

进入到保存 hello.py 文件的目录，执行：

    $ python hello.py

用 Python 运行这个文件，其实就已经发布了一个网站，只不过这个网站太简单了。

接下来，打开浏览器，在浏览器中输入：http://localhost:8000，得到如下界面：

![](./3images/30201.png)

我在 ubuntu 的 shell 中还可以用下面方式运行：

    $ curl http://localhost:8000/
    Hello, welcome you to read: www.itdiffer.com 
    
    $ curl http://localhost:8000/?greeting=Qiwsir
    Qiwsir, welcome you to read: www.itdiffer.com 

此操作，读者可以根据自己系统而定。

恭喜你，迈出了决定性一步，已经可以用 Tornado 发布网站了。在这里似乎没有做什么部署，只是安装了 Tornado。是的，不需要多做什么，因为 Tornado 就是一个很好的 server，也是一个开发框架。

下面以这个非常简单的网站为例，对用 tornado 做的网站的基本结构进行解释。

## WEB 服务器工作流程

任何一个网站都离不开 Web 服务器，这里所说的不是指那个更计算机一样的硬件设备，是指里面安装的软件，有时候初次接触的看官容易搞混。就来伟大的[维基百科都这么说](http://zh.wikipedia.org/wiki/%E6%9C%8D%E5%8A%A1%E5%99%A8)：

>有时，这两种定义会引起混淆，如 Web 服务器。它可能是指用于网站的计算机，也可能是指像 Apache 这样的软件，运行在这样的计算机上以管理网页组件和回应网页浏览器的请求。

在具体的语境中，看官要注意分析，到底指的是什么。

关于 Web 服务器比较好的解释，推荐看看百度百科的内容，我这里就不复制粘贴了，具体可以点击连接查阅：[WEB 服务器](http://baike.baidu.com/view/460250.htm)

在 WEB 上，用的最多的就是输入网址，访问某个网站。全世界那么多网站网页，如果去访问，怎么能够做到彼此互通互联呢。为了协调彼此，就制定了很多通用的协议，其中 http 协议，就是网络协议中的一种。关于这个协议的介绍，网上随处就能找到，请自己 google.

网上偷来的[一张图](http://kenby.iteye.com/blog/1159621)（从哪里偷来的，我都告诉你了，多实在呀。哈哈。），显示在下面，简要说明web服务器的工作过程

![](./3images/30202.png)

偷个彻底，把原文中的说明也贴上：

1. 创建 listen socket, 在指定的监听端口, 等待客户端请求的到来
2. listen socket 接受客户端的请求, 得到 client socket, 接下来通过 client socket 与客户端通信
3. 处理客户端的请求, 首先从 client socket 读取 http 请求的协议头, 如果是 post 协议, 还可能要读取客户端上传的数据, 然后处理请求, 准备好客户端需要的数据, 通过 client socket 写给客户端

## 引入模块

	import tornado.httpserver
	import tornado.ioloop
	import tornado.options
	import tornado.web

这四个都是 Tornado 的模块，在本例中都是必须的。它们四个在一般的网站开发中，都要用到，基本作用分别是：

- tornado.httpserver：这个模块就是用来解决 web 服务器的 http 协议问题，它提供了不少属性方法，实现客户端和服务器端的互通。Tornado 的非阻塞、单线程的特点在这个模块中体现。
- tornado.ioloop：这个也非常重要，能够实现非阻塞 socket 循环，不能互通一次就结束呀。
- tornado.options：这是命令行解析模块，也常用到。
- tornado.web：这是必不可少的模块，它提供了一个简单的 Web 框架与异步功能，从而使其扩展到大量打开的连接，使其成为理想的长轮询。

读者看到这里可能有点莫名其妙，对一些属于不理解。没关系，你可以先不用管它，如果愿意管，就把不理解属于放到 google 立面查查看。一定要硬着头皮一字一句地读下去，随着学习和实践的深入，现在不理解的以后就会逐渐领悟理解的。

还有一个模块引入，是用 from...import 完成的

	from tornado.options import define, options
	define("port", default=8000, help="run on the given port", type=int)

这两句就显示了所谓“命令行解析模块”的用途了。在这里通过 `tornado.options.define()` 定义了访问本服务器的端口，就是当在浏览器地址栏中输入 `http:localhost:8000` 的时候，才能访问本网站，因为 http 协议默认的端口是 80，为了区分，我在这里设置为 8000,为什么要区分呢？因为我的计算机或许你的也是，已经部署了别（或许是 Nginx、Apache）服务器了，它的端口是 80,所以要区分开（也可能是故意不用80端口），并且，后面我们还会将 tornado 和 Nginx 联合起来工作，这样两个服务器在同一台计算机上，就要分开喽。

## 定义请求-处理程序类

	class IndexHandler(tornado.web.RequestHandler):
	    def get(self):
	        greeting = self.get_argument('greeting', 'Hello')
	        self.write(greeting + ', welcome you to read: www.itdiffer.com')

所谓“请求处理”程序类，就是要定义一个类，专门应付客户端（就是你打开的那个浏览器界面）向服务器提出的请求（这个请求也许是要读取某个网页，也许是要将某些信息存到服务器上），服务器要有相应的程序来接收并处理这个请求，并且反馈某些信息（或者是针对请求反馈所要的信息，或者返回其它的错误信息等）。

于是，就定义了一个类，名字是 IndexHandler，当然，名字可以随便取了，但是，按照习惯，类的名字中的单词首字母都是大写的，并且如果这个类是请求处理程序类，那么就最好用 Handler 结尾，这样在名称上很明确，是干什么的。

类 IndexHandler 继承 `tornado.web.RequestHandler`,其中再定义 `get()` 和 `post()` 两个在 web 中应用最多的方法的内容（关于这两个方法的详细解释，可以参考：[HTTP GET POST 的本质区别详解](https://github.com/qiwsir/ITArticles/blob/master/Tornado/DifferenceHttpGetPost.md)，作者在这篇文章中，阐述了两个方法的本质）。

在本例中，只定义了一个 `get()` 方法。

用 `greeting = self.get_argument('greeting', 'Hello')` 的方式可以得到 url 中传递的参数，比如

    $ curl http://localhost:8000/?greeting=Qiwsir
    Qiwsir, welcome you to read: www.itdiffer.com 

就得到了在 url 中为 greeting 设定的值 Qiwsir。如果 url 中没有提供值，就是 Hello.

官方文档对这个方法的描述如下：

>RequestHandler.get_argument(name, default=, []strip=True)

>Returns the value of the argument with the given name.

>If default is not provided, the argument is considered to be required, and we raise a MissingArgumentError if it is missing.

>If the argument appears in the url more than once, we return the last value.

>The returned value is always unicode.

接下来的那句 `self.write(greeting + ',weblcome you to read: www.itdiffer.com)'`中，`write()` 方法主要功能是向客户端反馈信息。也浏览一下官方文档信息，对以后正确理解使用有帮助：

>RequestHandler.write(chunk)[source]

>Writes the given chunk to the output buffer.

>To write the output to the network, use the flush() method below.

>If the given chunk is a dictionary, we write it as JSON and set the Content-Type of the response to be application/json. (if you want to send JSON as a different Content-Type, call set_header after calling write()).

## main() 方法

`if __name__ == "__main__"`,这个方法跟以往执行 Python 程序是一样的。

`tornado.options.parse_command_line()`,这是在执行 tornado 的解析命令行。在 tornado 的程序中，只要 import 模块之后，就会在运行的时候自动加载，不需要了解细节，但是，在 main（）方法中如果有命令行解析，必须要提前将模块引入。

## Application 类

下面这句是重点：

    app = tornado.web.Application(handlers=[(r"/", IndexHandler)])
    
将 tornado.web.Application 类实例化。这个实例化，本质上是建立了整个网站程序的请求处理集合，然后它可以被 HTTPServer 做为参数调用，实现 http 协议服务器访问。Application 类的`__init__`方法参数形式：

    def __init__(self, handlers=None, default_host="", transforms=None,**settings):
        pass

在一般情况下，handlers 是不能为空的，因为 Application 类通过这个参数的值处理所得到的请求。例如在本例中，`handlers=[(r"/", IndexHandler)]`，就意味着如果通过浏览器的地址栏输入根路径（`http://localhost:8000` 就是根路径，如果是 `http://localhost:8000/qiwsir`，就不属于根，而是一个子路径或目录了），对应着就是让名字为 IndexHandler 类处理这个请求。

通过 handlers 传入的数值格式，一定要注意，在后面做复杂结构的网站是，这里就显得重要了。它是一个 list，list 里面的元素是 tuple，tuple 的组成包括两部分，一部分是请求路径，另外一部分是处理程序的类名称。注意请求路径可以用正则表达式书写(关于正则表达式，后面会进行简要介绍)。举例说明：

    handlers = [
        (r"/", IndexHandlers),              #来自根路径的请求用 IndesHandlers 处理
        (r"/qiwsir/(.*)", QiwsirHandlers),  #来自 /qiwsir/ 以及其下任何请求（正则表达式表示任何字符）都由 QiwsirHandlers 处理
    ]

**注意**

在这里我使用了 `r"/"`的样式，意味着就不需要使用转义符，r 后面的都表示该符号本来的含义。例如，\n，如果单纯这么来使用，就以为着换行，因为符号“\”具有转义功能（关于转义详细阅读[《字符串(1)》](./106.md)），当写成 `r"\n"` 的形式是，就不再表示换行了，而是两个字符，\ 和 n，不会转意。一般情况下，由于正则表达式和 \ 会有冲突，因此，当一个字符串使用了正则表达式后，最好在前面加上'r'。

关于 Application 类的介绍，告一段落，但是并未完全讲述了，因为还有别的参数设置没有讲，请继续关注后续内容。

## HTTPServer 类

实例化之后，Application 对象（用app做为标签的）就可以被另外一个类 HTTPServer 引用，形式为：

    http_server = tornado.httpserver.HTTPServer(app)

HTTPServer 是 tornado.httpserver 里面定义的类。HTTPServer 是一个单线程非阻塞 HTTP 服务器，执行 HTTPServer 一般要回调 Application 对象，并提供发送响应的接口,也就是下面的内容是跟随上面语句的（options.port 的值在 IndexHandler 类前面通过 from...import.. 设置的）。

    http_server.listen(options.port)

这种方法，就建立了单进程的 http 服务。

请看官牢记，如果在以后编码中，遇到需要多进程，请参考官方文档说明：<http://tornado.readthedocs.org/en/latest/httpserver.html#http-server>

## IOLoop 类

剩下最后一句了：

    tornado.ioloop.IOLoop.instance().start()

这句话，总是在`__main()__`的最后一句。表示可以接收来自 HTTP 的请求了。

以上把一个简单的 hello.py 剖析。想必读者对 Tornado 编写网站的基本概念已经有了。

如果一头雾水，也不要着急，以来将上面的内容多看几遍。对整体结构有一个基本了解，不要拘泥于细节或者某些词汇含义。然后即继续学习。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：为做网站而准备](./301.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：用 tornado 做网站(1)](./303.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。