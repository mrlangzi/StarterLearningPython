# 用 tornado 做网站 (7)

到上一节结束，其实读者已经能够做一个网站了，但是，仅仅用前面的技术来做的网站，仅能算一个小网站，在[《为做网站而准备》](./301.md)中，说明之所以选 tornado，就是因为它能够解决 c10k 问题，即能够实现大用户量访问。

要实现大用户量访问，必须要做的就是：异步。除非你是很土的土豪。

## 相关概念

### 同步和异步

有不少资料对这两个概念做了不同角度和层面的解释。在我来看，一个最典型的例子就是打电话和发短信。

- 打电话就是同步。张三给李四打电话，张三说：“是李四吗？”。当这个信息被张三发出，提交给李四，就等待李四的响应（一般会听到“是”，或者“不是”），只有得到了李四返回的信息之后，才能进行后续的信息传送。
- 发短信是异步。张三给李四发短信，编辑了一句话“今晚一起看老齐的零基础学 Python”，发送给李四。李四或许马上回复，或许过一段时间，这段时间多长也不定，才回复。总之，李四不管什么时候回复，张三会以听到短信铃声为提示查看短信。

以上方式理解“同步”和“异步”不是很精准，有些地方或有牵强。要严格理解，需要用严格一点的定义表述（以下表述参照了[知乎](http://www.zhihu.com/question/19732473)上的回答）：

>同步和异步关注的是消息通信机制 (synchronous communication/ asynchronous communication)

>所谓同步，就是在发出一个“调用”时，在没有得到结果之前，该“调用”就不返回。但是一旦调用返回，就得到返回值了。
换句话说，就是由“调用者”主动等待这个“调用”的结果。

>而异步则是相反，“调用”在发出之后，这个调用就直接返回了，所以没有返回结果。换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在“调用”发出后，“被调用者”通过状态、通知来通知调用者，或通过回调函数处理这个调用。

可能还是前面的打电话和发短信更好理解。

### 阻塞和非阻塞

“阻塞和非阻塞”与“同步和异步”常常被换为一谈，其实它们之间还是有差别的。如果按照一个“差不多”先生的思维方法，你也可以不那么深究它们之间的学理上的差距，反正在你的程序中，会使用就可以了。不过，必要的严谨还是需要的，特别是我写这个教程，要装扮的让别人看来自己懂，于是就再引用[知乎](http://www.zhihu.com/question/19732473)上的说明（我个人认为，别人已经做的挺好的东西，就别重复劳动了，“拿来主义”，也不错。或许你说我抄袭和山寨，但是我明确告诉你来源了）：

>阻塞和非阻塞关注的是程序在等待调用结果（消息，返回值）时的状态.

>阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。非阻塞调用指在不能立刻得到结果之前，该调用不会阻塞当前线程。

按照这个说明，发短信就是显然的非阻塞，发出去一条短信之后，你利用手机还可以干别的，乃至于再发一条“老齐的课程没意思，还是看 PHP 刺激”也是可以的。

关于这两组基本概念的辨析，不是本教程的重点，读者可以参阅这篇文章：[http://www.cppblog.com/converse/archive/2009/05/13/82879.html](http://www.cppblog.com/converse/archive/2009/05/13/82879.html)，文章作者做了细致入微的辨析。

## tornado 的同步

此前，在 tornado 基础上已经完成的 web，就是同步的、阻塞的。为了更明显的感受这点，不妨这样试一试。

在 handlers 文件夹中建立一个文件，命名为 sleep.py

    #!/usr/bin/env python
    # coding=utf-8

    from base import BaseHandler

    import time

    class SleepHandler(BaseHandler):
        def get(self):
            time.sleep(17)
            self.render("sleep.html")

    class SeeHandler(BaseHandler):
        def get(self):
            self.render("see.html")

其它的事情，如果读者对我在[《用 tornado 做网站 (1)》](./303.md)中所讲述的网站框架熟悉，应该知道如何做了，不熟悉，请回头复习。

sleep.html 和 see.html 是两个简单的模板，内容可以自己写。别忘记修改 url.py 中的目录。

然后的测试稍微复杂一点点，就是打开浏览器之后，打开两个标签，分别在两个标签中输入 `localhost:8000/sleep`（记为标签 1）和 `localhost:8000/see`（记为标签 2），注意我用的是 8000 端口。输入之后先不要点击回车去访问。做好准备，记住切换标签可以用“ctrl-tab”组合键。

1. 执行标签 1，让它访问网站；
2. 马上切换到标签 2，访问网址。
3. 注意观察，两个标签页面，是不是都在显示正在访问，请等待。
4. 当标签 1 不呈现等待提示（比如一个正在转的圆圈）时，标签 2 的表现如何？几乎同时也访问成功了。

建议读者修改 sleep.py 中的 time.sleep(17) 这个值，多试试。很好玩的吧。

当然，这是比较笨拙的方法，本来是可以通过测试工具完成上述操作比较的。怎奈要用别的工具，还要进行介绍，又多了一个分散精力的东西，故用如此笨拙的方法，权当有一个体会。

## 异步设置

tornado 本来就是一个异步的服务框架，体现在 tornado 的服务器和客户端的网络交互的异步上，起作用的是 tornado.ioloop.IOLoop。但是如果的客户端请求服务器之后，在执行某个方法的时候，比如上面的代码中执行 get() 方法的时候，遇到了 `time.sleep(17)` 这个需要执行时间比较长的操作，耗费时间，就会使整个 tornado 服务器的性能受限了。

为了解决这个问题，tornado 提供了一套异步机制，就是异步装饰器 `@tornado.web.asynchronous`：

    #!/usr/bin/env Python
    # coding=utf-8
    
    import tornado.web
    from base import BaseHandler
    
    import time
    
    class SleepHandler(BaseHandler):
        @tornado.web.asynchronous
        def get(self):
            tornado.ioloop.IOLoop.instance().add_timeout(time.time() + 17, callback=self.on_response)
        def on_response(self):
            self.render("sleep.html")
            self.finish()

将 sleep.py 的代码如上述一样改造，即在 get() 方法前面增加了装饰器 `@tornado.web.asynchronous`，它的作用在于将 tornado 服务器本身默认的设置`_auto_fininsh` 值修改为 false。如果不用这个装饰器，客户端访问服务器的 get() 方法并得到返回值之后，两只之间的连接就断开了，但是用了 `@tornado.web.asynchronous` 之后，这个连接就不关闭，直到执行了 `self.finish()` 才关闭这个连接。

`tornado.ioloop.IOLoop.instance().add_timeout()` 也是一个实现异步的函数，`time.time()+17` 是给前面函数提供一个参数，这样实现了相当于 `time.sleep(17)` 的功能，不过，还没有完成，当这个操作完成之后，就执行回调函数 `on_response()` 中的 `self.render("sleep.html")`，并关闭连接 `self.finish()`。

过程清楚了。所谓异步，就是要解决原来的 `time.sleep(17)` 造成的服务器处理时间长，性能下降的问题。解决方法如上描述。

读者看这个代码，或许感觉有点不是很舒服。如果有这么一点感觉，是正常的。因为它里面除了装饰器之外，用到了一个回调函数，它让代码的逻辑不是平铺下去，而是被分割为了两段。第一段是 `tornado.ioloop.IOLoop.instance().add_timeout(time.time() + 17, callback=self.on_response)`，用`callback=self.on_response` 来使用回调函数，并没有如同改造之前直接 `self.render("sleep.html")`；第二段是回调函数 on_response(self)`，要在这个函数里面执行 `self.render("sleep.html")`，并且以 `self.finish()`结尾以关闭连接。

这还是执行简单逻辑，如果复杂了，不断地要进行“回调”，无法让逻辑顺利延续，那面会“眩晕”了。这种现象被业界成为“代码逻辑拆分”，打破了原有逻辑的顺序性。为了让代码逻辑不至于被拆分的七零八落，于是就出现了另外一种常用的方法：

    #!/usr/bin/env Python
    # coding=utf-8
    
    import tornado.web
    import tornado.gen
    from base import BaseHandler
    
    import time

    class SleepHandler(tornado.web.RequestHandler):
        @tornado.gen.coroutine
        def get(self):
            yield tornado.gen.Task(tornado.ioloop.IOLoop.instance().add_timeout, time.time() + 17)
            #yield tornado.gen.sleep(17)
            self.render("sleep.html")

从整体上看，这段代码避免了回调函数，看着顺利多了。

再看细节部分。

首先使用的是 `@tornado.gen.coroutine` 装饰器，所以要在前面有 `import tornado.gen`。跟这个装饰器类似的是 `@tornado.gen.engine` 装饰器，两者功能类似，有一点细微差别。请阅读[官方对此的解释](http://www.tornadoweb.org/en/stable/gen.html)：

>This decorator(指 engine) is similar to coroutine, except it does not return a Future and the callback argument is not treated specially.

`@tornado.gen.engine` 是古时候用的，现在我们都使用 `@tornado.gen.corroutine` 了，这个是在 tornado 3.0 以后开始。在网上查阅资料的时候，会遇到一些使用 `@tornado.gen.engine` 的，但是在你使用或者借鉴代码的时候，就勇敢地将其修改为 `@tornado.gen.coroutine` 好了。有了这个装饰器，就能够控制下面的生成器的流程了。

然后就看到 get() 方法里面的 yield 了，这是一个生成器（参阅本教程[《生成器》](./215.md)）。`yield tornado.gen.Task(tornado.ioloop.IOLoop.instance().add_timeout, time.time() + 17)` 的执行过程，应该先看括号里面，跟前面的一样，是来替代 `time.sleep(17)` 的，然后是 `tornado.gen.Task()` 方法，其作用是“Adapts a callback-based asynchronous function for use in coroutines.”（由于怕翻译后遗漏信息，引用[原文](http://tornado.readthedocs.org/en/latest/gen.html)）。返回后，最后使用 yield 得到了一个生成器，先把流程挂起，等完全完毕，再唤醒继续执行。要提醒读者，生成器都是异步的。

其实，上面啰嗦一对，可以用代码中注释了的一句话来代替 `yield tornado.gen.sleep(17)`，之所以扩所，就是为了顺便看到 `tornado.gen.Task()` 方法，因为如果读者在看古老的代码时候，会遇到。但是，后面你写的时候，就不要那么啰嗦了，请用 `yield tornado.gen.sleep()`。

至此，基本上对 tornado 的异步设置有了概览，不过，上面的程序在实际中没有什么价值。在工程中，要让 tornado 网站真正异步起来，还要做很多事情，不仅仅是如上面的设置，因为很多东西，其实都不是异步的。

## 实践中的异步

以下各项同步（阻塞）的，如果在 tornado 中按照之前的方式只用它们，就是把 tornado 的非阻塞、异步优势削减了。

- 数据库的所有操作，不管你的数据是 SQL 还是 noSQL，connect、insert、update 等
- 文件操作，打开，读取，写入等
- time.sleep，在前面举例中已经看到了
- smtplib，发邮件的操作
- 一些网络操作，比如 tornado 的 httpclient 以及 pycurl 等

除了以上，或许在编程实践中还会遇到其他的同步、阻塞实践。仅仅就上面几项，就是编程实践中经常会遇到的，怎么解决？

聪明的大牛程序员帮我们做了扩展模块，专门用来实现异步/非阻塞的。

- 在数据库方面，由于种类繁多，不能一一说明，比如 mysql，可以使用[adb](https://github.com/ovidiucp/pymysql-benchmarks)模块来实现 python 的异步 mysql 库；对于 mongodb 数据库，有一个非常优秀的模块，专门用于在 tornado 和 mongodb 上实现异步操作，它就是 motor。特别贴出它的 logo，我喜欢。官方网站：[http://motor.readthedocs.org/en/stable/](http://motor.readthedocs.org/en/stable/)上的安装和使用方法都很详细。

![](./3images/30901.png)

- 文件操作方面也没有替代模块，只能尽量控制好 IO，或者使用内存型（Redis）及文档型（MongoDB）数据库。
- time.sleep() 在 tornado 中有替代：`tornado.gen.sleep()` 或者 `tornado.ioloop.IOLoop.instance().add_timeout`，这在前面代码已经显示了。
- smtp 发送邮件，推荐改为 tornado-smtp-client。
- 对于网络操作，要使用 tornado.httpclient.AsyncHTTPClient。

其它的解决方法，只能看到问题具体说了，甚至没有很好的解决方法。不过，这里有一个列表，列出了足够多的库，供使用者选择：[Async Client Libraries built on tornado.ioloop](https://github.com/tornadoweb/tornado/wiki/Links)，同时这个页面里面还有很多别的链接，都是很好的资源，建议读者多看看。

教程到这里，读者是不是要思考一个问题，既然对于 mongodb 有专门的 motor 库来实现异步，前面对于 tornado 的异步，不管是哪个装饰器，都感觉麻烦，有没有专门的库来实现这种异步呢？这不是异想天开，还真有。也应该有，因为这才体现python的特点。比如[greenlet-tornado](https://github.com/mopub/greenlet-tornado)，就是一个不错的库。读者可以浏览官方网站深入了解（为什么对 mysql 那么不积极呢？按理说应该出来好多支持 mysql 异步的库才对）。

必须声明，前面演示如何在 tornado 中设置异步的代码，仅仅是演示理解设置方法。在工程实践中，那个代码的意义不到。为此，应该有一个近似于实践的代码示例。是的，的确应该有。当我正要写这样的代码时候，在网上发现一篇文章，这篇文章阻止了我写，因为我要写的那篇文章的作者早就写好了，而且我认为表述非常到位，示例也详细。所以，我不得不放弃，转而推荐给读者这篇好文章：

举例：[http://emptysqua.re/blog/refactoring-tornado-coroutines/](http://emptysqua.re/blog/refactoring-tornado-coroutines/)

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：用 tornado 做网站 (6)](./308.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：为计算做准备](./310.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。