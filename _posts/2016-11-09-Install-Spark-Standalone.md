---
layout: post
title: Install Spark Standalone without HDFS
categories:  Spark
tags: Spark
---

### Download Spark package and unzip

{% highlight bash linenos %}
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.6.2-bin-hadoop2.6.tgz /myhome/hadoop/upload
tar -zxvf spark-1.6.2-bin-hadoop2.6.tgz -C /myhome/hadoop/
{% endhighlight %}

### Configure Spark environment in /etc/profile

{% highlight bash linenos %}
export SPARK_HOME=/myhome/hadoop/spark-1.6.2-bin-hadoop2.6
export PATH=$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH
{% endhighlight %}

### Configure Spark Master and Slave

Backup first

{% highlight bash linenos %}
cp slaves.template slaves
{% endhighlight %}
A Spark Worker will be started on each of the machines listed below.

{% highlight bash linenos %}
bjqilitst1
bjqilitst2
bjqilitst3
{% endhighlight %}

### Configure $SPARK_HOME/conf/spark-env.sh
Backup first

{% highlight bash linenos %}
cp spark-env.sh.template spark-env.sh
{% endhighlight %}

Add the following environment parameters into the end of spark-env.sh

{% highlight bash linenos %}
export SPARK_MASTER_IP=9.111.159.156
export SPARK_MASTER_PORT=7077
export SPARK_WORKER_CORES=1
export SPARK_WORKER_INSTANCES=1
export SPARK_WORKER_MEMORY=512M
export MASTER=spark://9.111.159.156:7077
{% endhighlight %}

### Distribute the spark installtion dir to all nodes in the cluster

{% highlight bash linenos %}
cd /myhome/hadoop/
scp -r spark-1.6.2-bin-hadoop2.6 hadoop@bjqilitst2:/myhome/hadoop/
scp -r spark-1.6.2-bin-hadoop2.6 hadoop@bjqilitst3:/myhome/hadoop/
{% endhighlight %}

### Start Spark standalone

{% highlight bash linenos %}
cd /myhome/hadoop/spark-1.6.2-bin-hadoop2.6/sbin
./start-all.sh
{% endhighlight %}

### Verify the installation

{% highlight bash linenos %}
[hadoop@bjqilitst1 logs]$ jps
17439 Master
17649 Jps
17518 Worker
[hadoop@bjqilitst2 logs]$ jps
18075 Worker
18168 Jps
[hadoop@bjqilitst3 logs]$ jps
17558 Jps
17469 Worker
[hadoop@bjqilitst1 logs]$ netstat -nlt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp6       0      0 :::8080                 :::*                    LISTEN
{% endhighlight %}

### Fix the "No route to host" issue
Disable the proxy server on ~/.bash_profile

{% highlight bash linenos %}
yum  -y install telnet.x86_64  telnet-server.x86_64
systemctl start telnet.socket
systemctl enable telnet.socket => disable
systemctl stop firewalld.service
systemctl disable firewalld.service
{% endhighlight %}

### Submit Spark job via CLI

{% highlight bash linenos %}
cd /myhome/hadoop/spark-1.6.2-bin-hadoop2.6/bin
./spark-submit --master spark://9.111.159.156:7077 --class org.apache.spark.examples.SparkPi --executor-memory 512m ../lib/spark-examples-1.6.2-hadoop2.6.0.jar 2000
./spark-submit --master spark://9.111.159.156:7077 --class main.scala.internals.GroupByKeyTest --executor-memory 512m /myhome/hadoop/upload/GroupByKeyTest1102.jar
{% endhighlight %}

###  View Spark Master GUI

Open the following link after spark master started http://9.111.159.156:8080/

