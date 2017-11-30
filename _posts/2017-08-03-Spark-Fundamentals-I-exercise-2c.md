---
layout: post
title: Spark Fundamentals 1 - Introduction to Spark - Lab 2c
categories: Spark
tags: Spark
---

{% include toc.html html=content sanitize=true class="inline_toc" id="my_toc" h_min=2 h_max=3 %}

## Lab 2c. Python - Working with Dataframes

#### June 08, 2017 

**Related free online courses:**

Related courses can be found in the following learning paths:

- [Spark Fundamentals path](https://cognitiveclass.ai/learn/spark/)
- [Big Data Fundamentals path](https://cognitiveclass.ai/learn/big-data/)

A DataFrame is two-dimensional. Columns can be of different data types. DataFrames accept many data inputs including series and other DataFrames. You can pass indexes (row labels) and columns (column labels). Indexes can be numbers, dates, or strings/tuples.

<!-- excerpt -->

Pandas is a library used for data manipulation and analysis. Pandas offers data structures and operations for creating and manipulating Data Series and DataFrame objects. Data can be imported from various data sources, e.g., Numpy arrays, Python dictionaries and CSV files. Pandas allows you to manipulate, organize and display the data.

In this short notebook, we will load and explore the mtcars dataset. Specifically, this tutorial covers:

1. Loading data in memory
2. Creating SQLContext
3. Creating Spark DataFrame
4. Group data by columns
5. Operating on columns
6. Running SQL Queries from a Spark DataFrame

## Loading in a DataFrame

To create a Spark DataFrame we load an external DataFrame, called `mtcars`. This DataFrame includes 32 observations on 11 variables:

In [1]:

{% highlight pyhon linenos %}
import pandas as pd
mtcars = pd.read_csv('https://ibm.box.com/shared/static/f1dhhjnzjwxmy2c1ys2whvrgz05d1pui.csv')
{% endhighlight %}

In [2]:

{% highlight python linenos %}
mtcars.head()
{% endhighlight %}

Out[2]:

|      | car               | mpg  | cyl  | disp  | hp   | drat | wt    | qsec  | vs   | am   | gear | carb |
| ---- | ----------------- | ---- | ---- | ----- | ---- | ---- | ----- | ----- | ---- | ---- | ---- | ---- |
| 0    | Mazda RX4         | 21.0 | 6    | 160.0 | 110  | 3.90 | 2.620 | 16.46 | 0    | 1    | 4    | 4    |
| 1    | Mazda RX4 Wag     | 21.0 | 6    | 160.0 | 110  | 3.90 | 2.875 | 17.02 | 0    | 1    | 4    | 4    |
| 2    | Datsun 710        | 22.8 | 4    | 108.0 | 93   | 3.85 | 2.320 | 18.61 | 1    | 1    | 4    | 1    |
| 3    | Hornet 4 Drive    | 21.4 | 6    | 258.0 | 110  | 3.08 | 3.215 | 19.44 | 1    | 0    | 3    | 1    |
| 4    | Hornet Sportabout | 18.7 | 8    | 360.0 | 175  | 3.15 | 3.440 | 17.02 | 0    | 0    | 3    | 2    |

## Initialize SQLContext

To work with dataframes we need an SQLContext which is created using `SQLContext(sc)`. SQLContext uses SparkContext which has been already created in Data Scientist Workbench, named `sc`.

In [3]:

{% highlight python linenos %}
sqlContext = SQLContext(sc)
{% endhighlight %}

## Creating Spark DataFrames

With SQLContext and a loaded local DataFrame, we create a Spark DataFrame:

In [4]:

{% highlight python linenos %}
sdf = sqlContext.createDataFrame(mtcars) 
sdf.printSchema()
{% endhighlight %}

Out [4]:

{% highlight python linenos %}
root
 |-- car: string (nullable = true)
 |-- mpg: double (nullable = true)
 |-- cyl: long (nullable = true)
 |-- disp: double (nullable = true)
 |-- hp: long (nullable = true)
 |-- drat: double (nullable = true)
 |-- wt: double (nullable = true)
 |-- qsec: double (nullable = true)
 |-- vs: long (nullable = true)
 |-- am: long (nullable = true)
 |-- gear: long (nullable = true)
 |-- carb: long (nullable = true)
{% endhighlight %}

## Displays the content of the DataFrame

In [5]:

{% highlight python linenos %}
sdf.show(5)
{% endhighlight %}

Out [5]:

{% highlight python linenos %}
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
|              car| mpg|cyl| disp| hp|drat|   wt| qsec| vs| am|gear|carb|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
|        Mazda RX4|21.0|  6|160.0|110| 3.9| 2.62|16.46|  0|  1|   4|   4|
|    Mazda RX4 Wag|21.0|  6|160.0|110| 3.9|2.875|17.02|  0|  1|   4|   4|
|       Datsun 710|22.8|  4|108.0| 93|3.85| 2.32|18.61|  1|  1|   4|   1|
|   Hornet 4 Drive|21.4|  6|258.0|110|3.08|3.215|19.44|  1|  0|   3|   1|
|Hornet Sportabout|18.7|  8|360.0|175|3.15| 3.44|17.02|  0|  0|   3|   2|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
only showing top 5 rows
{% endhighlight %}

## Selecting columns

In [6]:

{% highlight python linenos %}
sdf.select('mpg').show(5)
{% endhighlight %}

Out [6]:

{% highlight python linenos %}
+----+
| mpg|
+----+
|21.0|
|21.0|
|22.8|
|21.4|
|18.7|
+----+
only showing top 5 rows
{% endhighlight %}

## Filtering Data

Filter the DataFrame to only retain rows with `mpg` less than 18

In [7]:

{% highlight python linenos %}
sdf.filter(sdf['mpg'] < 18).show(5)
{% endhighlight %}

Out [7]:

{% highlight python linenos %}
+-----------+----+---+-----+---+----+----+-----+---+---+----+----+
|        car| mpg|cyl| disp| hp|drat|  wt| qsec| vs| am|gear|carb|
+-----------+----+---+-----+---+----+----+-----+---+---+----+----+
| Duster 360|14.3|  8|360.0|245|3.21|3.57|15.84|  0|  0|   3|   4|
|  Merc 280C|17.8|  6|167.6|123|3.92|3.44| 18.9|  1|  0|   4|   4|
| Merc 450SE|16.4|  8|275.8|180|3.07|4.07| 17.4|  0|  0|   3|   3|
| Merc 450SL|17.3|  8|275.8|180|3.07|3.73| 17.6|  0|  0|   3|   3|
|Merc 450SLC|15.2|  8|275.8|180|3.07|3.78| 18.0|  0|  0|   3|   3|
+-----------+----+---+-----+---+----+----+-----+---+---+----+----+
only showing top 5 rows
{% endhighlight %}

## Operating on Columns

SparkR also provides a number of functions that can be directly applied to columns for data processing and aggregation. The example below shows the use of basic arithmetic functions to convert lb to metric ton.

In [8]:

{% highlight python linenos %}
sdf.withColumn('wtTon', sdf['wt'] * 0.45).show(6)
{% endhighlight %}

Out [8]:

{% highlight python linenos %}
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+-------+
|              car| mpg|cyl| disp| hp|drat|   wt| qsec| vs| am|gear|carb|  wtTon|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+-------+
|        Mazda RX4|21.0|  6|160.0|110| 3.9| 2.62|16.46|  0|  1|   4|   4|  1.179|
|    Mazda RX4 Wag|21.0|  6|160.0|110| 3.9|2.875|17.02|  0|  1|   4|   4|1.29375|
|       Datsun 710|22.8|  4|108.0| 93|3.85| 2.32|18.61|  1|  1|   4|   1|  1.044|
|   Hornet 4 Drive|21.4|  6|258.0|110|3.08|3.215|19.44|  1|  0|   3|   1|1.44675|
|Hornet Sportabout|18.7|  8|360.0|175|3.15| 3.44|17.02|  0|  0|   3|   2|  1.548|
|          Valiant|18.1|  6|225.0|105|2.76| 3.46|20.22|  1|  0|   3|   1|  1.557|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+-------+
only showing top 6 rows
{% endhighlight %}

In [9]:

{% highlight python linenos %}
sdf.show(6)
{% endhighlight %}

{% highlight python linenos %}
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
|              car| mpg|cyl| disp| hp|drat|   wt| qsec| vs| am|gear|carb|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
|        Mazda RX4|21.0|  6|160.0|110| 3.9| 2.62|16.46|  0|  1|   4|   4|
|    Mazda RX4 Wag|21.0|  6|160.0|110| 3.9|2.875|17.02|  0|  1|   4|   4|
|       Datsun 710|22.8|  4|108.0| 93|3.85| 2.32|18.61|  1|  1|   4|   1|
|   Hornet 4 Drive|21.4|  6|258.0|110|3.08|3.215|19.44|  1|  0|   3|   1|
|Hornet Sportabout|18.7|  8|360.0|175|3.15| 3.44|17.02|  0|  0|   3|   2|
|          Valiant|18.1|  6|225.0|105|2.76| 3.46|20.22|  1|  0|   3|   1|
+-----------------+----+---+-----+---+----+-----+-----+---+---+----+----+
only showing top 6 rows
{% endhighlight %}

## Grouping, Aggregation

Spark DataFrames support a number of commonly used functions to aggregate data after grouping. For example we can compute the average weight of cars by their cylinders as shown below:

In [10]:

{% highlight python linenos %}
sdf.groupby(['cyl'])\
.agg({"wt": "AVG"})\
.show(5)
{% endhighlight %}

Out [10]:

{% highlight python linenos %}
+---+-----------------+
|cyl|          avg(wt)|
+---+-----------------+
|  4|2.285727272727273|
|  6|3.117142857142857|
|  8|3.999214285714286|
+---+-----------------+

{% endhighlight %}

In [11]:

{% highlight python linenos %}
# We can also sort the output from the aggregation to get the most common cars
car_counts = sdf.groupby(['cyl'])\
.agg({"wt": "count"})\
.sort("count(wt)", ascending=False)\
.show(5)
{% endhighlight %}

Out [11]:

{% highlight python linenos %}
+---+---------+
|cyl|count(wt)|
+---+---------+
|  8|       14|
|  4|       11|
|  6|        7|
+---+---------+
{% endhighlight %}

### Running SQL Queries from Spark DataFrames

A Spark DataFrame can also be registered as a temporary table in Spark SQL and registering a DataFrame as a table allows you to run SQL queries over its data. The `sql` function enables applications to run SQL queries programmatically and returns the result as a DataFrame.

In [12]:

{% highlight python linenos %}
# Register this DataFrame as a table.
sdf.registerTempTable("cars")

# SQL statements can be run by using the sql method
highgearcars = sqlContext.sql("SELECT gear FROM cars WHERE cyl >= 4 AND cyl <= 9")
highgearcars.show(6)
{% endhighlight %}

Out [12]:

{% highlight python linenos %}
+----+
|gear|
+----+
|   4|
|   4|
|   4|
|   3|
|   3|
|   3|
+----+
only showing top 6 rows
{% endhighlight %}

NOTE: This tutorial draws heavily from the original [Spark Quick Start Guide](http://spark.apache.org/docs/latest/quick-start.html)

### Summary

Having completed this exercise, you should now be able to load data in memory, create SQLContext, create Spark DataFrame, group data by columns, and run SQL Queries from a Spark dataframe.

This notebook is part of the free course on **Cognitive Class** called *Spark Fundamentals I*. If you accessed this notebook outside the course, you can take this free self-paced course, online by going to: [https://cognitiveclass.ai/courses/what-is-spark/](https://cognitiveclass.ai/courses/what-is-spark/)

### About the Authors:

Hi! It's [Alex Aklson](https://www.linkedin.com/in/aklson/), one of the authors of this notebook. I hope you found this lab educational! There is much more to learn about Spark but you are well on your way. Feel free to connect with me if you have any questions.