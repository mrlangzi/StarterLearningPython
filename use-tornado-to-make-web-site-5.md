# 用 tornado 做网站 (5)

## 模板继承

用前面的方法，已经能够很顺利地编写模板了。读者如果留心一下，会觉得每个模板都有相同的部分内容。在 Python 中，有一种被称之为“继承”的机制（请阅读本教程第贰季第肆章中的[类 (4)(./209.md)中有关“继承”讲述]），它的作用之一就是能够让代码重用。

在 tornado 的模板中，也能这样。

先建立一个文件，命名为 base.html，代码如下：

    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>Learning Python</title>
    </head>
    <body>
        <header>
            {% block header %}{% end %}
        </header>
        <content>
            {% block body %}{% end %}
        </content>
        <footer>
            {% set website = "<a href='http://www.itdiffer.com'>welcome to my website</a>" %}
            {% raw website %}
        </footer>
        <script src="{{static_url("js/jquery.min.js")}}"></script>
        <script src="{{static_url("js/script.js")}}"></script>
    </body>
    </html>
    
接下来就以 base.html 为父模板，依次改写已经有的 index.html 和 user.html 模板。

index.html 代码如下：

    {% extends "base.html" %}

    {% block header %}
        <h2>登录页面</h2>
        <p>用用户名为：{{user}}登录</p> 
    {% end %}
    {% block body %}
        <form method="POST">
            <p><span>UserName:</span><input type="text" id="username"/></p>
            <p><span>Password:</span><input type="password" id="password" /></p>
            <p><input type="BUTTON" value="登录" id="login" /></p>
        </form>
    {% end %}

user.html 的代码如下：

    {% extends "base.html" %}

    {% block header %}
        <h2>Your informations are:</h2>
    {% end %}

    {% block body %}
        <ul>
            {% for one in users %}
                <li>username:{{one[1]}}</li>
                <li>password:{{one[2]}}</li>
                <li>email:{{one[3]}}</li>
            {% end %}
        </ul>
    {% end %}
    
看以上代码，已经没有以前重复的部分了。`{% extends "base.html" %}`意味着以 base.html 为父模板。在 base.html 中规定了形式如同`{% block header %}{% end %}`这样的块语句。在 index.html 和 user.html 中，分别对块语句中的内容进行了重写（或者说填充）。这就相当于在 base.html 中做了一个结构，在子模板中按照这个结构填内容。

## CSS

基本上的流程已经差不多了，如果要美化前端，还需要使用 css，它的使用方法跟 js 类似，也是在静态目录中建立文件即可。然后把下面这句加入到 base.html 的 `<head></head>` 中：

     <link rel="stylesheet" type="text/css" href="{{static_url("css/style.css")}}">
     
当然，要在 style.css 中写一个样式，比如：

    body {
        color:red;
    }

然后看看前端显示什么样子了，我这里是这样的：

![](./3images/30701.png)

关注字体颜色。

至于其它关于 CSS 方面的内容，本教程就不重点讲解了。读者可以参考关于 CSS 的资料。

至此，一个简单的基于 tornado 的网站就做好了，虽然它很丑，但是它很有前途。因为读者只要按照上述的讨论，可以在里面增加各种自己认为可以增加的内容。

建议读者在上述学习基础上，可以继续完成下面的几个功能：

- 用户注册
- 用户发表文章
- 用户文章列表，并根据文章标题查看文章内容
- 用户重新编辑文章

在后续教程内容中，也会涉及到上述功能。

## cookie 和安全

cookie 是现在网站重要的内容，特别是当有用户登录的时候。所以，要了解 cookie。维基百科如是说：

>Cookie（复数形態 Cookies），中文名稱為小型文字檔案或小甜餅，指某些网站为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。定義於 RFC2109。是网景公司的前雇员 Lou Montulli 在 1993 年 3 月的發明。

关于 cookie 的作用，维基百科已经说的非常详细了（读者还能正常访问这么伟大的网站吗？）：

