---
layout: post
title: Kerberos认证原理
categories: 技术解读
tags: Kerberos
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

参考文章
1. [Kerberos认证原理](http://blog.csdn.net/wulantian/article/details/42418231)
2. [Kerberos Explained](https://msdn.microsoft.com/en-us/library/bb742516.aspx)

### Chapter 1: 走进Java

#### Java技术体系

{% mermaid %}
sequenceDiagram
participant KDC
participant ClientA
participant ServerB
ClientA ->> KDC: [1] I'm clientA, I want to visit serverA
Note over KDC,ClientA: KDC create new sessionKey
KDC -->> ClientA: [2] msg1: sessionKey(encrypt by masterKeyA)
KDC -->> ClientA: [2] msg2: sessionKey+clientAInfo(encrypt by masterKeyB)
Note over ClientA,ServerB: decrypt sessionKey by hash(pwdA)
ClientA ->> ServerB: msg2: sessionKey+clientAInfo(encrypt by masterKeyB)
ClientA ->> ServerB: msg3: TimeStamp+clientAInfo+(encrypt by sessionKey)
Note over ClientA,ServerB: decrypt sessionKey by hash(pwdB) + timeStamp by sessionKey
ServerB -->> ClientA: msg4: TimeStamp (for mutual authentication)
{% endmermaid %}



{% highlight bash linenos %}
args.foreach(arg => println(arg))
args.foreach(println)
for (arg <- args)
    println(arg)
{% endhighlight %}

### Chapter 3: Next Steps in Scala