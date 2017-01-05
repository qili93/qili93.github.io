---
layout: post
title: VM Environment Setup
categories:  Spark
tags: Spark
---

# VM Environment Setup

[TOC]

## VM Environment Setup

### Add Hadoop User and Groups

Execute the following commands

```Shell
mkdir -p /myhome/hadoop
groupadd -g 1000 hadoop
useradd -u 2000 -g hadoop -d /myhome/hadoop hadoop
chown -R hadoop:hadoop /myhome/hadoop
passwd hadoop
```

Check the group and user are correctly created
```Shell
tail /etc/group
tail /etc/passwd
groups hadoop
id hadoop
```

Edit the home directroy of user
```Shell
usermod -d /myhome/hadoop hadoop
```

Prepare the app and upload directories for hadoop
```shell
mkdir -p /app/hadoop
chown -R hadoop:hadoop /app/hadoop
mkdir /myhome/hadoop/upload
chown -R hadoop:hadoop /myhome/hadoop/upload
```

Grant sudo permission to hadoop
```shell
chmod u+w /etc/sudoers
```

Edit /etc/sudoers to add the following lines
```Shell
hadoop  ALL=(ALL)       NOPASSWD: ALL
```



### Install and config JDK

Add the following environment under `/etc/profile`

```shell
 export JAVA_HOME=/pcc/app/Linux_jdk1.7.0_x86_64
 export PATH=$JAVA_HOME/bin:$PATH
 export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

Verify after re-logon
```shell
 java -version
```



### Install and config Scala

```shell
wget http://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz /myhome/hadoop/upload
wget http://downloads.lightbend.com/scala/2.10.6/scala-2.10.6.tgz /myhome/hadoop/upload
tar -zxvf scala-2.10.6.tgz -C /app
```

Edit `/etc/profile` with SCALA configuration
```shell
export SCALA_HOME=/app/scala-2.10.6
export PATH=$PATH:${SCALA_HOME}/bin
```

Verify after re-logon
```shell
scala -version
```


### Configure SSH logon without password
```shell
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server_name
```



### Show [user@hostname ~]  when su user

```shell
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
```

```shell
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
```

Change the file permission after copy done
```shell
chown hadoop:hadoop /myhome/hadoop/.*
```

