---
layout: post
title: Spark Fundamentals 1 - Lab 2a. Scala - Working with RDD operations
categories: Spark
tags: Spark
---

# Spark Fundamentals 1 - Introduction to Spark 

## Lab 2a. Scala - Working with RDD operations

#### June 08, 2017

**Related free online courses:**

Related courses can be found in the following learning paths:

- [Spark Fundamentals path](https://cognitiveclass.ai/learn/spark/)
- [Big Data Fundamentals path](https://cognitiveclass.ai/learn/big-data/)

### Starting with Spark using Scala

### Run the following lines of code to get the data

In [1]:

{% highlight shell linenos %}
// download the required module to run shell commands within the notebook
import sys.process._
{% endhighlight %}

In [2]:

{% highlight shell linenos %}
// download the data from the IBM Server
// this may take ~30 seconds depending on your internet speed
"wget --quiet https://ibm.box.com/shared/static/1c65hfqjxyxpdkts42oab8i8mzxbpvc8.zip" !
println("Data Downloaded!")
{% endhighlight %}

Out[2]:

{% highlight shell linenos %}
Data Downloaded!
{% endhighlight %}

In [3]:

{% highlight shell linenos %}
// unzip the folder's content into "resources" directory
// this may take ~30 seconds depending on your internet speed
"unzip -q -o -d /resources 1c65hfqjxyxpdkts42oab8i8mzxbpvc8.zip" !
println("Data Extracted!")
{% endhighlight %}

Out[3]:

{% highlight shell linenos %}
Data Extracted!
{% endhighlight %}

In [4]:

{% highlight shell linenos %}
// list the extracted files
"ls -1 /resources/LabData/" !
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

Now we are going to create an RDD file from the file README. This is created using the spark context ".textFile" just as in the previous lab. As we know the initial operation is a transformation, so nothing actually happens. We're just telling it that we want to create a readme RDD.

Run the code in the following cell. This was an RDD transformation, thus it returned a pointer to a RDD, which we have named as readme.

In [5]:

{% highlight scala linenos %}
val readme = sc.textFile("/resources/LabData/README.md")
{% endhighlight %}

Let’s perform some RDD actions on this text file. Count the number of items in the RDD using this command:

In [6]:

{% highlight scala linenos %}
readme.count()
{% endhighlight %}

Out[6]:

{% highlight shell linenos %}
98
{% endhighlight %}

Let’s run another action. Run this command to find the first item in the RDD:

In [7]:

{% highlight scala linenos %}
readme.first()
{% endhighlight %}

Out[7]:

{% highlight shell linenos %}
# Apache Spark
{% endhighlight %}

Now let’s try a transformation. Use the filter transformation to return a new RDD with a subset of the items in the file. Type in this command:

In [8]:

{% highlight scala linenos %}
val linesWithSpark = readme.filter(line => line.contains("Spark"))
linesWithSpark.count()
{% endhighlight %}

Out[8]:

{% highlight shell linenos %}
18
{% endhighlight %}

Again, this returned a pointer to a RDD with the results of the filter transformation.

You can even chain together transformations and actions. To find out how many lines contains the word “Spark”, type in:

In [9]:

{% highlight scala linenos %}
readme.filter(line => line.contains("Spark")).count()
{% endhighlight %}

Out[9]:

{% highlight shell linenos %}
18
{% endhighlight %}

### More on RDD Operations

This section builds upon the previous section. In this section, you will see that RDD can be used for more complex computations. You will find the line from that readme file with the most words in it.

In [10]:

{% highlight scala linenos %}

readme.map(line => line.split(" ").size).
                    reduce((a, b) => if (a > b) a else b)
{% endhighlight %}

Out[10]:

{% highlight shell linenos %}
14
{% endhighlight %}

There are two parts to this. The first maps a line to an integer value, the number of words in that line. In the second part reduce is called to find the line with the most words in it. The arguments to map and reduce are Scala function literals (closures), but you can use any language feature or Scala/Java library.

In the next step, you use the Math.max() function to show that you can indeed use a Java library instead. Import in the java.lang.Math library:

In [11]:

{% highlight scala linenos %}
import java.lang.Math
{% endhighlight %}

Now run with the max function:

In [12]:

{% highlight scala linenos %}
readme.map(line => line.split(" ").size).
        reduce((a, b) => Math.max(a, b))
{% endhighlight %}

Out[12]:

{% highlight shell linenos %}
14
{% endhighlight %}

Spark has a MapReduce data flow pattern. We can use this to do a word count on the readme file.

In [13]:

{% highlight scala linenos %}
val wordCounts = readme.flatMap(line => line.split(" ")).
                        map(word => (word, 1)).
                        reduceByKey((a,b) => a + b)
{% endhighlight %}

Here we combined the flatMap, map, and the reduceByKey functions to do a word count of each word in the readme file.

To collect the word counts, use the collect action.

#### It should be noted that the collect function brings all of the data into the driver node. For a small dataset, this isacceptable but, for a large dataset this can cause an Out Of Memory error. It is recommended to use collect() for testing only. The safer approach is to use the take() function e.g. take(n).foreach(println)

In [14]:

{% highlight scala linenos %}
wordCounts.collect().foreach(println)
{% endhighlight %}

Out[14]:

{% highlight shell linenos %}
(package,1)
(this,1)
(Version"](http://spark.apache.org/docs/latest/building-spark.html#specifying-the-hadoop-version),1)
(Because,1)
... ...
{% endhighlight %}

You can also do:

println(wordCounts.collect().mkString("\n"))

println(wordCounts.collect().deep)

### YOUR TURN:

#### In the cell below, determine what is the most frequent CHARACTER in the README, and how many times was it used?

In [15]:

{% highlight scala linenos %}
// WRITE YOUR CODE BELOW
wordCounts.reduce((a, b) => if (a._2 > b._2) a else b)
{% endhighlight %}

Out[15]:

{% highlight shell linenos %}
("",67)
{% endhighlight %}

Highlight text for answer:

## Analysing a log file

First, let's analyze a log file in the current directory.

In [16]:

{% highlight scala linenos %}
val logFile = sc.textFile("/resources/LabData/notebook.log")
{% endhighlight %}

Filter out the lines that contains INFO (or ERROR, if the particular log has it)

In [17]:

{% highlight scala linenos %}
val info = logFile.filter(line => line.contains("INFO"))
{% endhighlight %}

Count the lines:

In [18]:

{% highlight scala linenos %}
info.count()
{% endhighlight %}

Out[18]:

{% highlight shell linenos %}
13438
{% endhighlight %}

Count the lines with Spark in it by combining transformation and action.

In [19]:

{% highlight scala linenos %}
info.filter(line => line.contains("spark")).count()
{% endhighlight %}

Out[19]:

{% highlight shell linenos %}
156
{% endhighlight %}

Fetch those lines as an array of Strings

In [20]:

{% highlight scala linenos %}
info.filter(line => line.contains("spark")).collect() foreach println
{% endhighlight %}

Out  [20]:

{% highlight shell linenos %}
15/10/14 14:29:23 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkDriver@172.17.0.22:53333]
15/10/14 14:29:23 INFO Utils: Successfully started service 'sparkDriver' on port 53333.
15/10/14 14:29:23 INFO DiskBlockManager: Created local directory at /tmp/spark-fe150378-7bad-42b6-876b-d14e2c193eb6/blockmgr-c142f2f1-ebb6-4612-945b-0a67d156230a
... ...
{% endhighlight %}

Remember that we went over the DAG. It is what provides the fault tolerance in Spark. Nodes can re-compute its state by borrowing the DAG from a neighboring node. You can view the graph of an RDD using the toDebugString command.

In [21]:

{% highlight scala linenos %}
println(info.toDebugString)
{% endhighlight %}

Out  [21]:

{% highlight shell linenos %}
(2) MapPartitionsRDD[12] at filter at <console>:26 []
 |  MapPartitionsRDD[11] at textFile at <console>:24 []
 |  /resources/LabData/notebook.log HadoopRDD[10] at textFile at <console>:24 []
{% endhighlight %}

## Joining RDDs

Next, you are going to create RDDs for the README and the POM file in the current directory.

In [22]:

{% highlight scala linenos %}
val readmeFile = sc.textFile("/resources/LabData/README.md")
val pom = sc.textFile("/resources/LabData/pom.xml")
{% endhighlight %}

How many Spark keywords are in each file?

In [23]:

{% highlight scala linenos %}
println(readmeFile.filter(line => line.contains("Spark")).count())
println(pom.filter(line => line.contains("Spark")).count())
{% endhighlight %}

Out  [23]:
{% highlight shell linenos %}
18
2
{% endhighlight %}

Now do a WordCount on each RDD so that the results are (K,V) pairs of (word,count)

In [24]:

{% highlight scala linenos %}
val readmeCount = readmeFile.
                    flatMap(line => line.split(" ")).
                    map(word => (word, 1)).
                    reduceByKey(_ + _)

val pomCount = pom.
                flatMap(line => line.split(" ")).
                map(word => (word, 1)).
                reduceByKey(_ + _)
{% endhighlight %}

To see the array for either of them, just call the collect function on it.

In [25]:

{% highlight scala linenos %}
println("Readme Count\n")
readmeCount.collect() foreach println
{% endhighlight %}

Out [25]:

{% highlight shell linenos %}
Readme Count

(package,1)
(this,1)
(Version"](http://spark.apache.org/docs/latest/building-spark.html#specifying-the-hadoop-version),1)
... ...
{% endhighlight %}

In [26]:

{% highlight scala linenos %}
println("Pom Count\n")
pomCount.collect() foreach println
{% endhighlight %}

Out [26]:

{% highlight shell linenos %}
Pom Count

(<id>kinesis-asl</id>,1)
(Unless,1)
(this,3)
(under,4)
... ...
{% endhighlight %}

Now let's join these two RDDs together to get a collective set. The join function combines the two datasets (K,V) and (K,W) together and get (K, (V,W)). Let's join these two counts together and then cache it.

In [27]:

{% highlight scala linenos %}
val joined = readmeCount.join(pomCount)
joined.cache()
{% endhighlight %}

Out[27]:

{% highlight shell linenos %}
MapPartitionsRDD[29] at join at <console>:32
{% endhighlight %}

Let's see what's in the joined RDD.

In [28]:

{% highlight scala linenos %}
joined.collect.foreach(println)
{% endhighlight %}

Out [28]:

{% highlight shell linenos %}
(file,(1,3))
(are,(1,1))
(Apache,(1,2))
(is,(6,2))
(uses,(1,1))
(this,(1,3))
(one,(2,1))
(with,(4,2))
(,(67,2931))
(The,(1,2))
(the,(21,10))
(not,(1,1))
... ...
{% endhighlight %}

Let's combine the values together to get the total count. The operations in this command tells Spark to combine the values from (K,V) and (K,W) to give us(K, V+W). The ._ notation is a way to access the value on that particular index of the key value pair.

In [29]:

{% highlight scala linenos %}
val joinedSum = joined.map(k => (k._1, (k._2)._1 + (k._2)._2))
joinedSum.collect() foreach println
{% endhighlight %}

Out [29]:

{% highlight shell linenos %}
(file,4)
(are,2)
(Apache,3)
(is,8)
(uses,2)
(this,4)
(one,3)
(with,6)
(,2998)
(The,3)
(the,31)
(not,2)
... ...
{% endhighlight %}

To check if it is correct, print the first five elements from the joined and the joinedSum RDD

In [30]:

{% highlight scala linenos %}
println("Joined Individial\n")
joined.take(5).foreach(println)

println("\n\nJoined Sum\n") 
joinedSum.take(5).foreach(println)
{% endhighlight %}

Out [30]:

{% highlight shell linenos %}
Joined Individial

(file,(1,3))
(are,(1,1))
(Apache,(1,2))
(is,(6,2))
(uses,(1,1))


Joined Sum

(file,4)
(are,2)
(Apache,3)
(is,8)
(uses,2)

{% endhighlight %}

## Shared variables

Broadcast variables allow the programmer to keep a read-only variable cached on each worker node rather than shipping a copy of it with tasks. They can be used, for example, to give every node a copy of a large input dataset in an efficient manner. After the broadcast variable is created, it should be used instead of the value v in any functions run on the cluster so that v is not shipped to the nodes more than once. In addition, the object v should not be modified after it is broadcast in order to ensure that all nodes get the same value of the broadcast variable (e.g. if the variable is shipped to a new node later).

Read more here: [http://spark.apache.org/docs/latest/programming-guide.html#broadcast-variables](http://spark.apache.org/docs/latest/programming-guide.html#broadcast-variables)

Let's create a broadcast variable:

In [31]:

{% highlight scala linenos %}
val broadcastVar = sc.broadcast(Array(1,2,3))
{% endhighlight %}

To get the value, type in:

In [32]:

{% highlight scala linenos %}
broadcastVar.value
{% endhighlight %}

Out[32]:

{% highlight shell linenos %}
Array(1, 2, 3)
{% endhighlight %}

Accumulators are variables that can only be added through an associative operation. It is used to implement counters and sum efficiently in parallel. Spark natively supports numeric type accumulators and standard mutable collections. Programmers can extend these for new types. Only the driver can read the values of the accumulators. The workers can only invoke it to increment the value.

Create the accumulator variable. Type in:

In [33]:

{% highlight scala linenos %}
val accum = sc.accumulator(0)
{% endhighlight %}

Next parallelize an array of four integers and run it through a loop to add each integer value to the accumulator variable. Type in:

In [34]:

{% highlight scala linenos %}
sc.parallelize(Array(1,2,3,4)).foreach(x => accum += x)
{% endhighlight %}

To get the current value of the accumulator variable, type in:

In [35]:

{% highlight scala linenos %}
accum.value
{% endhighlight %}

Out[35]:

{% highlight shell linenos %}
10
{% endhighlight %}

You should get a value of 10. This command can only be invoked on the driver side. The worker nodes can only increment the accumulator.

## Key-value pairs

You have already seen a bit about key-value pairs in the Joining RDD section. Here is a brief example of how to create a key-value pair and access its values. Remember that certain operations such as map and reduce only works on key-value pairs.

Create a key-value pair of two characters. Type in:

In [36]:

{% highlight scala linenos %}
val pair = ('a', 'b')
{% endhighlight %}

To access the value of the first index using the *._1* method and *._2*method for the 2nd.

In [37]:

{% highlight scala linenos %}
pair._1
{% endhighlight %}

Out[37]:

{% highlight shell linenos %}
a
{% endhighlight %}

In [38]:

{% highlight scala linenos %}
pair._2
{% endhighlight %}

Out[38]:

{% highlight shell linenos %}
b
{% endhighlight %}

## Sample Application

In this section, you will be using a subset of a data for taxi trips that will determine the top 10 medallion numbers based on the number of trips.

In [39]:

{% highlight scala linenos %}
val taxi = sc.textFile("/resources/LabData/nyctaxi.csv")
{% endhighlight %}

To view the five rows of content, invoke the take function. Type in:

In [40]:

{% highlight scala linenos %}
taxi.take(5).foreach(println)
{% endhighlight %}

{% highlight scala linenos %}
"_id","_rev","dropoff_datetime","dropoff_latitude","dropoff_longitude","hack_license","medallion","passenger_count","pickup_datetime","pickup_latitude","pickup_longitude","rate_code","store_and_fwd_flag","trip_distance","trip_time_in_secs","vendor_id"
"29b3f4a30dea6688d4c289c9672cb996","1-ddfdec8050c7ef4dc694eeeda6c4625e","2013-01-11 22:03:00",+4.07033460000000E+001,-7.40144200000000E+001,"A93D1F7F8998FFB75EEF477EB6077516","68BC16A99E915E44ADA7E639B4DD5F59",2,"2013-01-11 21:48:00",+4.06760670000000E+001,-7.39810790000000E+001,1,,+4.08000000000000E+000,900,"VTS"
"2a80cfaa425dcec0861e02ae44354500","1-b72234b58a7b0018a1ec5d2ea0797e32","2013-01-11 04:28:00",+4.08190960000000E+001,-7.39467470000000E+001,"64CE1B03FDE343BB8DFB512123A525A4","60150AA39B2F654ED6F0C3AF8174A48A",1,"2013-01-11 04:07:00",+4.07280540000000E+001,-7.40020370000000E+001,1,,+8.53000000000000E+000,1260,"VTS"
"29b3f4a30dea6688d4c289c96758d87e","1-387ec30eac5abda89d2abefdf947b2c1","2013-01-11 22:02:00",+4.07277180000000E+001,-7.39942860000000E+001,"2D73B0C44F1699C67AB8AE322433BDB7","6F907BC9A85B7034C8418A24A0A75489",5,"2013-01-11 21:46:00",+4.07577480000000E+001,-7.39649810000000E+001,1,,+3.01000000000000E+000,960,"VTS"
"2a80cfaa425dcec0861e02ae446226e4","1-aa8b16d6ae44ad906a46cc6581ffea50","2013-01-11 10:03:00",+4.07643050000000E+001,-7.39544600000000E+001,"E90018250F0A009433F03BD1E4A4CE53","1AFFD48CC07161DA651625B562FE4D06",5,"2013-01-11 09:44:00",+4.07308080000000E+001,-7.39928280000000E+001,1,,+3.64000000000000E+000,1140,"VTS"
{% endhighlight %}

Note that the first line is the headers. Normally, you would want to filter that out, but since it will not affect our results, we can leave it in.

To parse out the values, including the medallion numbers, you need to first create a new RDD by splitting the lines of the RDD using the comma as the delimiter. Type in:

In [41]:

{% highlight scala linenos %}
val taxiParse = taxi.map(line=>line.split(","))
{% endhighlight %}

Now create the key-value pairs where the key is the medallion number and the value is 1. We use this model to later sum up all the keys to find out the number of trips a particular taxi took and in particular, will be able to see which taxi took the most trips. Map each of the medallions to the value of one. Type in:

In [42]:

{% highlight scala linenos %}
val taxiMedKey = taxiParse.map(vals=>(vals(6), 1))
{% endhighlight %}

vals(6) corresponds to the column where the medallion key is located

Next use the reduceByKey function to count the number of occurrence for each key.

In [43]:

{% highlight scala linenos %}
val taxiMedCounts = taxiMedKey.reduceByKey((v1,v2)=>v1+v2)
taxiMedCounts.take(5).foreach(println)
{% endhighlight %}

Out [43]:

{% highlight shell linenos %}
("A9907052C8BBDED5079252EFE6177ECF",195)
("26DE3DC2FCBB37A233BE231BA6F7364E",173)
("BA60553FAA4FE1A36BBF77B4C10D3003",171)
("67DD83EA2A67933B2724269121BF45BB",196)
("AD57F6329C387766186E1B3838A9CEDD",214)
{% endhighlight %}

Finally, the values are swapped so they can be ordered in descending order and the results are presented correctly.

In [44]:

{% highlight scala linenos %}
for (pair <-taxiMedCounts.map(_.swap).top(10)) println("Taxi Medallion %s had %s Trips".format(pair._2, pair._1))
{% endhighlight %}

Out [44]:

{% highlight shell linenos %}
Taxi Medallion "FE4C521F3C1AC6F2598DEF00DDD43029" had 415 Trips
Taxi Medallion "F5BB809E7858A669C9A1E8A12A3CCF81" had 411 Trips
Taxi Medallion "8CE240F0796D072D5DCFE06A364FB5A0" had 406 Trips
Taxi Medallion "0310297769C8B049C0EA8E87C697F755" had 402 Trips
Taxi Medallion "B6585890F68EE02702F32DECDEABC2A8" had 399 Trips
Taxi Medallion "33955A2FCAF62C6E91A11AE97D96C99A" had 395 Trips
Taxi Medallion "4F7C132D3130970CFA892CC858F5ECB5" had 391 Trips
Taxi Medallion "78833E177D45E4BC520222FFBBAC5B77" had 383 Trips
Taxi Medallion "E097412FE23295A691BEEE56F28FB9E2" had 380 Trips
Taxi Medallion "C14289566BAAD9AEDD0751E5E9C73FBD" had 377 Trips
{% endhighlight %}

While each step above was processed one line at a time, you can just as well process everything on one line:

In [45]:

{% highlight scala linenos %}
val taxiMedCountsOneLine = taxi.map(line=>line.split(',')).map(vals=>(vals(6),1)).reduceByKey(_ + _)
{% endhighlight %}

Run the same line as above to print the taxiMedCountsOneLine RDD.

In [46]:

{% highlight scala linenos %}
for (pair <-taxiMedCountsOneLine.map(_.swap).top(10)) println("Taxi Medallion %s had %s Trips".format(pair._2, pair._1))
{% endhighlight %}

Out [46]:

{% highlight shell linenos %}
Taxi Medallion "FE4C521F3C1AC6F2598DEF00DDD43029" had 415 Trips
Taxi Medallion "F5BB809E7858A669C9A1E8A12A3CCF81" had 411 Trips
Taxi Medallion "8CE240F0796D072D5DCFE06A364FB5A0" had 406 Trips
Taxi Medallion "0310297769C8B049C0EA8E87C697F755" had 402 Trips
Taxi Medallion "B6585890F68EE02702F32DECDEABC2A8" had 399 Trips
Taxi Medallion "33955A2FCAF62C6E91A11AE97D96C99A" had 395 Trips
Taxi Medallion "4F7C132D3130970CFA892CC858F5ECB5" had 391 Trips
Taxi Medallion "78833E177D45E4BC520222FFBBAC5B77" had 383 Trips
Taxi Medallion "E097412FE23295A691BEEE56F28FB9E2" had 380 Trips
Taxi Medallion "C14289566BAAD9AEDD0751E5E9C73FBD" had 377 Trips
{% endhighlight %}

Let's cache the taxiMedCountsOneLine to see the difference caching makes. Run it with the logs set to INFO and you can see the output of the time it takes to execute each line. First, let's cache the RDD

In [47]:

{% highlight scala linenos %}
taxiMedCountsOneLine.cache()
{% endhighlight %}

Out[47]:

{% highlight shell linenos %}
ShuffledRDD[41] at reduceByKey at <console>:26
{% endhighlight %}

Next, you have to invoke an action for it to actually cache the RDD. Note the time it takes here (either empirically using the INFO log or just notice the time it takes)

In [48]:

{% highlight scala linenos %}
taxiMedCountsOneLine.count()
{% endhighlight %}

Out[48]:

{% highlight shell linenos %}
13464
{% endhighlight %}

Run it again to see the difference.

In [49]:

{% highlight scala linenos %}
taxiMedCountsOneLine.count()
{% endhighlight %}

Out[49]:

{% highlight shell linenos %}
13464
{% endhighlight %}

The bigger the dataset, the more noticeable the difference will be. In a sample file such as ours, the difference may be negligible.

### Summary

Having completed this exercise, you should now be able to describe Spark’s primary data abstraction, understand how to create parallelized collections and external datasets, work with Resilient Distributed Dataset (RDD) operations, and utilize shared variables and key-value pairs.

This notebook is part of the free course on **Cognitive Class** called *Spark Fundamentals I*. If you accessed this notebook outside the course, you can take this free self-paced course, online by going to: [https://cognitiveclass.ai/courses/what-is-spark/](https://cognitiveclass.ai/courses/what-is-spark/)

### About the Authors:

Hi! It's [Alex Aklson](https://www.linkedin.com/in/aklson/), one of the authors of this notebook. I hope you found this lab educational! There is much more to learn about Spark but you are well on your way. Feel free to connect with me if you have any questions.