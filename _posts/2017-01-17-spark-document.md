---
layout: post
title: Spark相关文章
categories:  剪贴板
tags: Spark
---

本文整理了Spark相关的一些文章和链接，便于查询。持续更新中...

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Spark架构分析](#spark%E6%9E%B6%E6%9E%84%E5%88%86%E6%9E%90)
- [Spark源码分析](#spark%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
- [Spark参数配置](#spark%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE)
- [Spark Summit](#spark-summit) 
- [Spark Blogs](#spark-blogs)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

### Spark架构分析

[Spark Internals](https://github.com/JerryLead/SparkInternals/tree/master/markdown)
主要讨论 Apache Spark 的设计与实现，重点关注其设计思想、运行原理、实现架构及性能调优，附带讨论与 Hadoop MapReduce 在设计与实现上的区别。


### Spark源码分析

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

### Spark Blogs
[Apache Spark Future](https://0x0fff.com/apache-spark-future/) 
根据Spark Survey 和 Spark 2.0的进展对Spark未来的发展方向进行分析。
