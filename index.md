# This is for everyone.

>In the begning when God created the heavens and the earth. the earth was a formless void and darkness covered the face of the deep, while a wind from God swept over the face of the waters. Then God said,"Let there be light"; and there was light. And God saw that the light was good; and God separated the light from the darkness. (GENESIS 1:1-4)

#《零基础学 python》（第二版）

# 第一季 基础

## 第零章 预备

1. [关于 Python 的故事](./the-story-about-Python.md)
2. [从小工到专家](./from-the-laborer-to-the-experts.md.md)
3. [安装 Python 的开发环境](./Python-installation.md.md)
4. [集成开发环境](./integrated-development-environment.md)==>集成开发环境；Python 的 IDE

## 第一章 基本数据类型

1. [数和四则运算](./number-and-four-operations.md)==>整数和浮点数；变量；整数溢出问题；
2. [除法](./division.md)==>整数、浮点数相除；`from __future__ import division`；余数；四舍五入；
3. [常用数学函数和运算优先级](./common-mathematical-functions-and-operational-priorities.md)==>math模块，求绝对值，运算优先级
4. [写一个简单程序](./write-a-simple-program.md)==>程序和语句，注释
5. [字符串 (1)](./character-string-1.md)==>字符串定义，转义符，字符串拼接，str() 与 repr() 区别
6. [字符串 (2)](./character-string-2.md)==>raw_input,print，内建函数，原始字符串，再做一个小程序
7. [字符串 (3)](./character-string-3.md)==>字符串和序列，索引，切片，基本操作
8. [字符串 (4)](./character-string-4.md)==>字符串格式化，常用的字符串方法
9. [字符编码](./character-encoding.md)==>编码的基础知识，Python 中避免汉字乱码
10. [列表 (1)](./list-1.md)==>列表定义，索引和切片，列表反转，元素追加，基本操作
11. [列表 (2)](./list-2.md)==>列表 append/extend/index/count 方法，可迭代的和判断方法，列表原地修改
12. [列表 (3)](./list-3.md)==>列表 pop/remove/reverse/sort 方法
13. [回顾列表和字符串](./review-list-and-string.md)==>比较列表和字符串的相同点和不同点
14. [元组](./tuple.md)==>元组定义和基本操作，使用意义
15. [字典 (1)](./dictionaries-1.md)==>字典创建方法、基本操作（长度、读取值、删除值、判断键是否存在）
16. [字典 (2)](./dictionaries-2.md)==>字典方法:copy/deepcopy/clear/get/setdefault/items/iteritems/keys/iterkeys/values/itervalues/pop/popitem/update/has_key
17. [集合 (1)](./set-1.md)==>创建集合，集合方法：add/update,pop/remove/discard/clear，可哈希与不可哈希
18. [集合 (2)](./set-2.md)==>不可变集合，集合关系

## 第二章 语句和文件

1. [运算符](./operator.md)==>算数运算符，比较运算符，逻辑运算符/布尔类型
2. [语句 (1)](./sentence-1.md)==>print, import, 赋值语句、增量赋值
3. [语句 (2)](./sentence-2.md)==>if...elif...else 语句，三元操作
4. [语句 (3)](./sentence-3.md)==>for 循环，range()，循环字典
5. [语句 (4)](./sentence-4.md)==>并行迭代：zip()，enumerate()，list 解析
6. [语句 (5)](./sentence-5.md)==>while 循环，while...else，for...else
7. [文件 (1)](./file-1.md)==>文件打开，读取，写入
8. [文件 (2)](./file-2.md)==>文件状态，read/readline/readlines，大文件读取，seek
9. [迭代](./iterative.md)==>迭代含义，iter()
10. [练习](./practice.md)==>通过四个练习，综合运用以前所学
11. [自省](./introspection.md)==>自省概念，联机帮助，dir()，文档字符串，检查对象，文档

## 第三章 函数

1. [函数(1)](./function-1.md)==>定义函数方法，调用函数方法，命名方法，使用函数注意事项
2. [函数(2)](./function-2.md)==>函数返回值，函数文档，形参和实参，命名空间，全局变量和局部变量
3. [函数(3)](./function-3.md)==>收集参数:`*`和`**`，及其逆过程，复习参数知识
4. [函数(4)](./function-4.md)==>递归和 filter、map、reduce、lambda、yield
5. [函数练习](./function-exercise.md)==>解一元二次方程，统计考试成绩，找素数

# 第二季 进阶

## 第四章 类

1. [类(1)](./class-1.md)==>类的初步认识和基本概念理解：问题空间、对象、面向对象、类和实例化类
2. [类(2)](./class-2.md)==>新式类和旧式类，类的命名，构造函数，实例化及方法和属性，self 的作用
3. [类(3)](./class-3.md)==>类属性和实例属性，类内外数据流转，命名空间、作用域
4. [类(4)](./class-4.md)==>继承，多重继承，super 函数
5. [类(5)](./class-5.md)==>静态方法和类方法，两者的区别，类的文档
6. [多态和封装](./polymorphism-and-encapsulation.md)==>多态，封装和私有化
7. [特殊方法(1)](./special-method-1.md)==>`__dict__`和`__slots__`
8. [特殊方法(2)](./special-method-2.md)==>`__getattr__`,`__setattr__`以及查找属性顺序
9. [迭代器](./iterator.md)==>迭代器方法`__iter__`,`netx()`
10. [生成器](./generator.md)==>生成器定义，yield，生成器方法

