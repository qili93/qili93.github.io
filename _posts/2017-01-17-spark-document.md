---
layout: post
title: Spark相关文章和学习资源
categories:  Spark
tags: Spark
---

本文整理了Spark相关的一些文章和链接，便于查询。持续更新中...

### Spark架构分析

[Spark Internals](https://github.com/JerryLead/SparkInternals/tree/master/markdown)
主要讨论 Apache Spark 的设计与实现，重点关注其设计思想、运行原理、实现架构及性能调优，附带讨论与 Hadoop MapReduce 在设计与实现上的区别。

### Spark源码分析

[Mastering Apache Spark 2.0](https://jaceklaskowski.gitbooks.io/mastering-apache-spark/)
源自Spark community developer list, 详细介绍了Spark 2.0的相关内容。

[Spark Scala Tutorial](https://github.com/deanwampler/spark-scala-tutorial)
Step by step tutorial of programing Spark applications in scala and running in local mode.

[Spark源码分析之-Storage模块](http://jerryshao.me/architecture/2013/10/08/spark-storage-module-analysis/)
该作者博客中共有deploy, scheduler和storage三个模块的源码分析，非常详尽。

[Spark源码走读系列-徽沪一郎](http://www.cnblogs.com/hseagle/category/569175.html)
详尽的中文博客，手把手教你学会安装部署和调试Spark。

### Spark参数配置

[Apache Spark: Config Cheatsheet](http://c2fo.io/c2fo/spark/aws/emr/2016/07/06/apache-spark-config-cheatsheet//)
根据cluster配置情况，例如worker node数量、机器核数和内存大小等来给出建议的spark.exeuctor.instances等值，来提升机器性能。

### Spark Summit
[Spark Summit 2016](https://spark-summit.org/2016/schedule/)
Agenda and videos of Spark Summit in June, 2016

[Spark Summit East 2017](https://spark-summit.org/east-2017/schedule/)
Agenda and videos of Spark Summit East 2017 in Feb, 2017

### Spark Blogs
[Apache Spark Future](https://0x0fff.com/apache-spark-future/)
根据Spark Survey 和 Spark 2.0的进展对Spark未来的发展方向进行分析。

### Spark 论文

[Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing](http://www-bcf.usc.edu/~minlanyu/teach/csci599-fall12/papers/nsdi_spark.pdf)