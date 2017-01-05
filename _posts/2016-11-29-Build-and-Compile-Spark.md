---
layout: post
title: Compile Spark and Build Spark Package
categories:  Spark
tags: Spark
---

# Compile Spark and Build Spark Package

[TOC]

## Compile Spark and Build Spark Package

### Download Spark source code from git

 http://spark.apache.org/downloads.html

```shell
 yum install git

 # Master development branch
 git clone git://github.com/apache/spark.git

 # 1.6 maintenance branch with stability fixes on top of Spark 1.6.1
 git clone git://github.com/apache/spark.git -b branch-1.6
```

### Configure Maven Path into Enviroment

```shell
 export MAVEN_HOME=/app/compiled/spark/build/apache-maven-3.3.9
 export PATH=$PATH:$MAVEN_HOME/bin
```

###Compile Spark with Maven

Apache Spark document link  http://spark.apache.org/docs/1.6.2/building-spark.html

If using build/mvn with no MAVEN_OPTS set, the script will automate this for you.
```shell
export MAVEN_OPTS="-Xmx2g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m"`
```

If you want to read from HDFS
```shell
build/mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package
```

### Fix the build error

To see the full stack trace of the errors, re-run Maven with the -e switch
```shell
build/mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package -e
```

The following  issue gone after reboot the machine
```shell
 [error] Required file not found: sbt-interface.jar
 [error] See zinc -help for information about locating necessary files
```


### Build a Runnable Distribution
```shell
./make-distribution.sh --name 2.4.0 --tgz -Phadoop-2.4
```