## 第五章 错误和异常

1. [错误和异常(1)](./errors-and-exceptions-1.md)==>什么是错误和异常，常见异常类型，处理异常(try...except...)
2. [错误和异常(2)](./errors-and-exceptions-2.md)==>处理多个异常，else 子句，finally 子句
3. [错误和异常(3)](./errors-and-exceptions-3.md)==>assert 断言，异常小结

## 第六章 模块

1. [编写模块](./writing-module.md)==>模块是程序，模块的位置
2. [标准库(1)](./standard-library-1.md)==>引用模块的方式，dir() 查看属性和方法，模块文档和帮助
3. [标准库(2)](./standard-library-2.md)==>sys，copy
4. [标准库(3)](./standard-library-3.md)==>os 模块：操作文件、目录，查看修改属性，执行系统命令，打开网页
5. [标准库(4)](./standard-library-4.md)==>堆的基本知识，heapq 模块，deque 模块
6. [标准库(5)](./standard-library-5.md)==>calendar 模块、time 模块、datetime 模块
7. [标准库(6)](./standard-library-6.md)==>urllib 模块、urllib2 模块
8. [标准库(7)](./standard-library-7.md)==>xml.etree.ElementTree 模块：遍历查询、增删改查 xml，应用实例
9. [标准库(8)](./standard-library-8.md)==>json 模块：dumps(),loads(),dump(),load()，自定义类型数据的 json 编码和解码
10. [第三方库](./third-party-library.md)==>第三方库的模块安装方法，以 requests 模块为例说明

## 第七章 保存数据

1. [将数据存入文件](./put-data-into-a-file.md)==>pickle 模块，shelve 模块
2. [MySQL 数据库(1)](./MySQL-database-1.md)==>MySQL 概况，安装，Python 连接 MySQL 模块和方法
3. [MySQL 数据库(2)](./MySQL-database-2.md)==>连接对象方法，游标对象方法：数据库的增删改查基本操作
4. [MongoDB 数据库](./MongoDB-database.md)==>mongodb 的安装启动，pymongo 模块：连接客户端，数据库的增删改查操作
5. [SQLite 数据库](./SQLite-database.md)==>通过 sqlite3 模块操作 SQLite 数据库：连接对象方法，游标对象方法，数据库增删改查
6. [电子表格](./electronic-form.md)==>Python 操作 Excel 文件的第三方库 openpyxl 使用方法，以及其它与 Excel 相关的第三方库

# 第三季 实战

0. [引](./lead.md)

## 第八章 用 Tornado 做网站

1. [为做网站而准备](./prepare-for-the-web-site.md)==>开发框架，Python 的常用 web 框架，tornado 框架介绍和安装
2. [分析 Hello](./analysis-Hello.md)==>发布 tornado 做的网站，并剖析基本结构
3. [用 tornado 做网站(1)](./use-tornado-to-make-web-site-1.md)==>网站的基本结构，一个基于 tornado 框架的网站架子
4. [用 tornado 做网站(2)](./use-tornado-to-make-web-site-2.md)==>前端模板，静态文件引入
5. [用 tornado 做网站(3)](./use-tornado-to-make-web-site-3.md)==>ajax 传输数据，get_argument() 接收数据，验证用户名和密码
6. [用 tornado 做网站(4)](./use-tornado-to-make-web-site-4.md)==>render() 方法使用，模板语法，转义（自动转义，不转义）
7. [用 tornado 做网站(5)](./use-tornado-to-make-web-site-5.md)==>模板继承和块语句，CSS 文件，cookie 以及 XSRF 安全防护方法
8. [用 tornado 做网站(6)](./use-tornado-to-make-web-site-6.md)==>用户验证
9. [用 tornado 做网站(7)](./use-tornado-to-make-web-site-7.md)==>概念：同步和异步、阻塞和非阻塞，tornado 的同步，tornado 的异步设置，实践中的异步

## 第九章 科学计算

1. [为计算做准备](./prepare-for-calculation.md)==>相关模块的安装，ipython notebook
2. [Pandas使用(1)](./Pandas-appication-1.md)==>Series 数据类型：定义和基本属性，DataFrame 数据类型：定义和基本属性
3. [Pandas使用(2)](./Pandas-appication-2.md)==>读取 csv 文件，excel 文件，以及其它格式数据和从数据库加载数据方法
4. [处理股票数据](./handling-stock-data.md)==>下载 yahoo 股票数据，matplotlib 模块绘图

## 附：网络文摘

1. [如何成为 Python 高手](./how-to-become-a-Python-master.md)
2. [ASCII、Unicode、GBK 和 UTF-8 字符编码的区别联系](./the-difference.md)
