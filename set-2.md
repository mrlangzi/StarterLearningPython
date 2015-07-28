# 集合(2)

## 不变的集合

[《集合(1)》](./118.md)中以 `set()`来建立集合，这种方式所创立的集合都是可原处修改的集合，或者说是可变的，也可以说是 unhashable

还有一种集合，不能在原处修改。这种集合的创建方法是用 `frozenset()`，顾名思义，这是一个被冻结的集合，当然是不能修改了，那么这种集合就是 hashable 类型——可哈希。

    >>> f_set = frozenset("qiwsir")
    >>> f_set
    frozenset(['q', 'i', 's', 'r', 'w'])
    >>> f_set.add("python")             #报错，不能修改，则无此方法
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'frozenset' object has no attribute 'add'
    
    >>> a_set = set("github")           #对比看一看，这是一个可以原处修改的 set
    >>> a_set
    set(['b', 'g', 'i', 'h', 'u', 't'])
    >>> a_set.add("python")
    >>> a_set
    set(['b', 'g', 'i', 'h', 'python', 'u', 't'])

## 集合运算

唤醒一下中学数学（准确说是高中数学中的一点知识）中关于集合的一点知识，当然，你如果是某个理工科的专业大学毕业，更应该熟悉集合之间的关系。

### 元素与集合的关系

就一种关系，要么术语某个集合，要么不属于。

    >>> aset
    set(['h', 'o', 'n', 'p', 't', 'y'])
    >>> "a" in aset
    False
    >>> "h" in aset
    True

### 集合与集合的关系

假设两个集合 A、B

- A 是否等于 B，即两个集合的元素完全一样

在交互模式下实验

    >>> a           
    set(['q', 'i', 's', 'r', 'w'])
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a == b
    False
    >>> a != b
    True

- A 是否是 B 的子集，或者反过来，B 是否是 A 的超集。即 A 的元素也都是 B 的元素，但是 B 的元素比 A 的元素数量多。

判断集合 A 是否是集合 B 的子集，可以使用 `A<B`，返回 true 则是子集，否则不是。另外，还可以使用函数 `A.issubset(B)`判断。

    >>> a
    set(['q', 'i', 's', 'r', 'w'])
    >>> c
    set(['q', 'i'])
    >>> c<a     #c 是 a 的子集
    True
    >>> c.issubset(a)   #或者用这种方法，判断 c 是否是 a 的子集
    True
    >>> a.issuperset(c) #判断 a 是否是 c 的超集
    True
    
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a<b     #a 不是 b 的子集
    False
    >>> a.issubset(b)   #或者这样做
    False

- A、B 的并集，即 A、B 所有元素，如下图所示

![](./1images/11901.png)

可以使用的符号是“|”，是一个半角状态写的竖线，输入方法是在英文状态下，按下"shift"加上右方括号右边的那个键。找找吧。表达式是 `A | B`.也可使用函数 `A.union(B)`，得到的结果就是两个集合并集，注意，这个结果是新生成的一个对象，不是将结合 A 扩充。

    >>> a
    set(['q', 'i', 's', 'r', 'w'])
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a | b                       #可以有两种方式，结果一样
    set(['a', 'i', 'l', 'o', 'q', 's', 'r', 'w'])
    >>> a.union(b)
    set(['a', 'i', 'l', 'o', 'q', 's', 'r', 'w'])

- A、B 的交集，即 A、B 所公有的元素，如下图所示

![](./1images/11902.png)

    >>> a
    set(['q', 'i', 's', 'r', 'w'])
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a & b       #两种方式，等价
    set(['q', 'i'])
    >>> a.intersection(b)
    set(['q', 'i'])

我在实验的时候，顺手敲了下面的代码，出现的结果如下，看官能解释一下吗？（思考题）

    >>> a and b
    set(['a', 'q', 'i', 'l', 'o'])

- A 相对 B 的差（补），即 A 相对 B 不同的部分元素，如下图所示

![](./1images/11903.png)

    >>> a
    set(['q', 'i', 's', 'r', 'w'])
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a - b
    set(['s', 'r', 'w'])
    >>> a.difference(b)
    set(['s', 'r', 'w'])

-A、B 的对称差集，如下图所示

![](./1images/11904.png)

    >>> a
    set(['q', 'i', 's', 'r', 'w'])
    >>> b
    set(['a', 'q', 'i', 'l', 'o'])
    >>> a.symmetric_difference(b)
    set(['a', 'l', 'o', 's', 'r', 'w'])

以上是集合的基本运算。在编程中，如果用到，可以用前面说的方法查找。不用死记硬背。

------

[总目录](./index.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[上节：集合(1)](./118.md)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[下节：运算符](./120.md)

如果你认为有必要打赏我，请通过支付宝：**qiwsir@126.com**,不胜感激。