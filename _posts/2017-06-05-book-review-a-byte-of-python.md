---
layout: post
title: 阅读笔记 --《A Byte of Python》
categories: 技术解读
tags: Python
date: 2017-06-05 17:03:47
---

{% include toc.html html=content sanitize=true class="inline_toc" id="my_toc" h_min=2 h_max=3 %}

本文摘自[《A Byte of Python》中文版](https://bop.molun.net/)

## 基础

### 数字

数字主要分为两种类型——整数（Integers）与浮点数（Floats）。没有单独的 long 类型。int 类型可以指任何大小的整数。

<!-- excerpt -->

- 有关整数的例子即 2，它只是一个整数。
- 有关浮点数（Floating Point Numbers，在英文中也会简写为 floats ）的例子是 3.23 或 52.3E-4。其中，E 表示 10 的幂。在这里，52.3E-4 表示 52.3 * 10^-4。

### 单引号和双引号

都可以用来制定字符串，所有引号内的空间，诸如空格与制表符，都将按原样保留。

### 三引号

你可以通过使用三个引号——""" 或 ''' 来指定多行字符串。你可以在三引号之间自由地使用单引号与双引号。来看看这个例子：

{% highlight python linenos %}
'''这是一段多行字符串。这是它的第一行。
This is the second line.
"What's your name?," I asked.
He said "Bond, James Bond."
'''
{% endhighlight %}

### 字符串是不可变的

这意味着一旦你创造了一串字符串，你就不能再改变它。尽管这看起来像是一件坏事，但实际上并非如此。我们将会在稍后展现的多个程序中看到为何这一点不是一个限制。

针对 C/C++ 程序员的提示: Python 中没有单独的 char 数据类型。它并非切实必要，并且我相信你不会想念它的。

### 格式化方法

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

### 转义序列
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

### 原始字符串

在字符串前增加 r 或 R 来指定一个 原始（Raw） 字符串`r"Newlines are indicated by \n"`，例如反向引用可以通过 '\\1' 或 r'\1' 来实现。

### 变量

变量只需被赋予某一值。不需要声明或定义数据类型。

### 逻辑行与物理行

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

### 缩进

空白区在各行的开头非常重要。这被称作 缩进（Indentation）。在逻辑行的开头留下空白区（使用空格或制表符）用以确定各逻辑行的缩进级别，而后者又可用于确定语句的分组。这意味着放置在一起的语句必须拥有相同的缩进。每一组这样的语句被称为 块（block）。

有一件事你需要记住：错误的缩进可能会导致错误。下面是一个例子：

{% highlight python linenos %}
i = 5
# 下面将发生错误，注意行首有一个空格
 print('Value is', i)
print('I repeat, the value is', i)
{% endhighlight %}

当你运行这一程序时，你将得到如下错误：

{% highlight python linenos %}
  File "whitespace.py", line 3
    print('Value is', i)
    ^
IndentationError: unexpected indent
# 缩进错误：意外缩进
{% endhighlight %}

>  **如何缩进**: 使用四个空格来缩进。这是来自 Python 语言官方的建议。好的编辑器会自动为你完成这一工作。请确保你在缩进中使用数量一致的空格，否则你的程序将不会运行，或引发不期望的行为。

> **针对静态编程语言程序员的提示**: Python 将始终对块使用缩进，并且绝不会使用大括号。你可以通过运行 from__future__import braces 来了解更多信息。

## 运算符与表达式

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
| &#124;（按位或） | 对数字进行按位或操作。                              | 5 &#124;  3 输出 7。                        |
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

## 控制流

### if 语句

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

### while 语句

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

### for 循环

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

### break 语句

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

### continue 语句

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

## 函数

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

### 局部变量

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

{% highlight python linenos %}
$ python function_local.py
x is 50
Changed local x to 2
x is still 50
{% endhighlight %}

### global 语句

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

{% highlight python linenos %}
$ python function_global.py
x is 50
Changed global x to 2
Value of x is 2
{% endhighlight %}

`global` 语句用以声明 `x` 是一个全局变量——因此，当我们在函数中为 `x` 进行赋值时，这一改动将影响到我们在主代码块中使用的 `x` 的值。
你可以在同一句 `global` 语句中指定不止一个的全局变量，例如 `global x, y, z`。

### 默认参数值

对于一些函数来说，你可能为希望使一些参数可选并使用默认的值，以避免用户不想为他们提供值的情况。默认参数值可以有效帮助解决这一情况。你可以通过在函数定义时附加一个赋值运算符（=）来为参数指定默认参数值。要注意到，默认参数值应该是常数。更确切地说，默认参数值应该是不可变的。

{% highlight python linenos %}
def say(message, times=1):
    print(message * times)

say('Hello')
say('World', 5)
{% endhighlight %}

输出结果为：

{% highlight python linenos %}
$ python function_default.py
Hello
WorldWorldWorldWorldWorld
{% endhighlight %}

> 注意  
> * 只有那些位于参数列表末尾的参数才能被赋予默认参数值，意即在函数的参数列表中拥有默认参数值的参数不能位于没有默认参数值的参数之前。  
> * 这是因为值是按参数所处的位置依次分配的。举例来说，def func(a, b=5) 是有效的，但 def func(a=5, b) 是无效的。

### 关键字参数

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

{% highlight python linenos %}
$ python function_keyword.py
a is 3 and b is 7 and c is 10
a is 25 and b is 5 and c is 24
a is 100 and b is 5 and c is 50
{% endhighlight %}

### 可变参数

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

{% highlight python linenos %}
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

### return 语句

`return` 语句用于从函数中返回，也就是中断函数。我们也可以选择在中断函数时从函数中返回一个值。+

{% highlight python linenos %}
def maximum(x, y):
    if x > y:
        return x
    elif x == y:
        return 'The numbers are equal'
    else:
        return y

print(maximum(2, 3))
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python function_return.py
3
{% endhighlight %}

要注意到如果 return 语句没有搭配任何一个值则代表着 返回 None。None 在 Python 中一个特殊的类型，代表着虚无。举个例子， 它用于指示一个变量没有值，如果有值则它的值便是 None（虚无）。

每一个函数都在其末尾隐含了一句 return None，除非你写了你自己的 return 语句。你可以运行 print(some_function())，其中 some_function 函数不使用 return 语句，就像这样：

{% highlight python linenos %}
def some_function():
    pass
{% endhighlight %}

Python 中的 pass 语句用于指示一个没有内容的语句块。

### DocStrings

Python 有一个甚是优美的功能称作文档字符串（Documentation Strings），在称呼它时通常会使用另一个短一些的名字docstrings。DocStrings 是一款你应当使用的重要工具，它能够帮助你更好地记录程序并让其更加易于理解。令人惊叹的是，当程序实际运行时，我们甚至可以通过一个函数来获取文档！

{% highlight python linenos %}
def print_max(x, y):
    '''Prints the maximum of two numbers.打印两个数值中的最大数。

    The two values must be integers.这两个数都应该是整数'''
    # 如果可能，将其转换至整数类型
    x = int(x)
    y = int(y)

    if x > y:
        print(x, 'is maximum')
    else:
        print(y, 'is maximum')

print_max(3, 5)
print(print_max.__doc__)
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python function_docstring.py
5 is maximum
Prints the maximum of two numbers.

    The two values must be integers.
{% endhighlight %}

函数的第一行逻辑行中的字符串是该函数的 文档字符串（DocString）。这里要注意文档字符串也适用于后面相关章节将提到的模块（Modules）与类（Class） 。

该文档字符串所约定的是一串多行字符串，其中第一行以某一大写字母开始，以句号结束。第二行为空行，后跟的第三行开始是任何详细的解释说明。5在此强烈建议你在你所有重要功能的所有文档字符串中都遵循这一约定。

我们可以通过使用函数的 `__doc__`（注意其中的双下划綫）属性（属于函数的名称）来获取函数 `print_max` 的文档字符串属性。只消记住 Python 将所有东西都视为一个对象，这其中自然包括函数。我们将在后面的类（Class）章节讨论有关对象的更多细节。

如果你曾使用过 Python 的 `help()` 函数，那么你应该已经了解了文档字符串的用途了。它所做的便是获取函数的 `__doc__ `属性并以一种整洁的方式将其呈现给你。你可以在上方的函数中尝试一下——只需在程序中包含 `help(print_max)` 就行了。要记住你可以通过按下 q 键来退出 help。

自动化工具可以以这种方式检索你的程序中的文档。因此，我强烈推荐你为你编写的所有重要的函数配以文档字符串。你的 Python 发行版中附带的 `pydoc `命令与 `help()` 使用文档字符串的方式类似。

## 模块

在上一章，你已经了解了如何在你的程序中通过定义一次函数工作来重用代码。那么如果你想在你所编写的别的程序中重用一些函数的话，应该怎么办？正如你可能想象到的那样，答案是模块（Modules）。

编写模块有很多种方法，其中最简单的一种便是创建一个包含函数与变量、以 .py 为后缀的文件。

另一种方法是使用撰写 Python 解释器本身的本地语言来编写模块。举例来说，你可以使用 C 语言来撰写 Python 模块，并且在编译后，你可以通过标准 Python 解释器在你的 Python 代码中使用它们。

一个模块可以被其它程序导入并运用其功能。我们在使用 Python 标准库的功能时也同样如此。首先，我们要了解如何使用标准库模块。

案例 (保存为 module_using_sys.py):

{% highlight python linenos %}
import sys

print('The command line arguments are:')
for i in sys.argv:
    print(i)

print('\n\nThe PYTHONPATH is', sys.path, '\n')
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python module_using_sys.py we are arguments
The command line arguments are:
module_using_sys.py
we
are
arguments


The PYTHONPATH is ['/tmp/py',
# many entries here, not shown here
'/Library/Python/2.7/site-packages',
'/usr/local/lib/python2.7/site-packages']
{% endhighlight %}

### 按字节码编译的 .pyc 文件

导入一个模块是一件代价高昂的事情，因此 Python 引入了一些技巧使其能够更快速的完成。其中一种方式便是创建按字节码编译的（Byte-Compiled）文件，这一文件以 `.pyc` 为其扩展名，是将 Python 转换成中间形式的文件（还记得《介绍》一章中介绍的 Python 是如何工作的吗？）。这一 `.pyc` 文件在你下一次从其它不同的程序导入模块时非常有用——它将更加快速，因为导入模块时所需要的一部分处理工作已经完成了。同时，这些按字节码编译的文件是独立于运行平台的。

注意：这些 `.pyc` 文件通常会创建在与对应的 `.py` 文件所处的目录中。如果 Python 没有相应的权限对这一目录进行写入文件的操作，那么 `.pyc` 文件将不会被创建。

**from..import 语句**

如果你希望直接将 `argv` 变量导入你的程序（为了避免每次都要输入 `sys.`），那么你可以通过使用 `from sys import argv` 语句来实现这一点。

> **警告**：一般来说，你应该尽量避免使用 `from...import` 语句，而去使用 `import` 语句。这是为了避免在你的程序中出现名称冲突，同时也为了使程序更加易读。  

案例：

{% highlight python linenos %}
from math import sqrt
print("Square root of 16 is", sqrt(16))
{% endhighlight %}

### 模块的 `__name__`

每个模块都有一个名称，而模块中的语句可以找到它们所处的模块的名称。这对于确定模块是独立运行的还是被导入进来运行的这一特定目的来说大为有用。正如先前所提到的，当模块第一次被导入时，它所包含的代码将被执行。我们可以通过这一特性来使模块以不同的方式运行，这取决于它是为自己所用还是从其它从的模块中导入而来。这可以通过使用模块的 `__name__ `属性来实现。

案例（保存为 module_using_name.py）：

{% highlight python linenos %}
if __name__ == '__main__':
    print('This program is being run by itself')
else:
    print('I am being imported from another module')
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python module_using_name.py
This program is being run by itself

$ python
>>> import module_using_name
I am being imported from another module
>>>
{% endhighlight %}

每一个 Python 模块都定义了它的` __name__ `属性。如果它与 `__main__ `属性相同则代表这一模块是由用户独立运行的，因此我们便可以采取适当的行动。

### 编写你自己的模块

编写你自己的模块很简单，这其实就是你一直在做的事情！这是因为每一个 Python 程序同时也是一个模块。你只需要保证它以 .py 为扩展名即可。下面的案例会作出清晰的解释。

案例（保存为 mymodule.py）：

{% highlight python linenos %}
def say_hi():
    print('Hi, this is mymodule speaking.')

__version__ = '0.1'
{% endhighlight %}

上方所呈现的就是一个简单的模块。正如你所看见的，与我们一般所使用的 Python 的程序相比其实并没有什么特殊的区别。我们接下来将看到如何在其它 Python 程序中使用这一模块。

要记住该模块应该放置于与其它我们即将导入这一模块的程序相同的目录下，或者是放置在 sys.path 所列出的其中一个目录下。

另一个模块（保存为 mymodule_demo.py）：

{% highlight python linenos %}
import mymodule

mymodule.say_hi()
print('Version', mymodule.__version__)
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python mymodule_demo.py
Hi, this is mymodule speaking.
Version 0.1
{% endhighlight %}

你会注意到我们使用相同的点符来访问模块中的成员。Python 很好地重用了其中的符号，这充满了“Pythonic”式的气息，这使得我们可以不必学习新的方式来完成同样的事情。

下面是一个使用 `from...import` 语法的范本（保存为 mymodule_demo2.py）：

{% highlight python linenos %}
from mymodule import say_hi, __version__

say_hi()
print('Version', __version__)
{% endhighlight %}

`mymodule_demo2.py` 所输出的内容与 `mymodule_demo.py` 所输出的内容是一样的。

在这里需要注意的是，如果导入到 `mymodule` 中的模块里已经存在了 `__version__` 这一名称，那将产生冲突。这可能是因为每个模块通常都会使用这一名称来声明它们各自的版本号。因此，我们大都推荐最好去使用 import 语句，尽管这会使你的程序变得稍微长一些。

你还可以使用：

{% highlight python linenos %}
from mymodule import *
{% endhighlight %}

这将导入诸如 `say_hi` 等所有公共名称，但不会导入 `__version__` 名称，因为后者以双下划线开头。

> 警告：要记住你应该避免使用 import-star 这种形式，即 `from mymodule import *`。  
> **Python 之禅**  
> Python 的一大指导原则是“明了胜过晦涩”2。你可以通过在 Python 中运行 `import this` 来了解更多内容。  

### dir 函数

内置的 `dir()` 函数能够返回由对象所定义的名称列表。 如果这一对象是一个模块，则该列表会包括函数内所定义的函数、类与变量。

该函数接受参数。 如果参数是模块名称，函数将返回这一指定模块的名称列表。 如果没有提供参数，函数将返回当前模块的名称列表。

案例：

{% highlight python linenos %}
$ python
>>> import sys

# 给出 sys 模块中的属性名称
>>> dir(sys)
['__displayhook__', '__doc__',
'argv', 'builtin_module_names',
'version', 'version_info']
# only few entries shown here

# 给出当前模块的属性名称
>>> dir()
['__builtins__', '__doc__',
'__name__', '__package__']

# 创建一个新的变量 'a'
>>> a = 5

>>> dir()
['__builtins__', '__doc__', '__name__', '__package__', 'a']

# 删除或移除一个名称
>>> del a

>>> dir()
['__builtins__', '__doc__', '__name__', '__package__']
{% endhighlight %}

首先我们看到的是 `dir` 在被导入的 `sys` 模块上的用法。我们能够看见它所包含的一个巨大的属性列表。

随后，我们以不传递参数的形式使用 `dir` 函数。在默认情况下，它将返回当前模块的属性列表。要注意到被导入进来的模块所能生成的列表也会是这一列表的一部分。

给了观察 `dir` 函数的操作，我们定义了一个新的变量 `a` 并为其赋予了一个值，然后在检查 `dir` 返回的结果，我们就能发现，同名列表中出现了一个新的值。我们通过 `del` 语句移除了一个变量或是属性，这一变化再次反映在 `dir` 函数所处的内容中。

关于 `del` 的一个小小提示——这一语句用于删除一个变量或名称，当这一语句运行后，在本例中即 `del a`，你便不再能访问变量 `a`——它将如同从未存在过一般。
要注意到 `dir()` 函数能对任何对象工作。例如运行 `dir(str)` 可以访问 `str（String，字符串）`类的属性。

同时，还有一个 `vars()` 函数也可以返回给你这些值的属性，但只是可能，它并不能针对所有类都能正常工作。

### 包

现在，你必须开始遵守用以组织你的程序的层次结构。变量通常位于函数内部，函数与全局变量通常位于模块内部。如果你希望组织起这些模块的话，应该怎么办？这便是包（Packages）应当登场的时刻。

包是指一个包含模块与一个特殊的 `__init__.py` 文件的文件夹，后者向 Python 表明这一文件夹是特别的，因为其包含了 Python 模块。

建设你想创建一个名为“world”的包，其中还包含着 ”asia“、”africa“等其它子包，同时这些子包都包含了诸如”india“、”madagascar“等模块。

下面是你会构建出的文件夹的结构：

{% highlight python linenos %}
- <some folder present in the sys.path>/
    - world/
        - __init__.py
        - asia/
            - __init__.py
            - india/
                - __init__.py
                - foo.py
        - africa/
            - __init__.py
            - madagascar/
                - __init__.py
                - bar.py
{% endhighlight %}

包是一种能够方便地分层组织模块的方式。你将在 标准库 中看到许多有关于此的实例。

如同函数是程序中的可重用部分那般，模块是一种可重用的程序。包是用以组织模块的另一种层次结构。Python 所附带的标准库就是这样一组有关包与模块的例子。

## 数据结构

Python 中有四种内置的数据结构——列表（List）、元组（Tuple）、字典（Dictionary）和集合（Set）

**列表（List）**

项目的列表应该用方括号括起来，这样 Python 才能理解到你正在指定一张列表。一旦你创建了一张列表，你可以添加、移除或搜索列表中的项目。既然我们可以添加或删除项目，我们会说列表是一种可变的（Mutable）数据类型，意即，这种类型是可以被改变的。

Python 为 `list` 类提供了一种 `append` 方法，能够允许你向列表末尾添加一个项目。例如 `mylist.append('an item')` 将会向列表 `mylist` 添加一串字符串。

{% highlight python linenos %}
# This is my shopping list
shoplist = ['apple', 'mango', 'carrot', 'banana']

print('I have', len(shoplist), 'items to purchase.')

print('These items are:', end=' ')
for item in shoplist:
    print(item, end=' ')

print('\nI also have to buy rice.')
shoplist.append('rice')
print('My shopping list is now', shoplist)

print('I will sort my list now')
shoplist.sort()
print('Sorted shopping list is', shoplist)

print('The first item I will buy is', shoplist[0])
olditem = shoplist[0]
del shoplist[0]
print('I bought the', olditem)
print('My shopping list is now', shoplist)
{% endhighlight %}

输出

{% highlight python linenos %}
$ python ds_using_list.py
I have 4 items to purchase.
These items are: apple mango carrot banana
I also have to buy rice.
My shopping list is now ['apple', 'mango', 'carrot', 'banana', 'rice']
I will sort my list now
Sorted shopping list is ['apple', 'banana', 'carrot', 'mango', 'rice']
The first item I will buy is apple
I bought the apple
My shopping list is now ['banana', 'carrot', 'mango', 'rice']
{% endhighlight %}

如果你想了解列表对象定义的所有方法，可以通过 help(list) 来了解更多细节。


### 元组（Tuple）

元组（Tuple）用于将多个对象保存到一起。你可以将它们近似地看作列表，元组的一大特征类似于字符串，它们是不可变的，也就是说，你不能编辑或更改元组。

元组是通过特别指定项目来定义的，在指定项目时，你可以给它们加上括号，并在括号内部用逗号进行分隔。

元组通常用于保证某一语句或某一用户定义的函数可以安全地采用一组数值，意即元组内的数值不会改变。

{% highlight python linenos %}
# 我会推荐你总是使用括号
# 来指明元组的开始与结束
# 尽管括号是一个可选选项。
# 明了胜过晦涩，显式优于隐式。
zoo = ('python', 'elephant', 'penguin')
print('Number of animals in the zoo is', len(zoo))

new_zoo = 'monkey', 'camel', zoo
print('Number of cages in the new zoo is', len(new_zoo))
print('All animals in new zoo are', new_zoo)
print('Animals brought from old zoo are', new_zoo[2])
print('Last animal brought from old zoo is', new_zoo[2][2])
print('Number of animals in the new zoo is',
      len(new_zoo)-1+len(new_zoo[2]))
{% endhighlight %}

输出

{% highlight python linenos %}
$ python ds_using_tuple.py
Number of animals in the zoo is 3
Number of cages in the new zoo is 3
All animals in new zoo are ('monkey', 'camel', ('python', 'elephant', 'penguin'))
Animals brought from old zoo are ('python', 'elephant', 'penguin')
Last animal brought from old zoo is penguin
Number of animals in the new zoo is 5
{% endhighlight %}

> **包含 0 或 1 个项目的元组**  
> 一个空的元组由一对圆括号构成，就像 `myempty = ()` 这样。  
> 然而，一个只拥有一个项目的元组并不像这样简单。你必须在第一个（也是唯一一个）项目的后面加上一个逗号来指定它，如此一来 Python 才可以识别出在这个表达式想表达的究竟是一个元组还是只是一个被括号所环绕的对象，也就是说，如果你想指定一个包含项目 2 的元组，你必须指定 `singleton = (2, )`。


### 字典（Dictionary）

字典就像一本地址簿，如果你知道了他或她的姓名，你就可以在这里找到其地址或是能够联系上对方的更多详细信息，换言之，我们将键值（Keys）（即姓名）与值（Values）（即地址等详细信息）联立到一起，在这里要注意到键值必须是唯一的。

另外要注意的是你只能使用不可变的对象（如字符串）作为字典的键值，但是你可以使用可变或不可变的对象作为字典中的值。基本上这段话也可以翻译为你只能使用简单对象作为键值。

在字典中，你可以通过使用符号构成 `d = {key : value1 , key2 : value2}` 这样的形式，来成对地指定键值与值。在这里要注意到成对的键值与值之间使用冒号分隔，而每一对键值与值则使用逗号进行区分，它们全都由一对花括号括起。

字典中的成对的键值—值配对不会以任何方式进行排序。如果你希望为它们安排一个特别的次序，只能在使用它们之前自行进行排序。

{% highlight python linenos %}
# “ab”是地址（Address）簿（Book）的缩写

ab = {
    'Swaroop': 'swaroop@swaroopch.com',
    'Larry': 'larry@wall.org',
    'Matsumoto': 'matz@ruby-lang.org',
    'Spammer': 'spammer@hotmail.com'
}

print("Swaroop's address is", ab['Swaroop'])

# 删除一对键值—值配对
del ab['Spammer']

print('\nThere are {} contacts in the address-book\n'.format(len(ab)))

for name, address in ab.items():
    print('Contact {} at {}'.format(name, address))

# 添加一对键值—值配对
ab['Guido'] = 'guido@python.org'

if 'Guido' in ab:
    print("\nGuido's address is", ab['Guido'])
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python ds_using_dict.py
Swaroop's address is swaroop@swaroopch.com

There are 3 contacts in the address-book

Contact Swaroop at swaroop@swaroopch.com
Contact Matsumoto at matz@ruby-lang.org
Contact Larry at larry@wall.org

Guido's address is guido@python.org
{% endhighlight %}

我们可以使用 in 运算符来检查某对键值—值配对是否存在。要想了解有关 dict 类的更多方法，请参阅 help(dict)。

### 如何进行字典排序

python dict按照key 排序：

{% highlight python linenos %}
items = dict.items()
items.sort()
for key,value in items:
   print key, value # print key,dict[key]
{% endhighlight %}

或者

{% highlight python linenos %}
print key, dict[key] for key in sorted(dict.keys())
{% endhighlight %}

python dict按照value排序：

方法一：把dictionary中的元素分离出来放到一个list中，对list排序，从而间接实现对dictionary的排序。这个“元素”可以是key，value或者item。

方法二：用lambda表达式来排序

{% highlight python linenos %}
#用lambda表达式来排序，更灵活：
sorted(dict.items(), lambda x, y: cmp(x[1], y[1]))
#降序
sorted(dict.items(), lambda x, y: cmp(x[1], y[1]), reverse=True)
{% endhighlight %}

下面给出python内置sorted函数的帮助文档：

{% highlight python linenos %}
sorted(...)
sorted(iterable, cmp=None, key=None, reverse=False) --> new sorted list
{% endhighlight %}

### 序列（Sequence）

列表、元组和字符串可以看作序列（Sequence）的某种表现形式，序列的主要功能是资格测试（Membership Test）（也就是 in 与 not in 表达式）和索引操作（Indexing Operations），它们能够允许我们直接获取序列中的特定项目。

上面所提到的序列的三种形态——列表、元组与字符串，同样拥有一种切片（Slicing）运算符，它能够允许我们序列中的某段切片——也就是序列之中的一部分。

{% highlight python linenos %}
shoplist = ['apple', 'mango', 'carrot', 'banana']
name = 'swaroop'

# Indexing or 'Subscription' operation #
# 索引或“下标（Subscription）”操作符 #
print('Item 0 is', shoplist[0])
print('Item 1 is', shoplist[1])
print('Item 2 is', shoplist[2])
print('Item 3 is', shoplist[3])
print('Item -1 is', shoplist[-1])
print('Item -2 is', shoplist[-2])
print('Character 0 is', name[0])

# Slicing on a list #
print('Item 1 to 3 is', shoplist[1:3])
print('Item 2 to end is', shoplist[2:])
print('Item 1 to -1 is', shoplist[1:-1])
print('Item start to end is', shoplist[:])

# 从某一字符串中切片 #
print('characters 1 to 3 is', name[1:3])
print('characters 2 to end is', name[2:])
print('characters 1 to -1 is', name[1:-1])
print('characters start to end is', name[:])
{% endhighlight %}

输出

{% highlight python linenos %}
$ python ds_seq.py
Item 0 is apple
Item 1 is mango
Item 2 is carrot
Item 3 is banana
Item -1 is banana
Item -2 is carrot
Character 0 is s
Item 1 to 3 is ['mango', 'carrot']
Item 2 to end is ['carrot', 'banana']
Item 1 to -1 is ['mango', 'carrot']
Item start to end is ['apple', 'mango', 'carrot', 'banana']
characters 1 to 3 is wa
characters 2 to end is aroop
characters 1 to -1 is waroo
characters start to end is swaroop
{% endhighlight %}

我们已经了解了如何通过使用索引来获取序列中的各个项目。这也被称作下标操作（Subscription Operation）。如上所示，每当你在方括号中为序列指定一个数字，Python 将获取序列中与该位置编号相对应的项目。要记得 Python 从 0 开始计数。因此 shoplist[0] 将获得 shoplist 序列中的第一个项目，而 shoplist[3] 将获得第四个项目。

索引操作也可以使用负数，在这种情况下，位置计数将从队列的末尾开始。因此，shoplist[-1] 指的是序列的最后一个项目，shoplist[-2] 将获取序列中倒数第二个项目。+

在切片操作中，第一个数字（冒号前面的那位）指的是切片开始的位置，第二个数字（冒号后面的那位）指的是切片结束的位置。如果第一位数字没有指定，Python 将会从序列的起始处开始操作。如果第二个数字留空，Python 将会在序列的末尾结束操作。要注意的是切片操作会在开始处返回 start，并在 end 前面的位置结束工作。也就是说，序列切片将包括起始位置，但不包括结束位置。

因此，`shoplist[1:3]` 返回的序列的一组切片将从位置 1 开始，包含位置 2 并在位置 3 时结束，因此，这块切片返回的是两个项目。类似地，`shoplist[:]` 返回的是整个序列。

你同样可以在切片操作中提供第三个参数，这一参数将被视为切片的步长（Step）（在默认情况下，步长大小为 1）：

{% highlight python linenos %}
>>> shoplist = ['apple', 'mango', 'carrot', 'banana']
>>> shoplist[::1]
['apple', 'mango', 'carrot', 'banana']
>>> shoplist[::2]
['apple', 'carrot']
>>> shoplist[::3]
['apple', 'banana']
>>> shoplist[::-1]
['banana', 'carrot', 'mango', 'apple']
{% endhighlight %}

### 集合（Set）

集合（Set）是简单对象的无序集合（Collection）。当集合中的项目存在与否比起次序或其出现次数更加重要时，我们就会使用集合。

通过使用集合，你可以测试某些对象的资格或情况，检查它们是否是其它集合的子集，找到两个集合的交集，等等。

{% highlight python linenos %}
>>> bri = set(['brazil', 'russia', 'india'])
>>> 'india' in bri
True
>>> 'usa' in bri
False
>>> bric = bri.copy()
>>> bric.add('china')
>>> bric.issuperset(bri)
True
>>> bri.remove('russia')
>>> bri & bric # OR bri.intersection(bric)
{'brazil', 'india'}
{% endhighlight %}


### 引用 (Reference)

当你创建了一个对象并将其分配给某个变量时，变量只会查阅（Refer）某个对象，并且它也不会代表对象本身。也就是说，变量名只是指向你计算机内存中存储了相应对象的那一部分。这叫作将名称绑定（Binding）给那一个对象。

一般来说，你不需要去关心这个，不过由于这一引用操作困难会产生某些微妙的效果，这是需要你注意的：

{% highlight python linenos %}
print('Simple Assignment')
shoplist = ['apple', 'mango', 'carrot', 'banana']
# mylist 只是指向同一对象的另一种名称
mylist = shoplist

# 我购买了第一项项目，所以我将其从列表中删除
del shoplist[0]

print('shoplist is', shoplist)
print('mylist is', mylist)
# 注意到 shoplist 和 mylist 二者都
# 打印出了其中都没有 apple 的同样的列表，以此我们确认
# 它们指向的是同一个对象

print('Copy by making a full slice')
# 通过生成一份完整的切片制作一份列表的副本
mylist = shoplist[:]
# 删除第一个项目
del mylist[0]

print('shoplist is', shoplist)
print('mylist is', mylist)
# 注意到现在两份列表已出现不同
{% endhighlight %}

输出

{% highlight python linenos %}
$ python ds_reference.py
Simple Assignment
shoplist is ['mango', 'carrot', 'banana']
mylist is ['mango', 'carrot', 'banana']
Copy by making a full slice
shoplist is ['mango', 'carrot', 'banana']
mylist is ['carrot', 'banana']
{% endhighlight %}

你要记住如果你希望创建一份诸如序列等复杂对象的副本（而非整数这种简单的对象（Object）），你必须使用切片操作来制作副本。如果你仅仅是将一个变量名赋予给另一个名称，那么它们都将“查阅”同一个对象，如果你对此不够小心，那么它将造成麻烦。

### 有关字符串的更多内容

字符串同样也是一种对象，并且它也具有自己的方法，可以做到检查字符串中的一部分或是去掉空格等几乎一切事情！

你在程序中使用的所有字符串都是 str 类下的对象。下面的案例将演示这种类之下一些有用的方法。要想获得这些方法的完成清单，你可以查阅 help(str)。

{% highlight python linenos %}
# 这是一个字符串对象
name = 'Swaroop'

if name.startswith('Swa'):
    print('Yes, the string starts with "Swa"')

if 'a' in name:
    print('Yes, it contains the string "a"')

if name.find('war') != -1:
    print('Yes, it contains the string "war"')

delimiter = '_*_'
mylist = ['Brazil', 'Russia', 'India', 'China']
print(delimiter.join(mylist))
{% endhighlight %}

输出

{% highlight python linenos %}
$ python ds_str_methods.py
Yes, the string starts with "Swa"
Yes, it contains the string "a"
Yes, it contains the string "war"
Brazil_*_Russia_*_India_*_China
{% endhighlight %}

`find` 方法用于定位字符串中给定的子字符串的位置。如果找不到相应的子字符串，`find` 会返回 -1。`str` 类同样还拥有一个简洁的方法用以 联结（Join）序列中的项目，其中字符串将会作为每一项目之间的分隔符，并以此生成并返回一串更大的字符串。

## 类与对象

类与对象是面向对象编程的两个主要方面。一个**类（Class）**能够创建一种新的**类型（Type）**，其中**对象（Object）**就是类的**实例（Instance）**。

对象可以使用属于它的普通变量来存储数据。这种从属于对象或类的变量叫作**字段（Field）**。对象还可以使用属于类的函数来实现某些功能，这种函数叫作类的**方法（Method）**。这两个术语很重要，它有助于我们区分函数与变量，哪些是独立的，哪些又是属于类或对象的。总之，字段与方法通称类的属性（Attribute）。

字段有两种类型——它们属于某一类的各个实例或对象，或是从属于某一类本身。它们被分别称作**实例变量（Instance Variables）**与**类变量（Class Variables）**。

### `self`

Python 中的 self 相当于 C++ 中的指针以及 Java 与 C# 中的 this 指针。

### `__init__ `方法

`__init__ `方法会在类的对象被实例化（Instantiated）时立即运行。这一方法可以对任何你想进行操作的目标对象进行初始化（Initialization）操作。这里你要注意在 init 前后加上的双下划线。

### 类变量与对象变量

**字段（Filed）**有两种类型——类变量与对象变量，它们根据究竟是类还是对象拥有这些变量来进行分类。

**类变量（Class Variable）**是共享的（Shared）——它们可以被属于该类的所有实例访问。该类变量只拥有一个副本，当任何一个对象对类变量作出改变时，发生的变动将在其它所有实例中都会得到体现。+

**对象变量（Object variable）**由类的每一个独立的对象或实例所拥有。在这种情况下，每个对象都拥有属于它自己的字段的副本，也就是说，它们不会被共享，也不会以任何方式与其它不同实例中的相同名称的字段产生关联。


{% highlight python linenos %}
#!/usr/bin/env python
# -*- coding: utf_8 -*-
# Date: 2016年10月10日
# Author:蔚蓝行

#首先创建一个类cls,这个类中包含一个值为1的类变量clsvar，一个值为2的实例变量insvar,
class cls:
    clsvar = 1
    def __init__(self):
        self.insvar = 2

#创建类的实例ins1和ins2
ins1 = cls()
ins2 = cls()

#用实例1为类变量重新赋值并打印
print '#'*10
ins1.clsvar = 20
print cls.clsvar     #输出结果为1
print ins1.clsvar    #输出结果为20
print ins2.clsvar    #输出结果为1

#用类名为类变量重新赋值并打印
print '#'*10
cls.clsvar = 10
print cls.clsvar     #输出结果为10
print ins1.clsvar    #输出结果为20
print ins2.clsvar    #输出结果为10

#这次直接给实例1没有在类中定义的变量赋值
print '#'*10
ins1.x = 11
print ins1.x         #输出结果为11

#然后再用类名给类中没有定义的变量赋值
print '#'*10
cls.m = 21
print cls.m          #输出结果为21

#再创建一个实例ins3，然后打印一下ins3的变量
print '#'*10
ins3 = cls()
print ins3.insvar    #输出结果为2
print ins3.clsvar    #输出结果为10
print ins3.m         #输出结果为21
print ins3.x         #报错AttributeError: cls instance has no attribute 'x'
{% endhighlight %}

看上去怪怪的，为什么会出现这种结果呢？这就要了解python中的__dict__属性了,__dict__是一个字典，键是属性名，值为属性值。

Python的实例有自己的__dict__，它对应的类也有自己的__dict__   （但是有些特殊的对象是没有__dict__属性的，这里不做讨论）

如果在程序的第15行处加上两句打印语句，打印类和实例1的__dict__属性，将会输出如下：

{% highlight python linenos %}
print cls.__dict__
print ins1.__dict__

########输出#######

{'clsvar': 1, '__module__': '__main__', '__doc__': None, '__init__': <function __init__ at 0x101bbc398>}
{'insvar': 2}
{% endhighlight %}

当打印类的__dict__属性时，列出了类cls所包含的属性，包括一些类内置属性和类变量clsvar以及构造方法__init__

而实例变量则包含在实例对象ins1的__dict__属性中，一个对象的属性查找顺序遵循首先查找实例对象自己，然后是类，接着是类的父类。

现在可以解释开头代码中的神秘现象了，再强调一遍，**一个对象的属性查找顺序遵循首先查找实例对象自己，然后是类，接着是类的父类**。

在第18行  ins1.clsvar = 20这句后面我们打印一下实例和类的__dict__属性

{% highlight python linenos %}
ins1.clsvar = 20
print ins1.__dict__
print cls.__dict__

########输出#######
{'insvar': 2, 'clsvar': 20}
{'clsvar': 1, '__module__': '__main__', '__doc__': None, '__init__': <function __init__ at 0x10c768398>}
{% endhighlight %}

可以看到，ins1.clsvar = 20这句只是在实例ins1的__dict__属性中增加了'clsvar': 20这一键值对，而类中的clsvar的值并没有改变，重要的事情说三遍：一个对象的属性查找顺序遵循首先查找实例对象自己，然后是类，接着是类的父类。当ins1在自己的__dict__中查找到了clsvar，就不会再向上查找，所以输出了值20。但是此时，cls类中的clsvar的值仍然为1。

但是当在第25行通过类名改变了类的clsvar之后，类的__dict__中的clsvar就被改变成10了，这时打印ins1的clsvar，由于之前第18行的原因,ins1在自己的__dict__中找到了clsvar，就输出了它自己的值20，而ins2自己的__dict__中没有clsvar,就向上查找类的__dict__，并找到了类的clsvar，值为10

第46行的ins3一直向上查找x属性都没有找到，就会抛出AttributeError

像32行和37行这样给类或实例设置属性，其实就是在他们各自的__dict__中添加了该属性，相信现在其他的神秘现象大家也可以自己解释了。



最后附上一个将字典转换成对象的小技巧，如果我们有一个字典如下:

{% highlight python linenos %}
bokeyuan={"b":1,
       "o":2,
       "k":3,
       "e":4,
       "y":5,
       "u":6,
       "a":7,
       "n":8,     
       }
{% endhighlight %}

现在想将其转换为一个对象，通常会这样写:

{% highlight python linenos %}
class Dict2Obj:
    def __init__(self,bokeyuan):
        self.b = bokeyuan['b']
        self.o = bokeyuan['o']
        self.k = bokeyuan['k']
        self.e = bokeyuan['e']
        self.y = bokeyuan['y']
        self.u = bokeyuan['u']
        self.a = bokeyuan['a']
        self.n = bokeyuan['n']
{% endhighlight %}

但是在了解了__dict__属性之后可以这样写：

{% highlight python linenos %}
class Dict2Obj:
    def __init__(self,bokeyuan):
        self.__dict__.update(bokeyuan)  
{% endhighlight %}

### 继承

面向对象编程的一大优点是对代码的重用（Reuse），重用的一种实现方法就是通过继承（Inheritance）机制。继承最好是想象成在类之间实现类型与子类型（Type and Subtype）关系的工具。

{% highlight python linenos %}
# coding=UTF-8

class SchoolMember:
    '''代表任何学校里的成员。'''
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print('(Initialized SchoolMember: {})'.format(self.name))

    def tell(self):
        '''告诉我有关我的细节。'''
        print('Name:"{}" Age:"{}"'.format(self.name, self.age), end=" ")


class Teacher(SchoolMember):
    '''代表一位老师。'''
    def __init__(self, name, age, salary):
        SchoolMember.__init__(self, name, age)
        self.salary = salary
        print('(Initialized Teacher: {})'.format(self.name))

    def tell(self):
        SchoolMember.tell(self)
        print('Salary: "{:d}"'.format(self.salary))


class Student(SchoolMember):
    '''代表一位学生。'''
    def __init__(self, name, age, marks):
        SchoolMember.__init__(self, name, age)
        self.marks = marks
        print('(Initialized Student: {})'.format(self.name))

    def tell(self):
        SchoolMember.tell(self)
        print('Marks: "{:d}"'.format(self.marks))

t = Teacher('Mrs. Shrividya', 40, 30000)
s = Student('Swaroop', 25, 75)

# 打印一行空白行
print()

members = [t, s]
for member in members:
    # 对全体师生工作
    member.tell()
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python oop_subclass.py
(Initialized SchoolMember: Mrs. Shrividya)
(Initialized Teacher: Mrs. Shrividya)
(Initialized SchoolMember: Swaroop)
(Initialized Student: Swaroop)

Name:"Mrs. Shrividya" Age:"40" Salary: "30000"
Name:"Swaroop" Age:"25" Marks: "75"
{% endhighlight %}

## 输入与输出

### 控制台输入输出

通过 `input()` 函数与 `print` 函数来实现这一需求。`input()` 函数可以接受一个字符串作为参数，并将其展示给用户。尔后它将等待用户输入内容或敲击返回键。一旦用户输入了某些内容并敲下返回键，`input()` 函数将返回用户输入的文本。

{% highlight python linenos %}
def reverse(text):
    return text[::-1]


def is_palindrome(text):
    return text == reverse(text)


something = input("Enter text: ")
if is_palindrome(something):
    print("Yes, it is a palindrome")
else:
    print("No, it is not a palindrome")
{% endhighlight %}

输出

{% highlight shell linenos %}
$ python3 io_input.py
Enter text: sir
No, it is not a palindrome

$ python3 io_input.py
Enter text: madam
Yes, it is a palindrome

$ python3 io_input.py
Enter text: racecar
Yes, it is a palindrome
{% endhighlight %}

### 文件

你可以通过创建一个属于 `file` 类的对象并适当使用它的 `read`、`readline`、`write` 方法来打开或使用文件，并对它们进行读取或写入。读取或写入文件的能力取决于你指定以何种方式打开文件。最后，当你完成了文件，你可以调用 `close` 方法来告诉 Python 我们已经完成了对该文件的使用。

首先，我们使用内置的 `open` 函数并指定文件名以及我们所希望使用的打开模式来打开一个文件。打开模式可以是阅读模式（`'r'`），写入模式（`'w'`）和追加模式（`'a'`）。我们还可以选择是通过文本模式（`'t'`）还是二进制模式（`'b'`）来读取、写入或追加文本。实际上还有其它更多的模式可用，`help(open)` 会给你有关它们的更多细节。在默认情况下，`open()` 会将文件视作文本（**t**ext）文件，并以阅读（**r**ead）模式打开它。

{% highlight python linenos %}
poem = '''\
Programming is fun
When the work is done
if you wanna make your work also fun:
    use Python!
'''

# 打开文件以编辑（'w'riting）
f = open('poem.txt', 'w')
# 向文件中编写文本
f.write(poem)
# 关闭文件
f.close()

# 如果没有特别指定，
# 将假定启用默认的阅读（'r'ead）模式
f = open('poem.txt')
while True:
    line = f.readline()
    # 零长度指示 EOF
    if len(line) == 0:
        break
    # 每行（`line`）的末尾
    # 都已经有了换行符
    #因为它是从一个文件中进行读取的
    print(line, end='')
# 关闭文件
f.close()
{% endhighlight %}

输出

{% highlight shell linenos %}
$ python3 io_using_file.py
Programming is fun
When the work is done
if you wanna make your work also fun:
    use Python!
{% endhighlight %}

### Pickle

Python 提供了一个叫作 `Pickle` 的标准模块，通过它你可以将*任何*纯 Python 对象存储到一个文件中，并在稍后将其取回。这叫作*持久地（Persistently）*存储对象。

要想将一个对象存储到一个文件中，我们首先需要通过 `open` 以写入（**w**rite）二进制（**b**inary）模式打开文件，然后调用 `pickle` 模块的 `dump` 函数。这一过程被称作*封装（Pickling）*。

接着，我们通过 `pickle` 模块的 `load` 函数接收返回的对象。这个过程被称作*拆封（Unpickling）*。

{% highlight python linenos %}
import pickle

# The name of the file where we will store the object
shoplistfile = 'shoplist.data'
# The list of things to buy
shoplist = ['apple', 'mango', 'carrot']

# Write to the file
f = open(shoplistfile, 'wb')
# Dump the object to a file
pickle.dump(shoplist, f)
f.close()

# Destroy the shoplist variable
del shoplist

# Read back from the storage
f = open(shoplistfile, 'rb')
# Load the object from the file
storedlist = pickle.load(f)
print(storedlist)
{% endhighlight %}

输出：

{% highlight shell linenos %}
$ python io_pickle.py
['apple', 'mango', 'carrot']
{% endhighlight %}

### Unicode

如果你正在使用 Python 2，我们又希望能够读写其它非英语语言，我们需要使用 `unicode` 类型，它全都以字母 `u` 开头，例如 `u"hello world"`。

{% highlight python linenos %}
>>> "hello world"
'hello world'
>>> type("hello world")
<class 'str'>
>>> u"hello world"
'hello world'
>>> type(u"hello world")
<class 'str'>
{% endhighlight %}

当我们阅读或写入某一文件或当我们希望与互联网上的其它计算机通信时，我们需要将我们的 Unicode 字符串转换至一个能够被发送和接收的格式，这个格式叫作“UTF-8”。我们可以在这一格式下进行读取与写入，只需使用一个简单的关键字参数到我们的标准 `open` 函数中：

{% highlight python linenos %}
# encoding=utf-8
import io

f = io.open("abc.txt", "wt", encoding="utf-8")
f.write(u"Imagine non-English language here")
f.close()

text = io.open("abc.txt", encoding="utf-8").read()
print(text)
{% endhighlight %}

每当我们诸如上面那番使用 Unicode 字面量编写一款程序时，我们必须确保 Python 程序已经被告知我们使用的是 UTF-8，因此我们必须将 `# encoding=utf-8` 这一注释放置在我们程序的顶端。

我们使用 `io.open` 并提供了“编码（Encoding）”与“解码（Decoding）”参数来告诉 Python 我们正在使用 Unicode。

## 异常和错误

### 错误

你可以想象一个简单的 `print` 函数调用。如果我们把 `print` 误拼成 `Print` 会怎样？你会注意到它的首字母是大写。在这一例子中，Python 会*抛出（Raise）*一个语法错误。

{% highlight python linenos %}
>>> Print("Hello World")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'Print' is not defined
>>> print("Hello World")
Hello World
{% endhighlight %}

你会注意到一个 `NameError` 错误被抛出，同时 Python 还会打印出检测到的错误发生的位置。这就是一个错误**错误处理器（Error Handler）**[2](https://bop.molun.net/16.exceptions.html#fn_2) 为这个错误所做的事情。

### 异常

我们将**尝试（Try）**去读取用户的输入内容。按下 `[ctrl-d]` 来看看会发生什么事情。

{% highlight python linenos %}
>>> s = input('Enter something --> ')
Enter something --> Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
EOFError
{% endhighlight %}

此处 Python 指出了一个称作 `EOFError` 的错误，代表着它发现了一个*文件结尾（End of File）*符号（由 `ctrl-d` 实现）在不该出现的时候出现了。

### 处理异常

我们可以通过使用 `try..except` 来处理异常状况。我们将所有可能引发异常或错误的语句放在 `try` 代码块中，并将相应的错误或异常的处理器（Handler）放在 `except` 子句或代码块中。`except` 子句可以处理某种特定的错误或异常，或者是一个在括号中列出的错误或异常。如果没有提供错误或异常的名称，它将处理*所有*错误与异常。

案例（保存文 `exceptions_handle.py`）：

{% highlight python linenos %}
try:
    text = input('Enter something --> ')
except EOFError:
    print('Why did you do an EOF on me?')
except KeyboardInterrupt:
    print('You cancelled the operation.')
else:
    print('You entered {}'.format(text))
{% endhighlight %}

输出：

{% highlight shell linenos %}
# Press ctrl + d
$ python exceptions_handle.py
Enter something --> Why did you do an EOF on me?

# Press ctrl + c
$ python exceptions_handle.py
Enter something --> ^CYou cancelled the operation.

$ python exceptions_handle.py
Enter something --> No exceptions
You entered No exceptions
{% endhighlight %}

### 抛出异常

你可以通过 `raise` 语句来*引发*一次异常，具体方法是提供错误名或异常名以及要*抛出（Thrown）*异常的对象。

你能够引发的错误或异常必须是直接或间接从属于 `Exception`（异常） 类的派生类。

案例（保存为 `exceptions_raise.py`）：

在本例中，我们创建了我们自己的异常类型。这一新的异常类型叫作 `ShortInputException`。它包含两个字段——获取给定输入文本长度的 `length`，程序期望的最小长度 `atleast`。

在 `except` 子句中，我们提及了错误类，将该类存储 `as（为）` 相应的错误名或异常名。这类似于函数调用中的形参与实参。在这个特殊的 `except` 子句中我们使用异常对象的 `length` 与 `atlease` 字段来向用户打印一条合适的信息。

{% highlight python linenos %}
# encoding=UTF-8

class ShortInputException(Exception):
    '''一个由用户定义的异常类'''
    def __init__(self, length, atleast):
        Exception.__init__(self)
        self.length = length
        self.atleast = atleast

try:
    text = input('Enter something --> ')
    if len(text) < 3:
        raise ShortInputException(len(text), 3)
    # 其他工作能在此处继续正常运行
except EOFError:
    print('Why did you do an EOF on me?')
except ShortInputException as ex:
    print(('ShortInputException: The input was ' +
           '{0} long, expected at least {1}')
          .format(ex.length, ex.atleast))
else:
    print('No exception was raised.')
{% endhighlight %}

输出：

{% highlight shell linenos %}
$ python exceptions_raise.py
Enter something --> a
ShortInputException: The input was 1 long, expected at least 3

$ python exceptions_raise.py
Enter something --> abc
No exception was raised.
{% endhighlight %}

### Try ... Finally

假设你正在你的读取中读取一份文件。你应该如何确保文件对象被正确关闭，无论是否会发生异常？这可以通过 `finally` 块来完成。

保存该程序为 `exceptions_finally.py`：

我们按照通常文件读取进行操作，但是我们同时通过使用 `time.sleep` 函数任意在每打印一行后插入两秒休眠，使得程序运行变得缓慢（在通常情况下 Python 运行得非常快速）。当程序在处在运行过过程中时，按下 `ctrl + c` 来中断或取消程序。

你会注意到 `KeyboardInterrupt` 异常被抛出，尔后程序退出。不过，在程序退出之前，finally 子句得到执行，文件对象总会被关闭。

另外要注意到我们在 `print` 之后使用了 `sys.stout.flush()`，以便它能被立即打印到屏幕上。

{% highlight python linenos %}
import sys
import time

f = None
try:
    f = open("poem.txt")
    # 我们常用的文件阅读风格
    while True:
        line = f.readline()
        if len(line) == 0:
            break
        print(line, end='')
        sys.stdout.flush()
        print("Press ctrl+c now")
        # 为了确保它能运行一段时间
        time.sleep(2)
except IOError:
    print("Could not find file poem.txt")
except KeyboardInterrupt:
    print("!! You cancelled the reading from the file.")
finally:
    if f:
        f.close()
    print("(Cleaning up: Closed the file)")
{% endhighlight %}

输出：

{% highlight shell linenos %}
$ python exceptions_finally.py
Programming is fun
Press ctrl+c now
^C!! You cancelled the reading from the file.
(Cleaning up: Closed the file)
{% endhighlight %}

### `with` 语句

在 `try` 块中获取资源，然后在 `finally` 块中释放资源是一种常见的模式。因此，还有一个 `with` 语句使得这一过程可以以一种干净的姿态得以完成。

保存为 `exceptions_using_with.py`：

{% highlight python linenos %}
with open("poem.txt") as f:
    for line in f:
        print(line, end='')
{% endhighlight %}

程序输出的内容应与上一个案例所呈现的相同。本例的不同之处在于我们使用的是 `open` 函数与 `with` 语句——我们将关闭文件的操作交由 `with open` 来自动完成。

在幕后发生的事情是有一项 `with` 语句所使用的协议（Protocol）。它会获取由 `open` 语句返回的对象，在本案例中就是“thefile”。

它*总会*在代码块开始之前调用 `thefile.__enter__` 函数，并且*总会*在代码块执行完毕之后调用 `thefile.__exit__`。

因此，我们在 `finally` 代码块中编写的代码应该格外留心 `__exit__` 方法的自动操作。这能够帮助我们避免重复显式使用 `try..finally` 语句。

## 标准库

你能在你的 Python 安装包中附带的文档中的[“库概览（Library Reference）” 部分](http://docs.python.org/3/library/)中查找到所有模块的全部细节。

### `sys` 模块

`sys` 模块包括了一些针对特定系统的功能。我们已经了解过 `sys.argv` 列表中包括了命令行参数。

想象一些我们需要检查正在使用的 Python 软件的版本，`sys` 模块会给我们相关的信息。

{% highlight python linenos %}
>>> import sys
>>> sys.version_info
sys.version_info(major=3, minor=5, micro=1, releaselevel='final', serial=0)
>>> sys.version_info.major == 3
True
{% endhighlight %}

**它是如何工作的**

`sys` 模块包含一个 `version_info` 元组，它提供给我们版本信息。第一个条目是主版本信息。我们可以调出这些信息并使用它。

### 日志模块

如果你想将一些调试（Debugging）信息或一些重要的信息储存在某个地方，以便你可以检查你的程序是否如你所期望那般运行，应该怎么做？你应该如何将这些信息“储存在某个地方”？这可以通过 `logging` 模块来实现。

保存为 `stdlib_logging.py`：

我们使用了三款标准库中的模块——`os` 模块用以和操作系统交互，`platform` 模块用以获取平台——操作系统——的信息，`logging` 模块用来*记录（Log）*信息。

{% highlight python linenos %}
import os
import platform
import logging

if platform.platform().startswith('Windows'):
    logging_file = os.path.join(os.getenv('HOMEDRIVE'),
                                os.getenv('HOMEPATH'),
                                'test.log')
else:
    logging_file = os.path.join(os.getenv('HOME'),
                                'test.log')

print("Logging to", logging_file)

logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s : %(levelname)s : %(message)s',
    filename=logging_file,
    filemode='w',
)

logging.debug("Start of the program")
logging.info("Doing something")
logging.warning("Dying now")
{% endhighlight %}

输出：

{% highlight shell linenos %}
$ python stdlib_logging.py
Logging to /Users/swa/test.log

$ cat /Users/swa/test.log
2014-03-29 09:27:36,660 : DEBUG : Start of the program
2014-03-29 09:27:36,660 : INFO : Doing something
2014-03-29 09:27:36,660 : WARNING : Dying now
{% endhighlight %}


### 每周模块系列

标准库中还有许多模块值得探索，例如一些[用以调试（Debugging）的模块](http://docs.python.org/3/library/pdb.html)， [处理命令行选项的模块](http://docs.python.org/3/library/argparse.html)，[正则表达式（Regular Expressions）模块](http://docs.python.org/3/library/re.html) 等等等等。

进一步探索标准库的最好方法是阅读由 Doug Hellmann 撰写的优秀的 [Python Module of the Week](http://pymotw.com/2/contents.html) 系列（你还可以阅读[它的实体书](http://amzn.com/0321767349)或是阅读 [Python 官方文档](http://docs.python.org/3/)）。

## 更多

### 传递元组

你可曾希望从一个函数中返回两个不同的值？你能做到的。只需要使用一个元组。

{% highlight python linenos %}
>>> def get_error_details():
...     return (2, 'details')
...
>>> errnum, errstr = get_error_details()
>>> errnum
2
>>> errstr
'details'
{% endhighlight %}

要注意到 `a, b = <some expression>` 的用法会将表达式的结果解释为具有两个值的一个元组。

这也意味着在 Python 中交换两个变量的最快方法是：

{% highlight python linenos %}
>>> a = 5; b = 8
>>> a, b
(5, 8)
>>> a, b = b, a
>>> a, b
(8, 5)
{% endhighlight %}

### 特殊方法

诸如 `__init__` 和 `__del__` 等一些方法对于类来说有特殊意义。

特殊方法用来模拟内置类型的某些行为。举个例子，如果你希望为你的类使用 `x[key]` 索引操作（就像你在列表与元组中使用的那样），那么你所需要做的只不过是实现 `__getitem__()` 方法，然后你的工作就完成了。如果你试图理解它，就想想 Python 就是对 `list` 类这样做的！

下面的表格列出了一些有用的特殊方法。如果你想了解所有的特殊方法，请[参阅手册](http://docs.python.org/3/reference/datamodel.html#special-method-names)。

- `__init__(self, ...)`
  - 这一方法在新创建的对象被返回准备使用时被调用。
- `__del__(self)`
  - 这一方法在对象被删除之前调用（它的使用时机不可预测，所以避免使用它）
- `__str__(self)`
  - 当我们使用 `print` 函数时，或 `str()` 被使用时就会被调用。
- `__lt__(self, other)`
  - 当*小于*运算符（<）被使用时被调用。类似地，使用其它所有运算符（+、> 等等）时都会有特殊方法被调用。
- `__getitem__(self, key)`
  - 使用 `x[key]` 索引操作时会被调用。
- `__len__(self)`
  - 当针对序列对象使用内置 `len()` 函数时会被调用

### 单语句块

我们已经见识过每一个语句块都由其自身的缩进级别与其它部分相区分。 是这样没错，不过有一个小小的警告。如果你的语句块只包括单独的一句语句，那么你可以在同一行指定它，例如条件语句与循环语句。下面这个例子应该能比较清楚地解释：

{% highlight python linenos %}
>>> flag = True
>>> if flag: print('Yes')
...
Yes
{% endhighlight %}

注意，单个语句是在原地立即使用的，它不会被看作一个单独的块。尽管，你可以通过这种方式来使你的程序更加*小巧*，但除非是为了检查错误，我强烈建议你避免使用这种快捷方法，这主要是因为如果你不小心使用了一个“恰到好处”的缩进，它就很容易添加进额外的语句。

### Lambda 表格

`lambda` 语句可以创建一个新的函数对象。从本质上说，`lambda` 需要一个参数，后跟一个表达式作为函数体，这一表达式执行的值将作为这个新函数的返回值。

案例（保存为 `more_lambda.py`）：

要注意到一个 `list` 的 `sort` 方法可以获得一个 `key` 参数，用以决定列表的排序方式（通常我们只知道升序与降序）。在我们的案例中，我们希望进行一次自定义排序，为此我们需要编写一个函数，但是又不是为函数编写一个独立的 `def` 块，只在这一个地方使用，因此我们使用 Lambda 表达式来创建一个新函数。

{% highlight python linenos %}
points = [{'x': 2, 'y': 3},
          {'x': 4, 'y': 1}]
points.sort(key=lambda i: i['y'])
print(points)
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python more_lambda.py
[{'y': 1, 'x': 4}, {'y': 3, 'x': 2}]
{% endhighlight %}

### 列表推导

列表推导（List Comprehension）用于从一份现有的列表中得到一份新列表。想象一下，现在你已经有了一份数字列表，你想得到一个相应的列表，其中的数字在大于 2 的情况下将乘以 2。列表推导就是这类情况的理想选择。

案例（保存为 `more_list_comprehension.py`）：

在本案例中，当满足了某些条件时（`if i > 2`），我们进行指定的操作（`2*i`），以此来获得一份新的列表。要注意到原始列表依旧保持不变。

使用列表推导的优点在于，当我们使用循环来处理列表中的每个元素并将其存储到新的列表中时时，它能减少样板（Boilerplate）代码的数量。

{% highlight python linenos %}
listone = [2, 3, 4]
listtwo = [2*i for i in listone if i > 2]
print(listtwo)
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python more_list_comprehension.py
[6, 8]
{% endhighlight %}

### 在函数中接收元组与字典

有一种特殊方法，即分别使用 `*` 或 `**` 作为元组或字典的前缀，来使它们作为一个参数为函数所接收。当函数需要一个可变数量的实参时，这将颇为有用。

{% highlight python linenos %}
>>> def powersum(power, *args):
...     '''Return the sum of each argument raised to the specified power.'''
...     total = 0
...     for i in args:
...         total += pow(i, power)
...     return total
...
>>> powersum(2, 3, 4)
25
>>> powersum(2, 10)
100
{% endhighlight %}

因为我们在 `args` 变量前添加了一个 `*` 前缀，函数的所有其它的额外参数都将传递到 `args` 中，并作为一个元组予以储存。如果采用的是 `**` 前缀，则额外的参数将被视为字典的键值—值配对。

### `assert` 语句

`assert` 语句用以断言（Assert）某事是真的。例如说你非常确定你正在使用的列表中至少包含一个元素，并想确认这一点，如果其不是真的，就抛出一个错误，`assert` 语句就是这种情况下的理想选择。当语句断言失败时，将会抛出 `AssertionError`。

{% highlight python linenos %}
>>> mylist = ['item']
>>> assert len(mylist) >= 1
>>> mylist.pop()
'item'
>>> assert len(mylist) >= 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError
{% endhighlight %}

你应该明智地选用 `assert` 语句。在大多数情况下，它好过捕获异常，也好过定位问题或向用户显示错误信息然后退出。

### 装饰器

装饰器（Decorators）是应用包装函数的快捷方式。这有助于将某一功能与一些代码一遍又一遍地“包装”。举个例子，我为自己创建了一个 `retry` 装饰器，这样我可以将其运用到任何函数之中，如果在一次运行中抛出了任何错误，它就会尝试重新运行，直到最大次数 5 次，并且每次运行期间都会有一定的延迟。这对于你在对一台远程计算机进行网络调用的情况十分有用。请参阅：

- <http://www.ibm.com/developerworks/linux/library/l-cpdecor.html>
- <http://toumorokoshi.github.io/dry-principles-through-python-decorators.html>

{% highlight python linenos %}
from time import sleep
from functools import wraps
import logging
logging.basicConfig()
log = logging.getLogger("retry")


def retry(f):
    @wraps(f)
    def wrapped_f(*args, **kwargs):
        MAX_ATTEMPTS = 5
        for attempt in range(1, MAX_ATTEMPTS + 1):
            try:
                return f(*args, **kwargs)
            except:
                log.exception("Attempt %s/%s failed : %s",
                              attempt,
                              MAX_ATTEMPTS,
                              (args, kwargs))
                sleep(10 * attempt)
        log.critical("All %s attempts failed : %s",
                     MAX_ATTEMPTS,
                     (args, kwargs))
    return wrapped_f


counter = 0


@retry
def save_to_database(arg):
    print("Write to a database or make a network call or etc.")
    print("This will be automatically retried if exception is thrown.")
    global counter
    counter += 1
    # 这将在第一次调用时抛出异常
    # 在第二次运行时将正常工作（也就是重试）
    if counter < 2:
        raise ValueError(arg)


if __name__ == '__main__':
    save_to_database("Some bad value")
{% endhighlight %}

输出：

{% highlight python linenos %}
$ python more_decorator.py
Write to a database or make a network call or etc.
This will be automatically retried if exception is thrown.
ERROR:retry:Attempt 1/5 failed : (('Some bad value',), {})
Traceback (most recent call last):
  File "more_decorator.py", line 14, in wrapped_f
    return f(*args, **kwargs)
  File "more_decorator.py", line 39, in save_to_database
    raise ValueError(arg)
ValueError: Some bad value
Write to a database or make a network call or etc.
This will be automatically retried if exception is thrown.
{% endhighlight %}

## 迈出下一步

如果到现在你已经阅读过本书并且编写了许多程序，那么你一定已经开始熟悉并且习惯 Python 了。或许你已经创建了一些 Python 程序来尝试完成一些工作，同时锻炼你自己的 Python 技能。如果你尚未至此，你也应该作出努力。现在我们面临的问题是“下一步该做什么？”。

我会建议你试图解决这个问题：

> 编写一款你自己的命令行*地址簿*程序，你可以用它浏览、添加、编辑、删除或搜索你的联系人，例如你的朋友、家人、同事，还有他们诸如邮件地址、电话号码等多种信息。这些详细信息必须被妥善储存以备稍后的检索。

如果你回想至今我们学过、讨论过、遇见过的所有东西，你会发现这其实非常简单。如果你仍想要有关如何进行的提示，这儿倒是有一些。[2](https://bop.molun.net/19.what_next.html#fn_2)

一旦你能够做到这件事，你便可以说自己是一名 Python 程序员了。现在，赶快[写封邮件](http://www.swaroopch.com/contact/)来感谢我写出了这么棒的一本书 ;-)。这一步并非强制但我仍建议如此。同时，请考虑[购买本书的实体书](http://www.swaroopch.com/buybook/)来支持本书的后续改进。

如果你觉得上面的程序太容易了，这还有另一个：

> 实现[替换命令](http://unixhelp.ed.ac.uk/CGI/man-cgi?replace)。这个命令能将一串字符串替换为另外提供的文件或列表中的另一串。

只要你想，替换命令可以或简单或复杂地实现，从简单的字符串替换到搜寻搭配的样式（正则表达式）。

### 下一个项目

如果你发现上面的程序都能很容易地编写出来，那么看看下面这个完整的项目列表，并尝试编写你自己的程序：<https://github.com/thekarangoel/Projects#numbers> (这一列表与 [Martyr2 的超级项目列表](http://www.dreamincode.net/forums/topic/78802-martyr2s-mega-project-ideas-list/)相同)。

你还可以看看：

- [Exercises for Programmers: 57 Challenges to Develop Your Coding Skills](https://pragprog.com/book/bhwb/exercises-for-programmers)
- [Intermediate Python Projects](https://openhatch.org/wiki/Intermediate_Python_Workshop/Projects)

### 示例代码

学习一门编程语言的最好方式就是编写大量代码，并阅读大量代码：

- [Python Cookbook](http://code.activestate.com/recipes/langs/python/) 是一本极具价值的“烹饪法”与提示的集合，它介绍了如何通过 Python 解决某些特定类型的问题。
- [Python Module of the Week](http://pymotw.com/2/contents.html) 是另一本优秀的[标准库](https://bop.molun.net/stdlib.md#stdlib)必读指南。

### 建议

- [The Hitchhiker's Guide to Python!](http://docs.python-guide.org/en/latest/)
- [The Elements of Python Style](https://github.com/amontalenti/elements-of-python-style)
- [Python Big Picture](http://slott-softwarearchitect.blogspot.ca/2013/06/python-big-picture-whats-roadmap.html)
- ["Writing Idiomatic Python" ebook](http://www.jeffknupp.com/writing-idiomatic-python-ebook/) （付费）

### 视频

- [Full Stack Web Development with Flask](https://github.com/realpython/discover-flask)
- [PyVideo](http://www.pyvideo.org/)

### 问与答

- [Official Python Dos and Don'ts](http://docs.python.org/3/howto/doanddont.html)
- [Official Python FAQ](http://www.python.org/doc/faq/general/)
- [Norvig's list of Infrequently Asked Questions](http://norvig.com/python-iaq.html)
- [Python Interview Q & A](http://dev.fyicenter.com/Interview-Questions/Python/index.html)
- [StackOverflow questions tagged with python](http://stackoverflow.com/questions/tagged/python)

### 教程

- [Hidden features of Python](http://stackoverflow.com/q/101268/4869)
- [What's the one code snippet/python trick/etc did you wish you knew when you learned python?](http://www.reddit.com/r/Python/comments/19dir2/whats_the_one_code_snippetpython_tricketc_did_you/)
- [Awaretek's comprehensive list of Python tutorials](http://www.awaretek.com/tutorials.html)

### 讨论

如果你遇到了一个 Python 问题，但不知道该问谁，那么 [python-tutor list](http://mail.python.org/mailman/listinfo/tutor) 是你提问的最佳场所。

请确保你会自己做你的家庭作业，你会首先尝试自己解决问题，同时，还要会[问聪明的问题](http://catb.org/~esr/faqs/smart-questions.html)。

### 新闻

如果你希望了解 Python 世界的最新动态，那就跟随 [Official Python Planet](http://planet.python.org/) 的脚步吧。

### 安装库

[Python 库索引](http://pypi.python.org/pypi)中包含了大量开源的库，你可以在你自己的程序中使用它们。

要想了解如何安装并使用这些库，你可以使用 [pip](http://www.pip-installer.org/en/latest/)。

### 创建一个网站

学习使用 [Flask](http://flask.pocoo.org/) 来创建你自己的网站。下面这些资源有助于你开始学习：

- [Flask Official Quickstart](http://flask.pocoo.org/docs/quickstart/)
- [The Flask Mega-Tutorial](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)
- [Example Flask Projects](https://github.com/mitsuhiko/flask/tree/master/examples)

### 图形软件

假设你希望使用 Python 来创建你自己的图形程序。这可以通过采用一个 GUI（Graphical User Interface，图形用户界面）库和它们的 Python 绑定来实现。绑定是允许你用 Python 编写你自己的程序，然后使用它们在 C 或 C++ 或其它语言写编写的库。

使用 Python 的 GUI 有许多选择：

- Kivy
  - [http://kivy.org](http://kivy.org/)
- PyGTK
  - 这是 GTK+ 工具包的 Python 绑定，它是构建 GNOME 的基础。GTK+ 有许多奇怪的用法，但是你一旦习惯了使用它，就能很快的创建出你的 GUI 应用。Glade 图形界面设计工具是不可或缺的。它的文档至今仍在不断改进。GTK+ 在 GNU/Linux 下能够良好工作，但是它针对 Windows 平台的移植工作尚未完成。你可以使用 GTK+ 创建免费或专有的软件。要想开始使用，请阅读 [PyGTK 教程](http://www.pygtk.org/tutorial.html)。
- PyQt
  - 这是 Qt 工具包的 Python 绑定，它是构建 KDE 的基础。 受益于 Qt Designer 与令人惊讶的 Qt 文档，Qt 十分容易使用也十分强大。如果你希望创建一款开源（GPL）软件，你可以免费使用 PyQt，不过如果你想创建专有的比原软件，你需要购买它。从 Qt 4.5 开始你可以使用它来创建不采用 GPL 授权的软件。要想开始使用，请阅读 [PySide](http://qt-project.org/wiki/PySide)。
- wxPython
  - 这是 wxWidgets 工具包的 Python 绑定。wxPython 有一个与之相关的学习曲线。不过，它非常便携，并且可以运行在 GNU/Linux、Windwos、Mac、甚至是嵌入式平台中。有许多 IDE 可以采用 wxPython，并且包含了 GUI 设计工具，例如 [SPE (Stani's Python Editor)](http://spe.pycs.net/) 还有 [wxGlade](http://wxglade.sourceforge.net/) GUI 构建工具。你可以使用 wxPython 来创建免费或专有的软件。要想开始使用，请阅读[wxPython 教程](http://zetcode.com/wxpython/)。

#### GUI 工具总结

想要了解更多的选择，可以参阅 [GuiProgramming wiki page at the official python website](http://www.python.org/cgi-bin/moinmoin/GuiProgramming)。

不幸的是，Python 没有一款标准 GUI 工具。我建议你根据你的实际情况从上面列出的工具中进行挑选。第一个因素是你是否愿意为使用任何 GUI 工具付费。第二个因素是你希望你的程序只在 Windwos 上运行，还是在 Mac 和 GNU/Linux 上运行，还是在它们三者之上都能运行。第三个因素，如果 GNU/Linux 是目标平台，那你是要做 KDE 用户还是 GNOME 用户。

有关更详尽且更全面的分析，请参阅 ['The Python Papers, Volume 3, Issue 1' (PDF)](http://archive.pythonpapers.org/ThePythonPapersVolume3Issue1.pdf) 的第 26 页。

### 各种实现

编程语言主要有两部分——语言与软件。语言是你*如何*编写，软件是你*怎样*实际运行我们的程序。

我们一直在使用 *CPython* 软件来运行我们的程序。它被成为 CPython 是因为它是使用 C 语言编写的，同时它也是*经典的（Classical） Python 解释器*。

还有其他软件可以运行你的 Python 程序：

- [Jython](http://www.jython.org/)
  - 在 Java 平台上运行的 Python 实现。这意味着你可以在 Python 语言中使用 Java 的库与类，反之亦然。
- [IronPython](http://www.codeplex.com/Wiki/View.aspx?ProjectName=IronPython)
  - 在 .NET 平台上运行的 Python 实现。这意味着你可以在 Python 语言中使用 .NET 的库与类，反之亦然
- [PyPy](http://codespeak.net/pypy/dist/pypy/doc/home.html)
  - 用 Python 编写的 Pyhon 实现！这是一项研究项目，旨在于使其能快速且方便的改进解释器，因为解释器本身就是用动态语言编写的了（而不是采用上述三种 C、Java、C# 等动态语言来编写）。

还有其它诸如 [CLPython](http://common-lisp.net/project/clpython/)——采用 Common Lisp 编写的 Python 实现，和[Brython](http://brython.info/) ，它在 JavaScript 解释器之上实现，意味着你可以使用 Python（而非 JavaScript）编写你的 Web 浏览器（“Ajax”）程序。

上述这些实现每一种都有其大有作为的专门领域。

### 函数式编程（面向高阶读者）

当你开始编写更加庞大的程序时，你应该清楚了解更多关于使用函数的方式来进行编程，而不是我们在[《面向对象编程》章节中](https://bop.molun.net/14.oop.html#oop)所学习的基于类的方式进行编程：

- [Functional Programming Howto by A.M. Kuchling](http://docs.python.org/3/howto/functional.html)
- [Functional programming chapter in 'Dive Into Python' book](http://www.diveintopython.net/functional_programming/index.html)
- [Functional Programming with Python presentation](http://ua.pycon.org/static/talks/kachayev/index.html)
- [Funcy library](https://github.com/Suor/funcy)
- [PyToolz library](http://toolz.readthedocs.org/en/latest/)