---
layout: post
title: Compile Spark and Build Spark Package
categories:  Spark
tags: Spark
---

### Step 1: Download Spark source code from git

Download Link of [Apache Spark](http://spark.apache.org/downloads.html)

*Note: Starting version 2.0, Spark is built with Scala 2.11 and SBT 0.13.11 by default.*

{% highlight bash linenos %}
# Master development branch
git clone git://github.com/apache/spark.git

# 2.1 maintenance branch with stability fixes on top of Spark 2.1.0
git clone git://github.com/apache/spark.git -b branch-2.1
{% endhighlight %}

### Step 2: Configure Maven and Compile

Refer to [Building Apache Spark](http://spark.apache.org/docs/latest/building-spark.html) for more details.

(Optional) Export the mvn path within spark source code  if not installed in your environment.

{% highlight bash linenos %}
export MAVEN_HOME=/app/compiled/spark/build/apache-maven-3.3.9
export PATH=$PATH:$MAVEN_HOME/bin
{% endhighlight %}

If using build/mvn with no MAVEN_OPTS set, the script will automate this for you.

{% highlight bash linenos %}
export MAVEN_OPTS="-Xmx2g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m"
{% endhighlight %}

If you want to read from HDFS

{% highlight bash linenos %}
./build/mvn -Pyarn -Phadoop-2.6 -Dhadoop.version=2.6.0 -DskipTests clean package
{% endhighlight %}

Or using SBT to compile to support day-to-day development since it can provide much faster iterative compilation.

{% highlight bash linenos %}
./build/sbt -Pyarn -Phadoop-2.6 -Dhadoop.version=2.6.0 -DskipTests clean package
{% endhighlight %}

**Fix the build error**

The following  issue gone after reboot the machine

{% highlight bash linenos %}
[error] Required file not found: sbt-interface.jar
[error] See zinc -help for information about locating necessary files
{% endhighlight %}

### Step 3: Build a Runnable Distribution

{% highlight bash linenos %}
./dev/make-distribution.sh --name custom-spark --tgz -Phadoop-2.6
{% endhighlight %}