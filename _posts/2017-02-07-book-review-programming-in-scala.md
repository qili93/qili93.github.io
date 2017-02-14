---
layout: post
title: 《Programing in Scala》Chapter 1-3 introduction and first steps in scala
categories:  读书笔记
tags: 读书笔记
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Chapter 1: A Scalable Language](#chapter-1-a-scalable-language)
  - [What makes Scala scalable?](#what-makes-scala-scalable)
  - [Why Scala?](#why-scala)
- [Chapter 2: First Steps in Scala](#chapter-2-first-steps-in-scala)
  - [Step 1: Scala interpreter shell](#step-1-scala-interpreter-shell)
  - [Step 2: Define some variables](#step-2-define-some-variables)
  - [Step 3: Define some functions](#step-3-define-some-functions)
  - [Step 4: Write some Scala scripts](#step-4-write-some-scala-scripts)
  - [Step 5: Loop with *while* decide with *if*](#step-5-loop-with-while-decide-with-if)
  - [Step 6: Iterate with *foreach* and *for*](#step-6-iterate-with-foreach-and-for)
- [Chapter 3: Next Steps in Scala](#chapter-3-next-steps-in-scala)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


### Chapter 1: A Scalable Language

#### What makes Scala scalable?

- Scala is an **object-oriented** language in pure form: every value is an object and every operation is a method call.
- Scala is **functional**, and functional programming is guided by two main ideas:
    1. The first idea is that functions are first-class values. In a functional language, a function is a value of the same status as, say, an integer or a string.
    2. The second main idea of functional programming is that the operations of a program should map input values to output values rather than change data in place, that is, data structures are immutable.

#### Why Scala?

1) Scala is **compatible**: Scala was designed for seamless in- teroperability with Java. It allows you to add value to existing code—to build on what you already have. (While the backward compatibility of Scala itself is not good).

2) Scala is **concise**. Scala programs tend to be short. Scala programmers have reported reduc- tions in number of lines of up to a factor of ten compared to Java. For example, in Java, a class with a constructor often looks like this:

{% highlight java linenos %}
  // this is Java
  class MyClass {
      private int index;
      private String name;
      public MyClass(int index, String name) {
          this.index = index;
          this.name = name;
} }
{% endhighlight %}

In Scala, you would likely write this instead:

{% highlight scala linenos %}
  class MyClass(index: Int, name: String)
{% endhighlight %}

3) Scala is **high-level**. Scala helps you manage complexity by letting you raise the level of ab- straction in the interfaces you design and use. As an example, imagine you have a String variable name, and you want to find out whether or not that String contains an upper case character. In Java, you might write this:

{% highlight java linenos %}
  // this is Java
  boolean nameHasUpperCase = false;
  for (int i = 0; i < name.length(); ++i) {
      if (Character.isUpperCase(name.charAt(i))) {
          nameHasUpperCase = true;
          break;
} }
{% endhighlight %}

Whereas in Scala, you could write this:

{% highlight scala linenos %}
  val nameHasUpperCase = name.exists(_.isUpperCase)
{% endhighlight %}

4) Scala is **statically typed**. A static type system classifies variables and expressions according to the kinds of values they hold and compute. Scala stands out as a language with a very advanced static type system. (**TBD: more background of static type and then read this section again**)

### Chapter 2: First Steps in Scala

#### Step 1: Scala interpreter shell

Type `scala` at a command prompt

{% highlight bash linenos %}
$ scala
Welcome to Scala 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_121).
Type in expressions for evaluation. Or try :help.

scala> 1 + 2
res0: Int = 3
{% endhighlight %}

Type `:quit` to exit the scala interpreter shell

{% highlight bash linenos %}
scala> :quit
{% endhighlight %}

#### Step 2: Define some variables

Scala has two kinds of variables, vals and vars.
- A `val` is similar to a **final** variable in Java. Once initialized, a val can never be reassigned.

{% highlight bash linenos %}
scala> val msg = "Hello world!"
msg: String = Hello world!

scala> msg = "Goodbye world!"
<console>:12: error: reassignment to val
       msg = "Goodbye world!"
           ^
{% endhighlight %}

- A `var`, is similar to a **non-final** variable in Java. A var can be reassigned throughout its lifetime.

{% highlight bash linenos %}
scala> var msg = "Hello world!"
msg: String = Hello world!

scala> msg = "Goodbye world!"
msg: String = Goodbye world!

scala> println(msg)
Goodbye world!
{% endhighlight %}

<br/>
To enter something into the interpreter that spans multiple lines, just keep typing after the first line. If the code you typed so far is not complete, the interpreter will respond with a vertical bar on the next line.

{% highlight bash linenos %}
scala> val multiLine =
     | "This is the next line."
multiLine: String = This is the next line.
{% endhighlight %}

If you realize you have typed something wrong, but the interpreter is still waiting for more input, you can escape by pressing enter twice:

{% highlight bash linenos %}
scala> val oops =
     |
     |
You typed two blank lines.  Starting a new command.

scala>
{% endhighlight %}

#### Step 3: Define some functions

Scala’s Unit type is similar to Java’s void type, and in fact every void-returning method in Java is mapped to a Unit-returning method in Scala.

{% highlight bash linenos %}
scala> def greet() = println("Hello, world!")
greet: ()Unit
{% endhighlight %}

#### Step 4: Write some Scala scripts

Put this into a file named `hello.scala` and then run:

{% highlight bash linenos %}
$ echo 'println("Hello, "+ args(0) +"!")' > hello.scala
$ scala hello.scala world
Hello, world!
{% endhighlight %}

#### Step 5: Loop with *while* decide with *if*

{% highlight scala linenos %}
var i = 0
while (i < args.length) {
    if (i != 0)
        print(" ")
    print(args(i))
i += 1
}
println()
{% endhighlight %}

#### Step 6: Iterate with *foreach* and *for*

Program in a functional style:

{% highlight bash linenos %}
args.foreach(arg => println(arg))
args.foreach(println)
for (arg <- args)
    println(arg)
{% endhighlight %}

### Chapter 3: Next Steps in Scala