>因为 HTTP 协议是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了交互式 Web 应用程序的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两瓶饮料。最后结帐时，由于 HTTP 的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么。 所以 Cookie 就是用来绕开 HTTP 的无状态性的“额外手段”之一。服务器可以设置或读取 Cookies 中包含信息，借此维护用户跟服务器会话中的状态。

>在刚才的购物场景中，当用户选购了第一项商品，服务器在向用户发送网页的同时，还发送了一段 Cookie，记录着那项商品的信息。当用户访问另一个页面，浏览器会把 Cookie 发送给服务器，于是服务器知道他之前选购了什么。用户继续选购饮料，服务器就在原来那段 Cookie 里追加新的商品信息。结帐时，服务器读取发送来的 Cookie 就行了。

>Cookie 另一个典型的应用是当登录一个网站时，网站往往会请求用户输入用户名和密码，并且用户可以勾选“下次自动登录”。如果勾选了，那么下次访问同一网站时，用户会发现没输入用户名和密码就已经登录了。这正是因为前一次登录时，服务器发送了包含登录凭据（用户名加密码的某种加密形式）的 Cookie 到用户的硬盘上。第二次登录时，（如果该 Cookie 尚未到期）浏览器会发送该 Cookie，服务器验证凭据，于是不必输入用户名和密码就让用户登录了。

和任何别的事物一样，cookie 也有缺陷，比如来自伟大的维基百科也列出了三条：

1. cookie 会被附加在每个 HTTP 请求中，所以无形中增加了流量。
2. 由于在 HTTP 请求中的 cookie 是明文传递的，所以安全性成问题。（除非用 HTTPS）
3. Cookie 的大小限制在 4KB 左右。对于复杂的存储需求来说是不够用的。

对于用户来讲，可以通过改变浏览器设置，来禁用 cookie，也可以删除历史的 cookie。但就目前而言，禁用 cookie 的可能不多了，因为她总要在网上买点东西吧。

Cookie 最让人担心的还是由于它存储了用户的个人信息，并且最终这些信息要发给服务器，那么它就会成为某些人的目标或者工具，比如有 cookie 盗贼，就是搜集用户 cookie，然后利用这些信息进入用户账号，达到个人的某种不可告人之目的；还有被称之为 cookie 投毒的说法，是利用客户端的 cookie 传给服务器的机会，修改传回去的值。这些行为常常是通过一种被称为“跨站指令脚本(Cross site scripting)”（或者跨站指令码）的行为方式实现的。伟大的维基百科这样解释了跨站脚本：

>跨网站脚本（Cross-site scripting，通常简称为 XSS 或跨站脚本或跨站脚本攻击）是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了 HTML 以及用户端脚本语言。

>XSS 攻击通常指的是通过利用网页开发时留下的漏洞，通过巧妙的方法注入恶意指令代码到网页，使用户加载并执行攻击者恶意制造的网页程序。这些恶意网页程序通常是 JavaScript，但实际上也可以包括 Java， VBScript， ActiveX， Flash 或者甚至是普通的 HTML。攻击成功后，攻击者可能得到更高的权限（如执行一些操作）、私密网页内容、会话和 cookie 等各种内容。

cookie 是好的，被普遍使用。在 tornado 中，也提供对 cookie 的读写函数。

`set_cookie()` 和 `get_cookie()` 是默认提供的两个方法，但是它是明文不加密传输的。

在 index.py 文件的 IndexHandler 类的 post() 方法中，当用户登录，验证用户名和密码后，将用户名和密码存入 cookie，代码如下：
    def post(self):
        username = self.get_argument("username")
        password = self.get_argument("password")
        user_infos = mrd.select_table(table="users",column="*",condition="username",value=username)
        if user_infos:
            db_pwd = user_infos[0][2]
            if db_pwd == password:
                self.set_cookie(username,db_pwd)    #设置 cookie
                self.write(username)
            else:
                self.write("your password was not right.")
        else:
            self.write("There is no thi user.")

