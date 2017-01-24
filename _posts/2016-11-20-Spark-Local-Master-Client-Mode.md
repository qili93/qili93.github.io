---
layout: post
title: Spark Mode - Local/Client/Cluster
categories:  Spark
tags: Spark
---

Refer to http://spark.apache.org/docs/latest/submitting-applications.html

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

  - [Launching Applications with spark-submit](#launching-applications-with-spark-submit)
    - [Master URLs](#master-urls)
  - [Spark Shell Command](#spark-shell-command)
- [Local Mode](#local-mode)
- [Client Mode](#client-mode)
- [Cluster Mode](#cluster-mode)
  - [Spark Local Mode](#spark-local-mode)
  - [Spark Client Mode](#spark-client-mode)
  - [Spark Cluster Mode](#spark-cluster-mode)
  - [Spark UI to check app status](#spark-ui-to-check-app-status)
    - [Spark Master Web GUI](#spark-master-web-gui)
    - [Spark Driver Web GUI](#spark-driver-web-gui)
    - [Spark History Server UI](#spark-history-server-ui)
  - [Spark Application Sample](#spark-application-sample)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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



## Spark UI to check app status

### Spark Master Web GUI

` http://<master-node>:8080 `

### Spark Driver Web GUI

Every SparkContext launches a web UI, by default on port 4040, thatdisplays useful information about the application. This includes:

- A list of scheduler stages and tasks
- A summary of RDD sizes and memory usage
- Environmental information.
- Information about the running executors

You can access this interface by simply opening `http://<driver-node>:4040` in a web browser.If multiple SparkContexts are running on the same host, they will bind to successive portsbeginning with 4040 (4041, 4042, etc).

Note that this information is only available for the duration of the application by default.To view the web UI after the fact, set `spark.eventLog.enabled` to true before starting theapplication. This configures Spark to log Spark events that encode the information displayedin the UI to persisted storage.

### Spark History Server UI

If Spark is run on Mesos or YARN, it is still possible to construct the UI of anapplication through Spark’s history server, provided that the application’s event logs exist.You can start the history server by executing:

{% highlight bash linenos %}
./sbin/start-history-server.sh

{% endhighlight %}

This creates a web interface at `http://<server-url>:18080` by default, listing incompleteand completed applications and attempts.

When using the file-system provider class (see `spark.history.provider` below), the base loggingdirectory must be supplied in the `spark.history.fs.logDirectory` configuration option,and should contain sub-directories that each represents an application’s event logs.

The spark jobs themselves must be configured to log events, and to log them to the same shared,writeable directory. For example, if the server was configured with a log directory of`hdfs://namenode/shared/spark-logs`, then the client-side options would be:

{% highlight properties linenos %}
spark.eventLog.enabled  true
spark.eventLog.dir      hdfs://namenode/shared/spark-logs
{% endhighlight %}


## Spark Application Sample

The following sample is compiled and packaged by IDEA

{% highlight scala linenos %}
package main.scala.internals

/**
  * Created by qilibj on 02/11/2016.
  */

import java.util.Random

import org.apache.spark.{SparkConf, SparkContext}
import org.apache.spark.SparkContext._

/**
  * Usage: GroupByTest [numMappers] [numKVPairs] [KeySize] [numReducers]
  */
object GroupByKeyTest {
  def main(args: Array[String]) {
    // val sparkConf = new SparkConf().setAppName("GroupBy Test").setMaster("local[2]")
    val sparkConf = new SparkConf().setAppName("GroupBy Test")
    var numMappers = 100 //10
    var numKVPairs = 10000 //100
    var valSize = 1000 //100
    var numReducers = 36 //3

    val sc = new SparkContext(sparkConf)

    val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
      val ranGen = new Random
      var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
      for (i <- 0 until numKVPairs) {
        val byteArr = new Array[Byte](valSize)
        ranGen.nextBytes(byteArr)
        arr1(i) = (ranGen.nextInt(10), byteArr)
      }
      arr1
    }.cache
    // Enforce that everything has been calculated and in cache
    pairs1.count

    val result = pairs1.groupByKey(numReducers)
    println(result.count)
    println(result.toDebugString)

    sc.stop()
  }
}
{% endhighlight %}

