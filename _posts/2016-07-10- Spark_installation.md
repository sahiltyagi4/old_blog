---
layout:     post
title:      Apache Spark-2.0.0 setup and Installation
date:       2016-07-10 11:21:29
summary:    This post describes how to setup the latest Spark-2.0.0 on an Ubuntu (Linux) server. 
categories: spark RDDs Datasets
---

This post describes on how to setup the latest version of Apache Spark v2.0.0 on an Ubuntu machine. Apache Spark is an execution engine that supports cyclic data flow and performs distributed in-memory computing. Spark has been demonstrated to be around 100x faster than Hadoop in memory and around 10x faster on disk. Various APIs are available for writing application in Java, Scala, Python and R. Spark is not just limited to batch processing, but also provides real-time streaming, combines SQL, a machine learning library (MLlib) and other complex analytics. And it also runs on various tools and platforms as well, including Hadoop (HDFS), Mesos, standalone or on cloud.

<b>Please make sure you have Apache Maven installed and configured on your system!</b>

So let's get started with the installation!

- Create a new directory in <b>/home/spark</b>, and move into that directory
	
	```
	mkdir /home/spark
	```

	```
	cd /home/spark
	```

- Download the latest spark v2.0.0 tar.gz file with the following command:

	```
	wget http://d3kbcqa49mib13.cloudfront.net/spark-2.0.0-bin-hadoop2.7.tgz
	```

- This will create the file <b>spark-2.0.0-bin-hadoop2.7.tgz</b> in the current directory. Now untar the compressed file as:

	```
	tar -xzvf spark-2.0.0-bin-hadoop2.7.tgz
	```

	This will create the file <b>spark-2.0.0-bin-hadoop2.7</b> in the the directory.

- Now build the spark project with maven as:

	```
	mvn -Pyarn -Phadoop-2.4 -Dhadoop.version=2.4.0 -DskipTests clean package
	```

	If the above step completes successfully, then you have installed spark on your system. Congratulations!