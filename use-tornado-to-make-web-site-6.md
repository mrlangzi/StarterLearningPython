# 用 tornado 做网站 (6)

在[上一节](./307.md)中已经对安全问题进行了描述，另外一个内容是不能忽略的，那就是用户登录之后，对当前用户状态（用户是否登录）进行判断。

## 用户验证

用户登录之后，当翻到别的目录中时，往往需要验证用户是否处于登录状态。当然，一种比较直接的方法，就是在转到每个目录时，都从 cookie 中把用户信息，然后传到后端，跟数据库验证。这不仅是直接的，也是基本的流程。但是，这个过程如果总让用户自己来做，框架的作用就显不出来了。tornado 就提供了一种用户验证方法。

为了后面更工程化地使用 tornado 编程。需要将前面的已经有的代码进行重新梳理。我只是将有修改的文件代码写出来，不做过多解释，必要的有注释，相信读者在前述学习基础上，能够理解。

在 handler 目录中增加一个文件，名称是 base.py，代码如下：

    #! /usr/bin/env python
    # coding=utf-8

    import tornado.web

    class BaseHandler(tornado.web.RequestHandler):
        def get_current_user(self):
            return self.get_secure_cookie("user")

在这个文件中，目前只做一个事情，就是建立一个名为 BaseHandler 的类，然后在里面放置一个方法，就是得到当前的 cookie。在这里特别要向读者说明，在这个类中，其实还可以写不少别的东西，比如你就可以将数据库连接写到这个类的初始化`__init__()`方法中。因为在其它的类中，我们要继承这个类。所以，这样一个架势，就为读者以后的扩展增加了冗余空间。

然后把 index.py 文件改写为：

    #!/usr/bin/env Python
    # coding=utf-8

    import tornado.escape
    import methods.readdb as mrd
    from base import BaseHandler

    class IndexHandler(BaseHandler):    #继承 base.py 中的类 BaseHandler
        def get(self):
        usernames = mrd.select_columns(table="users",column="username")
        one_user = usernames[0][0]
        self.render("index.html", user=one_user)

        def post(self):
            username = self.get_argument("username")
            password = self.get_argument("password")
            user_infos = mrd.select_table(table="users",column="*",condition="username",value=username)
            if user_infos:
                db_pwd = user_infos[0][2]
                if db_pwd == password:
                    self.set_current_user(username)    #将当前用户名写入 cookie，方法见下面
                    self.write(username)
                else:
                    self.write("-1")
            else:
                self.write("-1")

        def set_current_user(self, user):
            if user:
                self.set_secure_cookie('user', tornado.escape.json_encode(user))    #注意这里使用了 tornado.escape.json_encode() 方法
            else:
                self.clear_cookie("user")

    class ErrorHandler(BaseHandler):    #增加了一个专门用来显示错误的页面
        def get(self):                                        #但是后面不单独讲述，读者可以从源码中理解
            self.render("error.html")

在 index.py 的类 IndexHandler 中，继承了 BaseHandler 类，并且增加了一个方法 set_current_user() 用于将用户名写入 cookie。请读者特别注意那个 tornado.escape.json_encode() 方法，其功能是：

>tornado.escape.json_encode(value)
>        JSON-encodes the given Python object.

如果要查看源码，可以阅读：[http://www.tornadoweb.org/en/branch2.3/escape.html](http://www.tornadoweb.org/en/branch2.3/escape.html)

这样做的本质是把 user 转化为 json，写入到了 cookie 中。如果从 cookie 中把它读出来，使用 user 的值时，还会用到：

>tornado.escape.json_decode(value)
>        Returns Python objects for the given JSON string

它们与[json 模块中的 dump()、load()](./227.md)功能相仿。

接下来要对 user.py 文件也进行重写：

    #!/usr/bin/env Python
    # coding=utf-8

    import tornado.web
    import tornado.escape
    import methods.readdb as mrd
    from base import BaseHandler

    class UserHandler(BaseHandler):
        @tornado.web.authenticated
        def get(self):
            #username = self.get_argument("user")
            username = tornado.escape.json_decode(self.current_user)
            user_infos = mrd.select_table(table="users",column="*",condition="username",value=username)
            self.render("user.html", users = user_infos)

在 get() 方法前面添加 `@tornado.web.authenticated`，这是一个装饰器，它的作用就是完成 tornado 的认证功能，即能够得到当前合法用户。在原来的代码中，用 `username = self.get_argument("user")` 方法，从 url 中得到当前用户名，现在把它注释掉，改用 `self.current_user`，这是和前面的装饰器配合使用的，如果它的值为假，就根据 setting 中的设置，寻找 login_url 所指定的目录（请关注下面对 setting 的配置）。

由于在 index.py 文件的 set_current_user() 方法中，是将 user 值转化为 json 写入 cookie 的，这里就得用 `username = tornado.escape.json_decode(self.current_user)` 解码。得到的 username 值，可以被用于后一句中的数据库查询。

application.py 中的 setting 也要做相应修改：

    #!/usr/bin/env Python
    # coding=utf-8

    from url import url

    import tornado.web
    import os

    setting = dict(
        template_path = os.path.join(os.path.dirname(__file__), "templates"),
        static_path = os.path.join(os.path.dirname(__file__), "statics"),
        cookie_secret = "bZJc2sWbQLKos6GkHn/VB9oXwQt8S0R0kRvJ5/xJ89E=",
        xsrf_cookies = True,
        login_url = '/',
    )
    
    application = tornado.web.Application(
        handlers = url,
        **setting
    )

与以前代码的重要区别在于 `login_url = '/',`，如果用户不合法，根据这个设置，会返回到首页。当然，如果有单独的登录界面，比如是 `/login`，也可以 `login_url = '/login'`。

如此完成的是用户登录到网站之后，在页面转换的时候实现用户认证。

为了演示本节的效果，我对教程的源码进行修改。读者在阅读的时候，可以参照源码。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：用 tornado 做网站 (5)](./307.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：用 tornado 做网站 (7)](./309.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。