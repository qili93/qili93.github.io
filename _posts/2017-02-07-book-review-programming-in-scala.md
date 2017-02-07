---
layout: post
title: Programing in Scala - Chapter 1-2
categories:  读书笔记
tags: 读书笔记
---

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

