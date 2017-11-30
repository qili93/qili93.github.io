---
layout: post
title: Spark Fundamentals 1 - Introduction to Spark - Lab 2b
categories: Spark
tags: Spark
---

{% include toc.html html=content sanitize=true class="inline_toc" id="my_toc" h_min=2 h_max=3 %}

## Lab 2b. Python - Working with RDD operations 

#### June 08, 2017 

**Related free online courses:**

Related courses can be found in the following learning paths:

- [Spark Fundamentals path](https://cognitiveclass.ai/learn/spark/)
- [Big Data Fundamentals path](https://cognitiveclass.ai/learn/big-data/)

<!-- excerpt -->

## Analyzing a log file

First, let's download the data that we will working with in this lab.

In [4]:

{% highlight shell linenos %}
# download the data from the IBM server
# this may take ~30 seconds depending on your interent speed
!wget --quiet https://ibm.box.com/shared/static/1c65hfqjxyxpdkts42oab8i8mzxbpvc8.zip
print "Data Downloaded!"
{% endhighlight %}

Out [4]:

{% highlight shell linenos %}
Data Downloaded!
{% endhighlight %}

In [5]:

{% highlight shell linenos %}
# unzip the folder's content into "resources" directory
# this may take ~30 seconds depending on your internet speed
!unzip -q -o -d /resources 1c65hfqjxyxpdkts42oab8i8mzxbpvc8.zip
print "Data Extracted!"
{% endhighlight %}

Out [5]:

{% highlight shell linenos %}
Data Extracted!
{% endhighlight %}

In [6]:

{% highlight shell linenos %}
# list the extracted files
!ls -1 /resources/LabData/
{% endhighlight %}

Out [6]:

{% highlight shell linenos %}
followers.txt
notebook.log
nyctaxi100.csv
nyctaxi.csv
nyctaxisub.csv
nycweather.csv
pom.xml
README.md
taxistreams.py
users.txt
{% endhighlight %}

Now, let's create an RDD by loading the log file that we analyze in the Scala version of this lab.

In [7]:

{% highlight python linenos %}
logFile = sc.textFile("/resources/LabData/notebook.log")
{% endhighlight %}

### YOUR TURN:

#### In the cell below, filter out the lines that contains INFO

In [8]:

{% highlight python linenos %}
# WRITE YOUR CODE BELOW
info = logFile.filter(lambda x: "INFO" in x)
{% endhighlight %}

Highlight text for answer:

#### Count the lines:

In [18]:

{% highlight python linenos %}
# WRITE YOUR CODE BELOW
info.count()
{% endhighlight %}

Out[18]:

{% highlight python linenos %}
13438
{% endhighlight %}

Highlight text for answer:

#### Count the lines with "spark" in it by combining transformation and action.

In [17]:

{% highlight python linenos %}
# WRITE YOUR CODE BELOW
logFile.filter(lambda x: "INFO" in x).count()
{% endhighlight %}

Out[17]:

{% highlight python linenos %}
13438
{% endhighlight %}

Highlight text for answer:

#### Fetch those lines as an array of Strings

In [12]:

{% highlight python linenos %}
# WRITE YOUR CODE BELOW
info.collect()
{% endhighlight %}

Out[12]:

{% highlight python linenos %}
[u'15/10/14 14:29:21 INFO SparkContext: Running Spark version 1.4.1',
 u'15/10/14 14:29:22 INFO SecurityManager: Changing view acls to: notebook',
 u'15/10/14 14:29:22 INFO SecurityManager: Changing modify acls to: notebook',
... ...
{% endhighlight %}

Highlight text for answer:

View the graph of an RDD using this command:

In [13]:

{% highlight python linenos %}
print info.toDebugString() 
{% endhighlight %}

Out [13]:

{% highlight python linenos %}
(2) PythonRDD[6] at collect at <ipython-input-12-6ddb1a8a7038>:2 []
 |  MapPartitionsRDD[3] at textFile at NativeMethodAccessorImpl.java:-2 []
 |  /resources/LabData/notebook.log HadoopRDD[2] at textFile at NativeMethodAccessorImpl.java:-2 []
{% endhighlight %}

## Joining RDDs

Next, you are going to create RDDs for the same README and the POM files that we used in the Scala version.

In [15]:

{% highlight python linenos %}
readmeFile = sc.textFile("/resources/LabData/README.md")
pomFile = sc.textFile("/resources/LabData/pom.xml")
{% endhighlight %}

How many Spark keywords are in each file?

In [16]:

{% highlight python linenos %}
print readmeFile.filter(lambda line: "Spark" in line).count()
print pomFile.filter(lambda line: "Spark" in line).count()
{% endhighlight %}

Out [16]:

{% highlight python linenos %}
18
2
{% endhighlight %}

Now do a WordCount on each RDD so that the results are (K,V) pairs of (word,count)

In [19]:

{% highlight python linenos %}
readmeCount = readmeFile.                    \
    flatMap(lambda line: line.split("   ")).   \
    map(lambda word: (word, 1)).             \
    reduceByKey(lambda a, b: a + b)

pomCount = pomFile.                          \
    flatMap(lambda line: line.split("   ")).   \
    map(lambda word: (word, 1)).            \
    reduceByKey(lambda a, b: a + b)
{% endhighlight %}

To see the array for either of them, just call the collect function on it.

In [20]:

{% highlight python linenos %}
print "Readme Count\n"
print readmeCount.collect()
{% endhighlight %}

Out [20]:

{% highlight python linenos %}
Readme Count

[(u'', 43), (u'rich set of higher-level tools including Spark SQL for SQL and DataFrames,', 1), (u'and Spark Streaming for stream processing.', 1), (u'Spark uses the Hadoop core library to talk to HDFS and other Hadoop-supported', 1), ... ...]
{% endhighlight %}

In [21]:

{% highlight python linenos %}
print "Pom Count\n"
print pomCount.collect()
{% endhighlight %}

Out [21]:

{% highlight python linenos %}
Pom Count

[(u'', 841), (u'<transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">', 1), (u' <artifactId>protobuf-java</artifactId>', 1), (u' <artifactId>jline</artifactId>', 1),... ...]
{% endhighlight %}

The join function combines the two datasets (K,V) and (K,W) together and get (K, (V,W)). Let's join these two counts together.

In [22]:

{% highlight python linenos %}
joined = readmeCount.join(pomCount)
{% endhighlight %}

Print the value to the console

In [23]:

{% highlight python linenos %}
joined.collect()
{% endhighlight %}

Out[23]:

{% highlight python linenos %}
[(u'', (43, 841))]
{% endhighlight %}

Let's combine the values together to get the total count

In [24]:

{% highlight python linenos %}
joinedSum = joined.map(lambda k: (k[0], (k[1][0]+k[1][1])))
{% endhighlight %}

To check if it is correct, print the first five elements from the joined and the joinedSum RDD

In [25]:

{% highlight python linenos %}
print "Joined Individial\n"
print joined.take(5)

print "\n\nJoined Sum\n"
print joinedSum.take(5)
{% endhighlight %}

{% highlight python linenos %}
Joined Individial
[(u'', (43, 841))]

Joined Sum
[(u'', 884)]
{% endhighlight %}

## Shared variables

Normally, when a function passed to a Spark operation (such as map or reduce) is executed on a remote cluster node, it works on separate copies of all the variables used in the function. These variables are copied to each machine, and no updates to the variables on the remote machine are propagated back to the driver program. Supporting general, read-write shared variables across tasks would be inefficient. However, Spark does provide two limited types of shared variables for two common usage patterns: broadcast variables and accumulators.

### Broadcast variables

Broadcast variables are useful for when you have a large dataset that you want to use across all the worker nodes. A read-only variable is cached on each machine rather than shipping a copy of it with tasks. Spark actions are executed through a set of stages, separated by distributed “shuffle” operations. Spark automatically broadcasts the common data needed by tasks within each stage.

Read more here: [http://spark.apache.org/docs/latest/programming-guide.html#broadcast-variables](http://spark.apache.org/docs/latest/programming-guide.html#broadcast-variables)

Create a broadcast variable. Type in:

In [26]:

{% highlight python linenos %}
broadcastVar = sc.broadcast([1,2,3])
{% endhighlight %}

To get the value, type in:

In [27]:

{% highlight python linenos %}
broadcastVar.value
{% endhighlight %}

Out[27]:

{% highlight python linenos %}
[1, 2, 3]
{% endhighlight %}

### Accumulators

Accumulators are variables that can only be added through an associative operation. It is used to implement counters and sum efficiently in parallel. Spark natively supports numeric type accumulators and standard mutable collections. Programmers can extend these for new types. Only the driver can read the values of the accumulators. The workers can only invoke it to increment the value.

Create the accumulator variable. Type in:

In [28]:

{% highlight python linenos %}
accum = sc.accumulator(0)
{% endhighlight %}

Next parallelize an array of four integers and run it through a loop to add each integer value to the accumulator variable. Type in:

In [29]:

{% highlight python linenos %}
rdd = sc.parallelize([1,2,3,4])
def f(x):
    global accum
    accum += x
{% endhighlight %}

Next, iterate through each element of the rdd and apply the function f on it:

In [30]:

{% highlight python linenos %}
rdd.foreach(f)
{% endhighlight %}

To get the current value of the accumulator variable, type in:

In [31]:

{% highlight python linenos %}
accum.value
{% endhighlight %}

Out[31]:

{% highlight python linenos %}
10
{% endhighlight %}

You should get a value of 10.

This command can only be invoked on the driver side. The worker nodes can only increment the accumulator.

## Key-value pairs

You have already seen a bit about key-value pairs in the Joining RDD section.

Create a key-value pair of two characters. Type in:

In [32]:

{% highlight python linenos %}
pair = ('a', 'b')
{% endhighlight %}

To access the value of the first index use [0] and [1] method for the 2nd.

In [33]:

{% highlight python linenos %}
print pair[0]
print pair[1]
{% endhighlight %}

Out [33]:

{% highlight python linenos %}
a
b
{% endhighlight %}

### Summary

Having completed this exercise, you should now be able to describe Spark’s primary data abstraction, work with Resilient Distributed Dataset (RDD) operations, and utilize shared variables and key-value pairs.

This notebook is part of the free course on **Cognitive Class** called *Spark Fundamentals I*. If you accessed this notebook outside the course, you can take this free self-paced course, online by going to: [https://cognitiveclass.ai/courses/what-is-spark/](https://cognitiveclass.ai/courses/what-is-spark/)

### About the Authors:

Hi! It's [Alex Aklson](https://www.linkedin.com/in/aklson/), one of the authors of this notebook. I hope you found this lab educational! There is much more to learn about Spark but you are well on your way. Feel free to connect with me if you have any questions.