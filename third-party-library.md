# 第三方库

标准库的内容已经非常多了，前面仅仅列举几个，但是 Python 给编程者的支持还不仅仅在于标准库，它还有不可胜数的第三方库。因此，如果作为一个 Python 编程者，即使你达到了 master 的水平，最好的还是要在做某个事情之前，在网上搜一下是否有标准库或者第三方库替你完成那件事。因为，伟大的艾萨克·牛顿爵士说过：

>如果我比别人看得更远，那是因为我站在巨人的肩上。

编程，就要站在巨人的肩上。标准库和第三方库以及其提供者，就是巨人，我们本应当谦卑地向其学习，并应用其成果。

## 安装第三方库

要是用第三方库，第一步就是要安装，在本地安装完毕，就能如同标准库一样使用了。其安装方法如下：

**方法一：利用源码安装**

在 github.com 网站可以下载第三方库的源码（或者其它途径），得到源码之后，在本地安装。

一般情况，得到的码格式大概都是 zip 、 tar.zip、 tar.bz2 格式的压缩包。解压这些包，进入其文件夹，通常会看见一个 setup.py 的文件。如果是 Linux 或者 Mac(我是用 ubuntu，特别推荐哦)，就在这里运行 shell，执行命令：

    Python setup.py install

如果用的是 windows，需要打开命令行模式，执行上述指令即可。

如此，就能把这个第三库安装到系统里。具体位置，要视操作系统和你当初安装 Python 环境时设置的路径而定。默认条件下,windows 是在 `C:\Python2.7\Lib\site-packages`，Linux 在 `/usr/local/lib/python2.7/dist-packages`（这个只是参考，不同发行版会有差别，具体请读者根据自己的操作系统，自己找找），Mac 在 `/Library/Python/2.7/site-packages`。

有安装就要有卸载，卸载所安装的库非常简单，只需要到相应系统的 site-packages 目录，直接删掉库文件即卸载。

**方法二：pip**

用源码安装，不是我推荐的，我推荐的是用第三方库的管理工具安装。有一个网站，是专门用来存储第三方库的，所有在这个网站上的，都能用 pip 或者 easy_install 这种安装工具来安装。这个网站的地址：<https://pypi.Python.org/pypi>

首先，要安装 pip（Python 官方推荐这个，我当然要顺势了，所以，就只介绍并且后面也只使用这个工具）。如果读者跟我一样，用的是 ubuntu 或者其它某种 Linux，基本不用这个操作，在安装操作系统的时候已经默认把这个东西安装好了（这还不是用 ubuntu 的理由吗？）。如果因为什么原因，没有安装，可以使用如下方法：

Debian and Ubuntu:

    sudo apt-get install Python-pip

Fedora and CentOS:

    sudo yum install python-pip