上面代码中，较以前只增加了一句 `self.set_cookie(username,db_pwd)`，在回到登录页面，等候之后就成为：

![](./3images/30702.png)

看图中箭头所指，从左开始的第一个是用户名，第二个是存储的该用户密码。将我在登录是的密码就以明文的方式存储在 cookie 里面了。

明文存储，显然不安全。

tornado 提供另外一种安全的方法：set_secure_cookie() 和 get_secure_cookie()，称其为安全 cookie，是因为它以明文加密方式传输。此外，跟 set_cookie() 的区别还在于， set_secure_cookie() 执行后的 cookie 保存在磁盘中，直到它过期为止。也是因为这个原因，即使关闭浏览器，在失效时间之间，cookie 都一直存在。

要是用 set_secure_cookie() 方法设置 cookie，要先在 application.py 文件的 setting 中进行如下配置：

    setting = dict(
        template_path = os.path.join(os.path.dirname(__file__), "templates"),
        static_path = os.path.join(os.path.dirname(__file__), "statics"),
        cookie_secret = "bZJc2sWbQLKos6GkHn/VB9oXwQt8S0R0kRvJ5/xJ89E=",
        )

其中 `cookie_secret = "bZJc2sWbQLKos6GkHn/VB9oXwQt8S0R0kRvJ5/xJ89E="`是为此增加的，但是，它并不是这正的加密，仅仅是一个障眼法罢了。

因为 tornado 会将 cookie 值编码为 Base-64 字符串，并增加一个时间戳和一个 cookie 内容的 HMAC 签名。所以，cookie_secret 的值，常常用下面的方式生成（这是一个随机的字符串）：

    >>> import base64, uuid
    >>> base64.b64encode(uuid.uuid4().bytes)
    'w8yZud+kRHiP9uABEXaQiA=='

如果嫌弃上面的签名短，可以用 `base64.b64encode(uuid.uuid4().bytes + uuid.uuid4().bytes)` 获取。这里得到的是一个随机字符串，用它作为 cookie_secret 值。

然后修改 index.py 中设置 cookie 那句话，变成：

    self.set_secure_cookie(username,db_pwd)
    
从新跑一个，看看效果。

![](./3images/30703.png)

啊哈，果然“密”了很多。

如果要获取此 cookie，用 `self.get_secure_cookie(username)` 即可。

这是不是就安全了。如果这样就安全了，你太低估黑客们的技术实力了，甚至于用户自己也会修改 cookie 值。所以，还不安全。所以，又有了 httponly 和 secure 属性，用来防范 cookie 投毒。设置方法是：

    self.set_secure_cookie(username, db_pwd, httponly=True, secure=True)
    
要获取 cookie，可以使用 `self.set_secure_cookie(username)` 方法，将这句放在 user.py 中某个适合的位置，并且可以用 print 语句打印出结果，就能看到变量 username 对应的 cookie 了。这时候已经不是那个“密”过的，是明文显示。
    
用这样的方法，浏览器通过 SSL 连接传递 cookie，能够在一定程度上防范跨站脚本攻击。

## XSRF

XSRF 的含义是 Cross-site request forgery，即跨站请求伪造，也称之为"one click attack"，通常缩写成 CSRF 或者 XSRF，可以读作"sea surf"。这种对网站的攻击方式跟上面的跨站脚本（XSS）似乎相像，但攻击方式不一样。XSS 利用站点内的信任用户，而 XSRF 则通过伪装来自受信任用户的请求来利用受信任的网站。与 XSS 攻击相比，XSRF 攻击往往不大流行（因此对其 进行防范的资源也相当稀少）和难以防范，所以被认为比 XSS 更具危险性。

