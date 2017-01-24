---
layout: post
title: VM Environment Setup
categories:  Spark
tags: Spark
---

### Add Hadoop User and Groups

Execute the following commands

{% highlight bash linenos %}
mkdir -p /myhome/hadoop
groupadd -g 1000 hadoop
useradd -u 2000 -g hadoop -d /myhome/hadoop hadoop
chown -R hadoop:hadoop /myhome/hadoop
passwd hadoop
{% endhighlight %}

Check the group and user are correctly created
{% highlight bash linenos %}
tail /etc/group
tail /etc/passwd
groups hadoop
id hadoop
{% endhighlight %}

Edit the home directroy of user
{% highlight bash linenos %}
usermod -d /myhome/hadoop hadoop
{% endhighlight %}

Prepare the app and upload directories for hadoop
{% highlight bash linenos %}
mkdir -p /app/hadoop
chown -R hadoop:hadoop /app/hadoop
mkdir /myhome/hadoop/upload
chown -R hadoop:hadoop /myhome/hadoop/upload
{% endhighlight %}

Grant sudo permission to hadoop
{% highlight bash linenos %}
chmod u+w /etc/sudoers
{% endhighlight %}

Edit /etc/sudoers to add the following lines
{% highlight bash linenos %}
hadoop  ALL=(ALL)       NOPASSWD: ALL
{% endhighlight %}



### Install and config JDK

Add the following environment under `/etc/profile`

{% highlight bash linenos %}
 export JAVA_HOME=/pcc/app/Linux_jdk1.7.0_x86_64
 export PATH=$JAVA_HOME/bin:$PATH
 export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
{% endhighlight %}

Verify after re-logon
{% highlight bash linenos %}
 java -version
{% endhighlight %}



### Install and config Scala

{% highlight bash linenos %}
wget http://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz /myhome/hadoop/upload
wget http://downloads.lightbend.com/scala/2.10.6/scala-2.10.6.tgz /myhome/hadoop/upload
tar -zxvf scala-2.10.6.tgz -C /app
{% endhighlight %}

Edit `/etc/profile` with SCALA configuration
{% highlight bash linenos %}
export SCALA_HOME=/app/scala-2.10.6
export PATH=$PATH:${SCALA_HOME}/bin
{% endhighlight %}

Verify after re-logon
{% highlight bash linenos %}
scala -version
{% endhighlight %}


### Configure SSH logon without password
{% highlight bash linenos %}
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server_name
{% endhighlight %}



### Show [user@hostname ~]  when su user

{% highlight bash linenos %}
 cp /root/.bashrc /myhome/hadoop/
 [hadoop@qilibjtst3 ~]$ cat .bashrc
 # .bashrc

 # User specific aliases and functions

 alias rm='rm -i'
 alias cp='cp -i'
 alias mv='mv -i'

 # Source global definitions
 if [ -f /etc/bashrc ]; then
         . /etc/bashrc

 fi
{% endhighlight %}

{% highlight bash linenos %}
 cp /root/.bash_profile /myhome/hadoop/
 [hadoop@qilibjtst3 ~]$ cat .bash_profile
 # .bash_profile

 # Get the aliases and functions
 if [ -f ~/.bashrc ]; then
         . ~/.bashrc
 fi

 # User specific environment and startup programs

 PATH=$PATH:$HOME/bin

 export PATH
{% endhighlight %}

Change the file permission after copy done
{% highlight bash linenos %}
chown hadoop:hadoop /myhome/hadoop/.*
{% endhighlight %}

