---
layout: post
title: Spark Web GUI to Check App Status
categories:  Spark
tags: Spark
---

Refer to http://spark.apache.org/docs/latest/submitting-applications.html

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