读者要详细了解 XSRF，推荐阅读：[CSRF | XSRF 跨站请求伪造](http://www.cnblogs.com/lsk/archive/2008/05/26/1207467.html)

对于防范 XSRF 的方法，上面推荐阅读的文章中有明确的描述。还有一点需要提醒读者，就是在开发应用时需要深谋远虑。任何会产生副作用的 HTTP 请求，比如点击购买按钮、编辑账户设置、改变密码或删除文档，都应该使用 post() 方法。这是良好的 RESTful 做法。

>又一个新名词：REST。这是一种 web 服务实现方案。伟大的维基百科中这样描述：

>表徵性狀態傳輸（英文：Representational State Transfer，简称 REST）是 Roy Fielding 博士在 2000  年他的博士论文中提出来的一种软件架构风格。目前在三种主流的 Web 服务实现方案中，因为 REST 模式与复杂的 SOAP 和 XML-RPC 相比更加简洁，越来越多的 web 服务开始采用 REST 风格设计和实现。例如，Amazon.com 提供接近 REST 风格的 Web 服务进行图书查找；雅虎提供的 Web 服务也是 REST 风格的。

>更详细的内容，读者可网上搜索来了解。

此外，在 tornado 中，还提供了 XSRF 保护的方法。

在 application.py 文件中，使用 xsrf_cookies 参数开启 XSRF 保护。

    setting = dict(
        template_path = os.path.join(os.path.dirname(__file__), "templates"),
        static_path = os.path.join(os.path.dirname(__file__), "statics"),
        cookie_secret = "bZJc2sWbQLKos6GkHn/VB9oXwQt8S0R0kRvJ5/xJ89E=",
        xsrf_cookies = True,
    )

这样设置之后，Tornado 将拒绝请求参数中不包含正确的`_xsrf` 值的 post/put/delete 请求。tornado 会在后面悄悄地处理`_xsrf` cookies，所以，在表单中也要包含 XSRF 令牌以却表请求合法。比如 index.html 的表单，修改如下：

    {% extends "base.html" %}

    {% block header %}
        <h2>登录页面</h2>
        <p>用用户名为：{{user}}登录</p> 
    {% end %}
    {% block body %}
        <form method="POST">
            {% raw xsrf_form_html() %}
            <p><span>UserName:</span><input type="text" id="username"/></p>
            <p><span>Password:</span><input type="password" id="password" /></p>
            <p><input type="BUTTON" value="登录" id="login" /></p>
        </form>
    {% end %}
    
 `{% raw xsrf_form_html() %}`是新增的，目的就在于实现上面所说的授权给前端以合法请求。
 
 前端向后端发送的请求是通过 ajax()，所以，在 ajax 请求中，需要一个 _xsrf 参数。
 
 以下是 script.js 的代码
 
     function getCookie(name){
        var x = document.cookie.match("\\b" + name + "=([^;]*)\\b");
        return x ? x[1]:undefined;
    }

    $(document).ready(function(){
        $("#login").click(function(){
            var user = $("#username").val();
            var pwd = $("#password").val();
            var pd = {"username":user, "password":pwd, "_xsrf":getCookie("_xsrf")};
            $.ajax({
                type:"post",
                url:"/",
                data:pd,
                cache:false,
                success:function(data){
                    window.location.href = "/user?user="+data;
                },
                error:function(){
                    alert("error!");
                },
            });
        });
    });

函数 getCookie() 的作用是得到 cookie 值，然后将这个值放到向后端 post 的数据中 `var pd = {"username":user, "password":pwd, "_xsrf":getCookie("_xsrf")};`。运行的结果：

![](./3images/30704.png)

这是 tornado 提供的 XSRF 防护方法。是不是这样做就高枕无忧了呢？<strong> 没这么简单。要做好一个网站，需要考虑的事情还很多 </strong>。特别推荐阅读[WebAppSec/Secure Coding Guidelines](https://wiki.mozilla.org/WebAppSec/Secure_Coding_Guidelines)

常常听到人说做个网站怎么怎么简单，客户用这种说辞来压低价格，老板用这种说辞来缩短工时成本，从上面的简单叙述中，你觉得网站还是随便几个页面就完事了吗？除非那个网站不是给人看的，是在那里摆着的。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：用 tornado 做网站 (4)](./306.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：用 tornado 做网站 (6)](./308.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。