当然，也可以这里下载文件[get-pip.py](https://bootstrap.pypa.io/get-pip.py)，然后执行 `Python get-pip.py` 来安装。这个方法也适用于 windows。

pip 安装好了。如果要安装第三方库，只需要执行 `pip install XXXXXX`（XXXXXX 代表第三方库的名字）即可。

当第三方库安装完毕，接下来的使用就如同前面标准库一样。

## 举例：requests 库

以 requests 模块为例，来说明第三方库的安装和使用。之所以选这个，是因为前面介绍了 urllib 和 urllib2 两个标准库的模块，与之有类似功能的第三方库中 requests 也是一个用于在程序中进行 http 协议下的 get 和 post 请求的模块，并且被网友说成“好用的要哭”。

**说明**：下面的内容是网友 1world0x00 提供，我仅做了适当编辑。

### 安装

    pip install requests

安装好之后，在交互模式下：

    >>> import requests
    >>> dir(requests)
    ['ConnectionError', 'HTTPError', 'NullHandler', 'PreparedRequest', 'Request', 'RequestException', 'Response', 'Session', 'Timeout', 'TooManyRedirects', 'URLRequired', '__author__', '__build__', '__builtins__', '__copyright__', '__doc__', '__file__', '__license__', '__name__', '__package__', '__path__', '__title__', '__version__', 'adapters', 'api', 'auth', 'certs', 'codes', 'compat', 'cookies', 'delete', 'exceptions', 'get', 'head', 'hooks', 'logging', 'models', 'options', 'packages', 'patch', 'post', 'put', 'request', 'session', 'sessions', 'status_codes', 'structures', 'utils']

从上面的列表中可以看出，在 http 中常用到的 get，cookies，post 等都赫然在目。

### get 请求

    >>> r = requests.get("http://www.itdiffer.com")
    
得到一个请求的实例，然后：

    >>> r.cookies
    <<class 'requests.cookies.RequestsCookieJar'>[]>

这个网站对客户端没有写任何 cookies 内容。换一个看看：

    >>> r = requests.get("http://www.1world0x00.com")
    >>> r.cookies
    <<class 'requests.cookies.RequestsCookieJar'>[Cookie(version=0, name='PHPSESSID', value='buqj70k7f9rrg51emsvatveda2', port=None, port_specified=False, domain='www.1world0x00.com', domain_specified=False, domain_initial_dot=False, path='/', path_specified=True, secure=False, expires=None, discard=True, comment=None, comment_url=None, rest={}, rfc2109=False)]>

原来这样呀。继续，还有别的属性可以看看。

    >>> r.headers
    {'x-powered-by': 'PHP/5.3.3', 'transfer-encoding': 'chunked', 'set-cookie': 'PHPSESSID=buqj70k7f9rrg51emsvatveda2; path=/', 'expires': 'Thu, 19 Nov 1981 08:52:00 GMT', 'keep-alive': 'timeout=15, max=500', 'server': 'Apache/2.2.15 (CentOS)', 'connection': 'Keep-Alive', 'pragma': 'no-cache', 'cache-control': 'no-store, no-cache, must-revalidate, post-check=0, pre-check=0', 'date': 'Mon, 10 Nov 2014 01:39:03 GMT', 'content-type': 'text/html; charset=UTF-8', 'x-pingback': 'http://www.1world0x00.com/index.php/action/xmlrpc'}
     
    >>> r.encoding
    'UTF-8'
    
    >>> r.status_code
    200

下面这个比较长，是网页的内容，仅仅截取显示部分：

    >>> print r.text

    <!DOCTYPE html>
    <html lang="zh-CN">
      <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>1world0x00sec</title>
      <link rel="stylesheet" href="http://www.1world0x00.com/usr/themes/default/style.min.css">
      <link rel="canonical" href="http://www.1world0x00.com/" />
      <link rel="stylesheet" type="text/css" href="http://www.1world0x00.com/usr/plugins/CodeBox/css/codebox.css" />
      <meta name="description" content="爱生活，爱拉芳。不装逼还能做朋友。" />
      <meta name="keywords" content="php" />
      <link rel="pingback" href="http://www.1world0x00.com/index.php/action/xmlrpc" />

      ......

请求发出后，requests 会基于 http 头部对相应的编码做出有根据的推测，当你访问 r.text 之时，requests 会使用其推测的文本编码。你可以找出 requests 使用了什么编码，并且能够使用 r.coding 属性来改变它。 

    >>> r.content
    '\xef\xbb\xbf\xef\xbb\xbf<!DOCTYPE html>\n<html lang="zh-CN">\n  <head>\n    <meta charset="utf-8">\n    <meta name="viewport" content="width=device-width, initial-scale=1.0">\n    <title>1world0x00sec</title>\n    <link rel="stylesheet" href="http://www.1world0x00.com/usr/themes/default/style.min.css">\n            <link ......

    以二进制的方式打开服务器并返回数据。

### post 请求

requests 发送 post 请求，通常你会想要发送一些编码为表单的数据——非常像一个 html 表单。要实现这个，只需要简单地传递一个字典给 data 参数。你的数据字典在发出请求时会自动编码为表单形式。

    >>> import requests
    >>> payload = {"key1":"value1","key2":"value2"}
    >>> r = requests.post("http://httpbin.org/post")
    >>> r1 = requests.post("http://httpbin.org/post", data=payload)

r 没有加 data 的请求，看看效果：

![](http://wxpictures.qiniudn.com/requets-post1.jpg)

r1 是加了 data 的请求，看效果：

![](http://wxpictures.qiniudn.com/requets-post2.jpg)

多了 form 项。喵。

### http 头部

    >>> r.headers['content-type']
    'application/json'

注意，在引号里面的内容，不区分大小写`'CONTENT-TYPE'`也可以。

还能够自定义头部：

    >>> r.headers['content-type'] = 'adad'
    >>> r.headers['content-type']
    'adad'

注意，当定制头部的时候，如果需要定制的项目有很多，需要用到数据类型为字典。

网上有一个更为详细叙述有关 requests 模块的网页，可以参考：[http://requests-docs-cn.readthedocs.org/zh_CN/latest/index.html](http://requests-docs-cn.readthedocs.org/zh_CN/latest/index.html)

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：标准库(8)](./227.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：存入文件](./229.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。