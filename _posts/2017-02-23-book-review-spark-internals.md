---
layout: post
title: 《Spark Internals》- Chapter 1
categories:  读书笔记
tags: Spark
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

《Spark Internal》is available on [GitBook](https://spark-internals.books.yourtion.com/)



### Chapter 1: 走进Java

#### Java技术体系



{% highlight bash linenos %}
args.foreach(arg => println(arg))
args.foreach(println)
for (arg <- args)
    println(arg)
{% endhighlight %}

### Chapter 3: Next Steps in Scala