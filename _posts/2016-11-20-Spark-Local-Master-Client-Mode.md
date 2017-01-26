---
layout: post
title: Spark Mode - Local/Client/Cluster
categories:  Spark
tags: Spark
---

Refer to http://spark.apache.org/docs/latest/submitting-applications.html

## Launching Applications with spark-submit

Once a user application is bundled, it can be launched using the `bin/spark-submit` script.This script takes care of setting up the classpath with Spark and itsdependencies, and can support different cluster managers and deploy modes that Spark supports:

{% highlight bash linenos %}
./bin/spark-submit \
  --class <main-class> \
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf <key>=<value> \
  ... # other options
  <application-jar> \
  [application-arguments]
{% endhighlight %}

Some of the commonly used options are:

- `--class`: The entry point for your application (e.g. `org.apache.spark.examples.SparkPi`)
- `--master`: The [master URL](http://spark.apache.org/docs/latest/submitting-applications.html#master-urls) for the cluster (e.g. `spark://23.195.26.187:7077`)
- `--deploy-mode`: Whether to deploy your driver on the worker nodes (`cluster`) or locally as an external client (`client`) (default: `client`)
- `--conf`: Arbitrary Spark configuration property in key=value format. For values that contain spaces wrap “key=value” in quotes (as shown).
- `application-jar`: Path to a bundled jar including your application and all dependencies. The URL must be globally visible inside of your cluster, for instance, an `hdfs://` path or a `file://` path that is present on all nodes.
- `application-arguments`: Arguments passed to the main method of your main class, if any



### Master URLs

The master URL passed to Spark can be in one of the following formats:

| Master URL          | Meaning                                  |
| ------------------- | ---------------------------------------- |
| `local`             | Run Spark locally with one worker thread (i.e. no parallelism at all). |
| `local[K]`          | Run Spark locally with K worker threads. |
| `local[*]`          | Run Spark locally with as many worker threads as logical cores on your machine. |
| `spark://HOST:PORT` | Connect to the given [Spark standalone cluster](http://spark.apache.org/docs/latest/spark-standalone.html) master. |
| `mesos://HOST:PORT` | Connect to the given [Mesos](http://spark.apache.org/docs/latest/running-on-mesos.html) cluster. |
| `yarn`              | Connect to a [ YARN ](http://spark.apache.org/docs/latest/running-on-yarn.html) cluster in  `client` or `cluster` mode. |



## Spark Shell Command

Run the following commands is  running succesffully in both local machine or cluster node.

{% highlight bash linenos %}
# Local Mode
./spark-shell

# Client Mode
./spark-shell --master spark://9.111.159.156:7077

# Cluster Mode
./spark-shell --master spark://9.111.159.156:7077 --deploy-mode cluster
{% endhighlight %}



## Spark Local Mode

Run the following command in the local laptop/cluster node
{% highlight bash linenos %}
./spark-submit \
 --class main.scala.internals.GroupByKeyTest \
 --master local[2] \
/out/artifacts/GroupByKeyTest1102_jar/GroupByKeyTest1102.jar
{% endhighlight %}



## Spark Client Mode

Run the following command in the local laptop/cluster node
{% highlight bash linenos %}
./spark-submit \
--master spark://9.111.159.156:7077 \
--class org.apache.spark.examples.GroupByTest \
../lib/spark-examples-1.6.2-hadoop2.6.0.jar

./spark-submit \
 --class main.scala.internals.GroupByKeyTest \
 --master spark://9.111.159.156:7077 \
 --deploy-mode client \
/myhome/hadoop/upload/GroupByKeyTest1102.jar
{% endhighlight %}



## Spark Cluster Mode

Run the following command in the local laptop/cluster node

{% highlight bash linenos %}
./spark-submit \
--master spark://9.111.159.156:7077 \
--class org.apache.spark.examples.GroupByTest \
 --deploy-mode cluster \
../lib/spark-examples-1.6.2-hadoop2.6.0.jar \
2 1000 1000 2

./spark-submit \
--master spark://9.111.159.156:7077 \
--class org.apache.spark.examples.GroupByTest \
--deploy-mode cluster \
../lib/spark-examples-1.6.2-hadoop2.6.0.jar \
100 1000 10000 36
{% endhighlight %}
