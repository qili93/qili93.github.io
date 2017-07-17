---
layout: post
title: （TBD）《A Byte of Python》阅读笔记
categories: 技术解读
tags: python
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

本文摘自[《A Byte of Python》中文版](https://bop.molun.net/)

### 基础

**数字**

数字主要分为两种类型——整数（Integers）与浮点数（Floats）。没有单独的 long 类型。int 类型可以指任何大小的整数。

- 有关整数的例子即 2，它只是一个整数。
- 有关浮点数（Floating Point Numbers，在英文中也会简写为 floats ）的例子是 3.23 或 52.3E-4。其中，E 表示 10 的幂。在这里，52.3E-4 表示 52.3 * 10^-4。

**单引号和双引号**

都可以用来制定字符串，所有引号内的空间，诸如空格与制表符，都将按原样保留。

**三引号**

你可以通过使用三个引号——""" 或 ''' 来指定多行字符串。你可以在三引号之间自由地使用单引号与双引号。来看看这个例子：

{% highlight python linenos %}
'''这是一段多行字符串。这是它的第一行。
This is the second line.
"What's your name?," I asked.
He said "Bond, James Bond."
'''
{% endhighlight %}

**字符串是不可变的**

这意味着一旦你创造了一串字符串，你就不能再改变它。尽管这看起来像是一件坏事，但实际上并非如此。我们将会在稍后展现的多个程序中看到为何这一点不是一个限制。

针对 C/C++ 程序员的提示: Python 中没有单独的 char 数据类型。它并非切实必要，并且我相信你不会想念它的。

**格式化方法**

Python 中 format 方法所做的事情便是将每个参数值替换至格式所在的位置：

{% highlight python linenos %}
# 字符串替换相应格式，Python从0开始计数
age = 20
name = 'Swaroop'
print('{0} was {1} years old when he wrote this book'.format(name, age))
print('Why is {0} playing with that python?'.format(name))

# 数字只是一个可选选项，所以你同样可以写成
age = 20
name = 'Swaroop'
print('{} was {} years old when he wrote this book'.format(name, age))
print('Why is {} playing with that python?'.format(name))

# 对于浮点数 '0.333' 保留小数点(.)后三位
print('{0:.3f}'.format(1.0/3))

# 使用下划线填充文本，并保持文字处于中间位置
# 使用 (^) 定义 '___hello___'字符串长度为 11
print('{0:_^11}'.format('hello'))

# 基于关键词输出 'Swaroop wrote A Byte of Python'
print('{name} wrote {book}'.format(name='Swaroop', book='A Byte of Python'))

# 注意 print 总是会以一个不可见的“新一行”字符（\n）结尾，因此重复调用 print将会在相互独立的一行中分别打印。
# 为防止打印过程中出现这一换行符，你可以通过 end 指定其应以空白结尾，输出结果为 "ab"
print('a', end='')
print('b', end='')

# 或者你通过 end 指定以空格结尾，输出结果为“a b c”
print('a', end=' ')
print('b', end=' ')
print('c')
{% endhighlight %}

**转义序列**
包含单引号字符串：
- 通过 \ 来指定单引号：要注意它可是反斜杠。现在，你可以将字符串指定为` 'What\'s your name?'`；
- 使用双引号：`"What's your name?"`。类似地， 你必须在使用双引号括起的字符串中对字符串内的双引号使用转义序列。同样，你必须使用转义序列 `\\ `来指定反斜杠本身。

双行字符串
- 一种方式即使用如前所述的三引号字符串
- 使用一个表示新一行的转义序列——\n 来表示新一行的开始 `'This is the first line\nThis is the second line'`，制表符为`\t`

一个放置在末尾的反斜杠表示字符串将在下一行继续，但不会添加新的一行

{% highlight python linenos %}
#  相当于"This is the first sentence. This is the second sentence."
"This is the first sentence. \
This is the second sentence."
{% endhighlight %}

**原始字符串**

在字符串前增加 r 或 R 来指定一个 原始（Raw） 字符串`r"Newlines are indicated by \n"`，例如反向引用可以通过 '\\1' 或 r'\1' 来实现。

**变量**

变量只需被赋予某一值。不需要声明或定义数据类型。

**逻辑行与物理行**

所谓物理行（Physical Line）是你在编写程序时 你所看到 的内容。所谓逻辑行（Logical Line）是 Python 所看到 的单个语句。Python 会假定每一 物理行 会对应一个 逻辑行。Python 之中暗含这样一种期望：Python 鼓励每一行使用一句独立语句从而使得代码更加可读。

如果你希望在一行物理行中指定多行逻辑行，那么你必须通过使用分号(;)来明确表明逻辑行或语句的结束，如`i = 5; print(i);`或者`i = 5; print(i)`。然而，强烈建议你对于**每一行物理行最多只写入一行逻辑行**。这个观点就是说你不应该使用分号。实际上，我从未在 Python 程序中使用、甚至是见过一个分号。

如果你有一行非常长的代码，你可以通过使用反斜杠将其拆分成多个物理行。这被称作**显式行连接（Explicit Line Joining）**

{% highlight python linenos %}
# 输出为This is a string. This continues the string.
s = 'This is a string. \
This continues the string.'
print(s)

# 等同于 i = 5
i = \
5
{% endhighlight %}

在某些情况下，会存在一个隐含的假设，允许你不使用反斜杠。这一情况即逻辑行以括号开始，它可以是方括号或花括号，但不能是结束括号。这被称作**隐式行连接（Implicit Line Joining）**。你可以在后面当我们讨论列表（List）的章节时了解这一点。

**缩进**

空白区在各行的开头非常重要。这被称作 缩进（Indentation）。在逻辑行的开头留下空白区（使用空格或制表符）用以确定各逻辑行的缩进级别，而后者又可用于确定语句的分组。这意味着放置在一起的语句必须拥有相同的缩进。每一组这样的语句被称为 块（block）。

有一件事你需要记住：错误的缩进可能会导致错误。下面是一个例子：

{% highlight python linenos %}
i = 5
# 下面将发生错误，注意行首有一个空格
 print('Value is', i)
print('I repeat, the value is', i)
{% endhighlight %}

当你运行这一程序时，你将得到如下错误：

{% highlight shell linenos %}
  File "whitespace.py", line 3
    print('Value is', i)
    ^
IndentationError: unexpected indent
# 缩进错误：意外缩进
{% endhighlight %}

>  **如何缩进**: 使用四个空格来缩进。这是来自 Python 语言官方的建议。好的编辑器会自动为你完成这一工作。请确保你在缩进中使用数量一致的空格，否则你的程序将不会运行，或引发不期望的行为。

> **针对静态编程语言程序员的提示**: Python 将始终对块使用缩进，并且绝不会使用大括号。你可以通过运行 from__future__import braces 来了解更多信息。

### 运算符与表达式

| 运算符         | 操作                                       | 举例                                       |
| ----------- | ---------------------------------------- | ---------------------------------------- |
| +（加）        | 两个对象相加。                                  | 3+5 则输出 8。'a' + 'b' 则输出 'ab'。            |
| -（减）        | 从一个数中减去另一个数，如果第一个操作数不存在，则假定为零。           | -5.2 将输出一个负数，50 - 24 输出 26。              |
| *（乘）        | 给出两个数的乘积，或返回字符串重复指定次数后的结果。               | 2 * 3 输出 6。'la' * 3 输出 'lalala'。         |
| ** （乘方）     | 返回 x 的 y 次方。                             | 3 ** 4 输出 81 （即 3 * 3 * 3 * 3）。          |
| / （除）       | x 除以 y                                   | 13 / 3 输出 4.333333333333333。             |
| // （整除）     | x 除以 y 并对结果向下取整至最接近的整数。                  | 13 // 3 输出 4。-13 // 3 输出 -5。             |
| % （取模）      | 返回除法运算后的余数。                              | 13 % 3 输出 1。-25.5 % 2.25 输出 1.5。         |
| << （左移）     | 将数字的位向左移动指定的位数。（每个数字在内存中以二进制数表示，即 0 和1）  | 2 << 2 输出 8。 2 用二进制数表示为 10。向左移 2 位会得到 1000 这一结果，表示十进制中的 8。 |
| >> （右移）     | 将数字的位向右移动指定的位数。                          | 11 >> 1 输出 5。11 在二进制中表示为 1011，右移一位后输出 101 这一结果，表示十进制中的 5。 |
| & （按位与）     | 对数字进行按位与操作。                              | 5 & 3 输出 1。                              |
| \| （按位或）    | 对数字进行按位或操作。                              | "5 \| 3 输出 7。"                           |
| ^（按位异或）     | 对数字进行按位异或操作。                             | 5 ^ 3 输出 6。                              |
| ~ （按位取反）    | x 的按位取反结果为 -(x+1)。                       | ~5 输出 -6。有关本例的更多细节可以参阅：http://stackoverflow.com/a/11810203。 |
| < （小于）      | 返回 x 是否小于 y。所有的比较运算符返回的结果均为 True 或 False。请注意这些名称之中的大写字母。 | 5 < 3 输出 False，3 < 6 输出 True。比较可以任意组成组成链接：3 < 5 < 7 返回 True。 |
| > （大于）      | 返回 x 是否大于 y。                             | 5 > 3 返回 True。如果两个操作数均为数字，它们首先将会被转换至一种共同的类型。否则，它将总是返回 False。 |
| <= （小于等于）   | 返回 x 是否小于或等于 y。                          | x = 3; y = 6; x<=y 返回 True。              |
| >= （大于等于）   | 返回 x 是否大于或等于 y。                          | x = 4; y = 3; x>=3 返回 True。              |
| == （等于）     | 比较两个对象是否相等。                              | x = 2; y = 2; x == y 返回 True。x = 'str'; y = 'stR'; x == y 返回 False。x = 'str'; y = 'str'; x == y 返回 True。 |
| != （不等于）    | 比较两个对象是否不相等。                             | x = 2; y = 3; x != y 返回 True。            |
| not （布尔“非”） | 如果 x 是 Ture，则返回 False。如果 x 是 False，则返回 True。 | x = Ture; not x 返回 False。                |
| and （布尔“与”） | 如果 x 是 False，则 x and y 返回 False，否则返回 y 的计算值。 | 当 x 是 False 时，x = False; y = True; x and y 将返回 False。 |
| or（布尔“或”）   | 如果 x 是 True，则返回 True，否则它将返回 y 的计算值。      | x = Ture; y = False; x or y 将返回 Ture。在这里短路计算同样适用。 |

### 控制流

**if 语句**

{% highlight python linenos %}
number = 23
guess = int(input('Enter an integer : '))

if guess == number:
    # 新块从这里开始
    print('Congratulations, you guessed it.')
    print('(but you do not win any prizes!)')
    # 新块在这里结束
elif guess < number:
    # 另一代码块
    print('No, it is a little higher than that')
    # 你可以在此做任何你希望在该代码块内进行的事情
else:
    print('No, it is a little lower than that')
    # 你必须通过猜测一个大于（>）设置数的数字来到达这里。

print('Done')
# 这最后一句语句将在
# if 语句执行完毕后执行。
{% endhighlight %}

> **针对 C/C++ 程序员的提示**: Python 中不存在 switch 语句。你可以通过使用 if..elif..else 语句来实现同样的事情（在某些情况下，使用一部字典能够更快速地完成）。

**while 语句**

你可以在 while 循环中使用 else 从句。

{% highlight python linenos %}
number = 23
running = True

while running:
    # 通过 input() 函数来获取用户的猜测数
    guess = int(input('Enter an integer : '))

    if guess == number:
        print('Congratulations, you guessed it.')
        # 这将导致 while 循环中止
        running = False
    elif guess < number:
        print('No, it is a little higher than that.')
    else:
        print('No, it is a little lower than that.')
else:
    print('The while loop is over.')
    # 在这里你可以做你想做的任何事

print('Done')
{% endhighlight %}

**for 循环**

{% highlight python linenos %}
# 通过内置的 range 函数生成这一数字序列
# range(1,5) 将输出序列 [1, 2, 3, 4]
for i in range(1, 5):
    print(i)
else:
    print('The for loop is over')
{% endhighlight %}

需要注意的是，`range()` 每次只会生成一个数字，如果你希望获得完整的数字列表，要在使用 `range() `时调用 `list()`。例如下面这样：`list(range(5))` ，它将会返回 `[0, 1, 2, 3, 4]`。

> **针对 C/C++/Java/C# 程序员的提示**：  
> Python 中的 for 循环和 C/C++ 中的 for 循环可以说是完全不同。C# 程序员会注意到 Python 中的 for 循环与 C# 中的 foreach 循环相似。Java 程序员则会注意到它同样与 Java 1.5 中的 for (int i : IntArray) 无甚区别。  
> 在 C/C++ 中，如果你希望编写 for (int i = 0; i < 5; i++)，那么在 Python 你只需要写下 for i in range(0,5)。正如你所看到的，Python 中的 for 循环将更加简单，更具表现力且更不容易出错。

**break 语句**

break 语句用以中断（Break）循环语句，也就是中止循环语句的执行，即使循环条件没有变更为 False，或队列中的项目尚未完全迭代依旧如此。

有一点需要尤其注意，如果你的 中断 了一个 for 或 while 循环，任何相应循环中的 else 块都将不会被执行。

{% highlight python linenos %}
while True:
    s = input('Enter something : ')
    if s == 'quit':
        break
    print('Length of the string is', len(s))
print('Done')
{% endhighlight %}

**continue 语句**

continue 语句用以告诉 Python 跳过当前循环块中的剩余语句，并继续该循环的下一次迭代。要注意 continue 语句同样能用于 for 循环。

{% highlight python linenos %}
while True:
    s = input('Enter something : ')
    if s == 'quit':
        break
    if len(s) < 3:
        print('Too small')
        continue
    print('Input is of sufficient length')
    # 自此处起继续进行其它任何处理
{% endhighlight %}

### 函数

内置函数，例如`len`和`range`。函数可以通过关键字 `def` 来定义。这一关键字后跟一个函数的标识符名称，再跟一对圆括号，其中可以包括一些变量的名称，再以冒号结尾，结束这一行。随后而来的语句块是函数的一部分。

{% highlight python linenos %}
def print_max(a, b):
    # 该块属于这一函数
    if a > b:
        print(a, 'is maximum')
    elif a == b:
        print(a, 'is equal to', b)
    else:
        print(b, 'is maximum')
# 函数结束

# 直接传递字面值
print_max(3, 4) # 调用函数

x = 5
y = 7

# 以参数的形式传递变量
print_max(x, y)
{% endhighlight %}

**局部变量**

当你在一个函数的定义中声明变量时，它们不会以任何方式与身处函数之外但具有相同名称的变量产生关系，也就是说，这些变量名只存在于函数这一局部（Local）。这被称为变量的作用域（Scope）。所有变量的作用域是它们被定义的块，从定义它们的名字的定义点开始。

{% highlight python linenos %}
x = 50

def func(x):
    print('x is', x)
    x = 2
    print('Changed local x to', x)

func(x)
print('x is still', x)
{% endhighlight %}

输出结果为：

{% highlight shell linenos %}
$ python function_local.py
x is 50
Changed local x to 2
x is still 50
{% endhighlight %}

**global 语句**

如果你想给一个在程序顶层的变量赋值（也就是说它不存在于任何作用域中，无论是函数还是类），那么你必须告诉 Python 这一变量并非局部的，而是全局（Global）的。我们需要通过 global 语句来完成这件事。因为在不使用 global 语句的情况下，不可能为一个定义于函数之外的变量赋值。

你可以使用定义于函数之外的变量的值（假设函数中没有具有相同名字的变量）。然而，这种方式不会受到鼓励而且应该避免，因为它对于程序的读者来说是含糊不清的，无法弄清楚变量的定义究竟在哪。而通过使用 global 语句便可清楚看出这一变量是在最外边的代码块中定义的。

{% highlight python linenos %}
x = 50

def func():
    global x

    print('x is', x)
    x = 2
    print('Changed global x to', x)

func()
print('Value of x is', x)
{% endhighlight %}

输出结果为：

{% highlight shell linenos %}
$ python function_global.py
x is 50
Changed global x to 2
Value of x is 2
{% endhighlight %}

`global` 语句用以声明 `x` 是一个全局变量——因此，当我们在函数中为 `x` 进行赋值时，这一改动将影响到我们在主代码块中使用的 `x` 的值。
你可以在同一句 `global` 语句中指定不止一个的全局变量，例如 `global x, y, z`。

**默认参数值**

对于一些函数来说，你可能为希望使一些参数可选并使用默认的值，以避免用户不想为他们提供值的情况。默认参数值可以有效帮助解决这一情况。你可以通过在函数定义时附加一个赋值运算符（=）来为参数指定默认参数值。要注意到，默认参数值应该是常数。更确切地说，默认参数值应该是不可变的。

{% highlight python linenos %}
def say(message, times=1):
    print(message * times)

say('Hello')
say('World', 5)
{% endhighlight %}

输出结果为：

{% highlight shell linenos %}
$ python function_default.py
Hello
WorldWorldWorldWorldWorld
{% endhighlight %}

> 注意  
> * 只有那些位于参数列表末尾的参数才能被赋予默认参数值，意即在函数的参数列表中拥有默认参数值的参数不能位于没有默认参数值的参数之前。  
> * 这是因为值是按参数所处的位置依次分配的。举例来说，def func(a, b=5) 是有效的，但 def func(a=5, b) 是无效的。

**关键字参数**

如果你有一些具有许多参数的函数，而你又希望只对其中的一些进行指定，那么你可以通过命名它们来给这些参数赋值——这就是关键字参数（Keyword Arguments）——我们使用命名（关键字）而非位置（一直以来我们所使用的方式）来指定函数中的参数。

这样做有两大优点——其一，我们不再需要考虑参数的顺序，函数的使用将更加容易。其二，我们可以只对那些我们希望赋予的参数以赋值，只要其它的参数都具有默认参数值。

{% highlight python linenos %}
def func(a, b=5, c=10):
    print('a is', a, 'and b is', b, 'and c is', c)

func(3, 7)
func(25, c=24)
func(c=50, a=100)
{% endhighlight %}

输出结果为：

{% highlight shell linenos %}
$ python function_keyword.py
a is 3 and b is 7 and c is 10
a is 25 and b is 5 and c is 24
a is 100 and b is 5 and c is 50
{% endhighlight %}

**可变参数**

有时你可能想定义的函数里面能够有任意数量的变量，也就是参数数量是可变的，这可以通过使用星号来实现：

{% highlight python linenos %}
def total(a=5, *numbers, **phonebook):
    print('a', a)

    #遍历元组中的所有项目
    for single_item in numbers:
        print('single_item', single_item)

    #遍历字典中的所有项目
    for first_part, second_part in phonebook.items():
        print(first_part,second_part)

print(total(10,1,2,3,Jack=1123,John=2231,Inge=1560))
{% endhighlight %}

输出结果为：

{% highlight shell linenos %}
$ python function_varargs.py
a 10
single_item 1
single_item 2
single_item 3
Inge 1560
John 2231
Jack 1123
None
{% endhighlight %}


当我们声明一个诸如 *param 的星号参数时，从此处开始直到结束的所有位置参数（Positional Arguments）都将被收集并汇集成一个称为“param”的元组（Tuple）。

类似地，当我们声明一个诸如 **param 的双星号参数时，从此处开始直至结束的所有关键字参数都将被收集并汇集成一个名为 param 的字典（Dictionary）。

**return 语句**

