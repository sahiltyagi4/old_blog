---
layout:     post
title:      Hadoop and HDFS setup
date:       2016-07-09 11:21:29
summary:    This post describes how to setup Hadoop 2.2.3 (Hortonworks release) on an Ubuntu (Linux) server. 
categories: hadoop hdfs
---

Hadoop, formally called Apache Hadoop, is an Apache Software Foundation project and open source software platform for scalable, distributed computing.
Following are the installation steps:

- Check the ip of the server by running the command:
	
	``` 
	$ ifconfig
	``` 

	Let's say the ip looks somewhat like xxx.xxx.xx.xxx

- Check the hostname provided by Hetzner by running nslookup on the remote machine:

	```
	$ nslookup xxx.xxx.xx.xxx
	```

- SSH to the server from your local machine:
	
	```
	$ ssh root@xxx.xxx.xx.xxx
	```

You should see something like the following after entering credentials:
	root@Ubuntu-1204-precise-64-minimal ~ 

- Edit /etc/hostname :
	 
	 ```
	 vim /etc/hostname
	 ```

- Change all instances of the current  hostname (Ubuntu-1204-precise-64-minimal) to the Hostname provided by Hetzner and save the file.
	If everything goes well, you should see an entry in /etc/hosts like the one shown below.
	xxx.xxx.xx.xxx 	static.xxx.xxx.xx.xxx.clients.your-server.de

	If you don’t see such an entry, add it manually.

- Edit <b>/etc/hosts</b> :
	
	```
	$ vim /etc/hosts
	```

	(If its empty just enter machine name)

- Again change the hostname with $ vim /etc/hostname command

- Reboot the machine:
	
	```
	$ sudo reboot
	```

- Download the .tar.gz file of the latest JDK from <b>http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.htm
	lwget --no-cookies --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com" "http://download.oracle.com/otn-pub/java/jdk/7u51-b13/jdk-7u51-linux-x64.tar.gz"</b>

	<b>UPDATE</b> : 

	<b>NOTE-1</b> : The version mentioned here is not the latest version of java. If you want to download the latest version, get the appropriate .tar.gz url. If you want the 7U51 version (used in the document) get the new link from archives page.

	<b>NOTE-2</b> : if the wget command used above doesn't work, use <b>wget --no-check-certificate --no-cookies --header "Cookie:oraclelicense=accept-securebackup-cookie"</b> instead of 
	<b>wget --no-cookies --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com"</b> on your local machine.

- Create a folder for the java setup on the remote machine:

	```
	$ mkdir /usr/local/java
	$ cd /usr/local/java
	```

- Copy the java setup to remote machine by running the following command on your local machine :
	
	```
	$ scp jdk-7u51-linux-x64.tar.gz root@148.xxx.xx.135:/usr/local/java/
	```

- Run the following command (replace jdk-7u51 with the corresponding version name) on the remote machine:
	
	```
	$ cd /usr/local/java
	$ sudo tar -xvzf jdk-7u51-linux-x64.tar.gz
	```

- Check the name of the extracted folder (marked in bold, this may differ for you, and you need to use that particular name in the steps that follow):
	
	```
	root@rq-namenode /usr/local/java  ls -a
		.  ..  jdk1.7.0_51  jdk-7u51-linux-x64.tar.gz
	```	

- (Optional) At this point, you can remove the .tar.gz file
	
	```
	rm jdk-7u51-linux-x64.tar.gz
	```

- $ <b>vim /etc/profile</b>

- Add the following lines at the end of the file, and save the file (Esc :wq):
	
	```
	JAVA_HOME=/usr/local/java/jdk1.7.0_51
	PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
	export JAVA_HOME
	export PATH
	```

- Inform the system where your Oracle Java JDK is located. This will tell the system that the new Oracle Java version is available for use.

	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.7.0_79/bin/java" 1
	```

	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.7.0_51/bin/javac" 1
	```

	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jdk1.7.0_51/bin/javaws" 1
	```

- Inform the system that Oracle Java JDK must be the default Java.
	
	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --set java /usr/local/java/jdk1.7.0_79/bin/java
	```

	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --set javac /usr/local/java/jdk1.7.0_51/bin/javac
	```

	```
	root@rq-namenode /usr/local/java  sudo update-alternatives --set javaws /usr/local/java/jdk1.7.0_51/bin/javaws
	```

- Reload your system wide PATH <b>/etc/profile</b> :

	```
	root@rq-namenode /usr/local/java  . /etc/profile
	```

- Check that java was installed correctly:

	```
	root@rq-namenode ~  java -version
		java version "1.7.0_51"
		Java(TM) SE Runtime Environment (build 1.7.0_51-b13)
		Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)
	```

	```
	root@rq-namenode ~  javac -version
		javac 1.7.0_51
	```

- Install NTP (if not already installed):
	
	```
	root@rq-namenode ~  apt-get install ntp
	```

- <b>sudo /etc/init.d/ntp reload</b>

- Disable SELinux:
	 
	 ```
	 apt-get install selinux-utils
	 getenforce
	 ```

	If result is disabled or permissive, no further action is required. Otherwise
	 
	 ```
	 setenforce 0
	 ```

- Disable IPTables:
	 
	 ```
	 service ufw stop
	 ```

- Install requirements:
	 
	 ```
	 sudo apt-get install rpm
	 sudo apt-get install curl
	 ```

- Configure remote repositories and add gpg keys:
	
	```
	wget http://public-repo-1.hortonworks.com/HDP/ubuntu12/2.x/hdp.list -O /etc/apt/sources.list.d/hdp.list
	```

	```
	wget http://public-repo-1.hortonworks.com/HDP/ubuntu12/2.0.6.1/hdp.list -O /etc/apt/sources.list.d/hdp.list
	```

	```
	 gpg --keyserver pgp.mit.edu --recv-keys B9733A7A07513CAD
	 gpg -a --export 07513CAD | apt-key add -
	 apt-get update
	 ```

- Download companion files
	 
	 ```
	 mkdir /tmp/hadoop
	 cd /tmp/hadoop
	 wget http://public-repo-1.hortonworks.com/HDP/tools/2.1.5.0/hdp_manual_install_rpm_helper_files-2.1.5.695.tar.gz
	 tar -zxvf hdp_manual_install_rpm_helper_files-2.0.6.101.tar.gz
	 cd hdp_manual_install_rpm_helper_files-2.0.6.101/
	 ```

-  <b>cd scripts</b>
	
	```
	vim usersAndGroups.sh
	```

	Comment the variables (using ) corresponding to the services that are not being installed (from PIG_USER to OOZIE_USER) and save the file (Esc :wq)
	 
	```
	source usersAndGroups.sh
	```

- <b>vim directories.sh</b>
	
		#Set the following variables and comment the rest:

		DFS_NAME_DIR="/grid/hadoop/hdfs/nn";

		DFS_DATA_DIR="/grid/hadoop/hdfs/dn";

		FS_CHECKPOINT_DIR="/grid/hadoop/hdfs/snn";

		HDFS_LOG_DIR="/var/log/hadoop/hdfs";

		HDFS_PID_DIR="/var/run/hadoop/hdfs";
	
		HADOOP_CONF_DIR="/etc/hadoop/conf";

		YARN_LOCAL_DIR="/grid/hadoop/yarn/local";

		YARN_LOG_DIR="/var/log/hadoop/yarn";

		YARN_LOCAL_LOG_DIR="/grid/hadoop/yarn/logs";

		YARN_PID_DIR="/var/run/hadoop/yarn";

		MAPRED_LOG_DIR="/var/log/hadoop/mapred";

		MAPRED_PID_DIR="/var/run/hadoop/mapred";

		ZOOKEEPER_DATA_DIR="/grid/hadoop/zookeeper/data";

		ZOOKEEPER_CONF_DIR="/etc/zookeeper/conf";

		ZOOKEEPER_LOG_DIR="/var/log/zookeeper";

		ZOOKEEPER_PID_DIR="/var/run/zookeeper";

		SQOOP_CONF_DIR="/etc/sqoop/conf";

		export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec

	Save the file and source it.

	 <b>source directories.sh</b>

-  <b>vim ~/hadoopVariables.sh</b>

	#Add the following
	
		!/bin/sh

		#Hadoop Users and Groups
		#User which will own the HDFS services.
		HDFS_USER=hdfs ;

		#User which will own the YARN services.
		YARN_USER=yarn ;

		#User which will own the MapReduce services.
		MAPRED_USER=mapred ;

		#set owning the ZooKeeper services.
		ZOOKEEPER_USER=zookeeper ;

		#A common group shared by services.
		HADOOP_GROUP=hadoop ;

		#Hadoop Service - HDFS
		#Space separated list of directories where NameNode will store file system image. For example, /grid/hadoop/hdfs/nn /grid1/hadoop/hdfs/nn
	
		DFS_NAME_DIR="/grid/hadoop/hdfs/nn";

		#Space separated list of directories where DataNodes will store the blocks. For example, /grid/hadoop/hdfs/dn /grid1/hadoop/hdfs/dn /grid2/hadoop/hdfs/dn
	
		DFS_DATA_DIR="/grid/hadoop/hdfs/dn";

		#Space separated list of directories where SecondaryNameNode will store checkpoint image. For example, /grid/hadoop/hdfs/snn /grid1/hadoop/hdfs/snn /grid2/hadoop/hdfs/snn
	
		FS_CHECKPOINT_DIR="/grid/hadoop/hdfs/snn";

		#Directory to store the HDFS logs.
		HDFS_LOG_DIR="/var/log/hadoop/hdfs";

		#Directory to store the HDFS process ID.
		HDFS_PID_DIR="/var/run/hadoop/hdfs";

		#Directory to store the Hadoop configuration files.
		HADOOP_CONF_DIR="/etc/hadoop/conf";

		#Hadoop Service - YARN 
		#Space separated list of directories where YARN will store temporary data. For example, /grid/hadoop/yarn/local /grid1/hadoop/yarn/local /grid2/hadoop/yarn/local
	
		YARN_LOCAL_DIR="/grid/hadoop/yarn/local";

		#Directory to store the YARN logs.
		YARN_LOG_DIR="/var/log/hadoop/yarn"; 

		#Space separated list of directories where YARN will store container log data. For example, /grid/hadoop/yarn/logs /grid1/hadoop/yarn/logs /grid2/hadoop/yarn/logs
	
		YARN_LOCAL_LOG_DIR="/grid/hadoop/yarn/logs";

		#Directory to store the YARN process ID.
		YARN_PID_DIR="/var/run/hadoop/yarn";

		#Hadoop Service - MAPREDUCE
		#Directory to store the MapReduce daemon logs.
		MAPRED_LOG_DIR="/var/log/hadoop/mapred";

		#Directory to store the mapreduce jobhistory process ID.
		MAPRED_PID_DIR="/var/run/hadoop/mapred";

		#Hadoop Service - ZOOKEEPER
		#Directory where ZooKeeper will store data
		ZOOKEEPER_DATA_DIR="/grid/hadoop/zookeeper/data";

		#Directory to store the ZooKeeper configuration files.
		ZOOKEEPER_CONF_DIR="/etc/zookeeper/conf";

		#Directory to store the ZooKeeper logs.
		ZOOKEEPER_LOG_DIR="/var/log/zookeeper";
	
		#Directory to store the ZooKeeper process ID.
		ZOOKEEPER_PID_DIR="/var/run/zookeeper";
	
		export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec

- <b>chmod +x hadoopVariables.sh</b>
	 
	 ```
	 source hadoopVariables.sh
	 ```

- ```
	python yarn-utils.py -c 6 -m 128 -d 1 -k False
	```

	Replace 6 by no.of cores, 128 by no. of gigabytes of RAM and 1 by no. of disks.Save the ouput for use in step 42 (yarn-site.xml)

- <b>umask</b>
	0022

- ```
	sudo apt-get install openssh-server
	```

- Install hadoop core services:
	 
	 ```
	 sudo apt-get install hadoop hadoop-hdfs libhdfs0 libhdfs0-dev hadoop-yarn hadoop-mapreduce hadoop-client openssl
	 ```

- Install compression libraries
	
	```
	apt-get install libsnappy1 libsnappy-dev
	```

	```
	ln -sf /usr/lib64/libsnappy.so /usr/lib/hadoop/lib/native/.
	```

	```
	apt-get install liblzo2-2 liblzo2-dev hadoop-lzo
	```

- Create the NameNode Directories (ONLY on NameNode):
	
		mkdir -p $DFS_NAME_DIR;
		chown -R $HDFS_USER:$HADOOP_GROUP $DFS_NAME_DIR;
		chmod -R 755 $DFS_NAME_DIR;

- Create the Secondary NameNode Directories (ONLY on SecondaryNameNode):

		mkdir -p $FS_CHECKPOINT_DIR;
		chown -R $HDFS_USER:$HADOOP_GROUP $FS_CHECKPOINT_DIR;
		chmod -R 755 $FS_CHECKPOINT_DIR;

- ONLY on all the datanodes :
	
		mkdir -p $DFS_DATA_DIR;
		chown -R $HDFS_USER:$HADOOP_GROUP $DFS_DATA_DIR;
		chmod -R 750 $DFS_DATA_DIR;

- Need to check about Zookeeper node:
	
		mkdir -p $ZOOKEEPER_LOG_DIR;
		chown -R $ZOOKEEPER_USER:$HADOOP_GROUP $ZOOKEEPER_LOG_DIR;
		chmod -R 755 $ZOOKEEPER_LOG_DIR;
		mkdir -p $ZOOKEEPER_PID_DIR;
		chown -R $ZOOKEEPER_USER:$HADOOP_GROUP $ZOOKEEPER_PID_DIR;
		chmod -R 755 $ZOOKEEPER_PID_DIR;
		mkdir -p $ZOOKEEPER_DATA_DIR;
		chmod -R 755 $ZOOKEEPER_DATA_DIR;
		chown -R $ZOOKEEPER_USER:$HADOOP_GROUP $ZOOKEEPER_DATA_DIR;

- ONLY on ResourceManager and all datanodes :

		mkdir -p $YARN_LOCAL_DIR;
		chown -R $YARN_USER:$HADOOP_GROUP $YARN_LOCAL_DIR;
		chmod -R 755 $YARN_LOCAL_DIR;
		mkdir -p $YARN_LOCAL_LOG_DIR;
		chown -R $YARN_USER:$HADOOP_GROUP $YARN_LOCAL_LOG_DIR;
		chmod -R 755 $YARN_LOCAL_LOG_DIR;

- On all nodes:

		mkdir -p $HDFS_LOG_DIR;
		chown -R $HDFS_USER:$HADOOP_GROUP $HDFS_LOG_DIR;
		chmod -R 755 $HDFS_LOG_DIR;
		mkdir -p $YARN_LOG_DIR;
		chown -R $YARN_USER:$HADOOP_GROUP $YARN_LOG_DIR;
		chmod -R 755 $YARN_LOG_DIR;
		mkdir -p $HDFS_PID_DIR;
		chown -R $HDFS_USER:$HADOOP_GROUP $HDFS_PID_DIR;
		chmod -R 755 $HDFS_PID_DIR;
		mkdir -p $YARN_PID_DIR;
		chown -R $YARN_USER:$HADOOP_GROUP $YARN_PID_DIR;
		chmod -R 755 $YARN_PID_DIR;
		mkdir -p $MAPRED_LOG_DIR;
		chown -R $MAPRED_USER:$HADOOP_GROUP $MAPRED_LOG_DIR;
		chmod -R 755 $MAPRED_LOG_DIR;
		mkdir -p $MAPRED_PID_DIR;
		chown -R $MAPRED_USER:$HADOOP_GROUP $MAPRED_PID_DIR;
		chmod -R 755 $MAPRED_PID_DIR;

- <b>cd /tmp/hadoop/hdp_manual_install_rpm_helper_files-2.0.6.101/configuration_files/core_hadoop</b>
	
- <b>vim core-site.xml</b>
		
		<!--Make the following change-->
 	<property>
    	<name>fs.defaultFS</name>
 	<value>hdfs://static.xxx.xxx.xx.xxx.clients.your-server.de:8020</value>
 	</property>

- <b>vim hdfs-site.xml</b>
	
		<!--Make the following changes-->
		<property>
    	<name>dfs.datanode.data.dir</name>
    	<value>file:///grid/hadoop/hdfs/dn</value>
		</property>
		<property>
   	<name>dfs.namenode.http-address</name>
    	<value>static.xxx.xxx.xx.xxx.clients.your-server.de:50070</value>
		</property>
		<property>
    	<name>dfs.namenode.name.dir</name>
    	<value>file:///grid/hadoop/hdfs/nn</value>
		</property>
		<property>
    	<name>dfs.namenode.checkpoint.dir</name>
    	<value>/grid/hadoop/hdfs/snn</value>
    	</property>
		<property>
    	<name>dfs.namenode.secondary.http-address</name>
    	<value>static.xxx.xxx.xx.xxx.clients.your-server.de:50090</value>
		</property>

- <b>vim yarn-site.xml</b>
	
		<!--Make the following changes-->
    	<property>
    		<name>yarn.nodemanager.local-dirs</name>
    		<value>/grid/hadoop/yarn/local</value>
  		</property>
    	<property>
    		<name>yarn.log.server.url</name>
    		<value>http://static.xxx.xxx.xx.xxx.clients.your-server.de:19888/jobhistory/logs</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.admin.address</name>
    		<value>static.xxx.xxx.xx.xxx.clients.your-server.de:8141</value>
  		</property>
    	<property>
    		<name>yarn.scheduler.maximum-allocation-mb</name>
    		<value>105984</value>
  		</property>
    	<property>
    		<name>yarn.nodemanager.resource.memory-mb</name>
    		<value>105984</value>
  		</property>
    	<property>
    		<name>yarn.nodemanager.log-dirs</name>
    		<value>/grid/hadoop/yarn/logs</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.scheduler.class</name>
    		<value>org.apache.hadoop.yarn.server.resourcemanager.scheduler.capacity.CapacityScheduler</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.resource-tracker.address</name>
    		<value>static.xxx.xxx.xx.xxx.clients.your-server.de:8025</value>
  		</property>
    	<property>
    		<name>yarn.scheduler.minimum-allocation-mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.webapp.address</name>
    		<value>static.xxx.xxx.xx.xxx.clients.your-server.de:8088</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.address</name>
    		<value>static.xxx.xxx.xx.xxx.clients.your-server.de:8050</value>
  		</property>
    	<property>
    		<name>yarn.resourcemanager.scheduler.address</name>
    		<value>static.xxx.xxx.xx.xxx.clients.your-server.de:8030</value>
  		</property>

- <b>vim mapred-site.xml</b>

		<!--Make the following changes-->
    	<property>
    		<name>mapreduce.jobhistory.address</name>
    		<value>static.xxx.xx.xxx.xxx.clients.your-server.de:10020</value>
  		</property>
    	<property>
    		<name>mapreduce.cluster.reduce.memory.mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>mapred.child.java.opts</name>
    		<value>-Xmx28262m</value>
  		</property>
    	<property>
    		<name>mapred.cluster.max.reduce.memory.mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>mapred.cluster.max.map.memory.mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>mapreduce.jobhistory.webapp.address</name>
    		<value>static.xxx.xx.xxx.xxx.clients.your-server.de:19888</value>
  		</property>
    	<property>
    		<name>mapreduce.map.memory.mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>mapreduce.task.io.sort.mb</name> 
    		<!--Please keep this as 1024 -->
    		<value>1024</value>
  		</property>
    	<property>
    		<name>mapred.cluster.map.memory.mb</name>
    		<value>35328</value>
  		</property>
    	<property>
    		<name>mapreduce.reduce.memory.mb</name>
    		<value>35328</value>
  		</property>

- Copy the configuration files to <b>/etc/hadoop/conf/</b>

- <b>vim /etc/hadoop/conf/hadoop-env.sh</b>
	
	Comment the line containing export JAVA_HOME

- <b>vim /etc/rc.local</b>
	
		Add the following lines (after the first block of comments):
		mkdir /var/run/hadoop
		chown -R hdfs:hadoop /var/run/hadoop
		chmod -R 755 /var/run/hadoop

		mkdir /var/run/hadoop-yarn
		chown -R yarn:hadoop /var/run/hadoop-yarn
		chmod -R 755 /var/run/hadoop-yarn

		mkdir /var/run/hadoop-mapreduce
		chown -R mapred:hadoop /var/run/hadoop-mapreduce
		chmod -R 755 /var/run/hadoop-mapreduce

		mkdir /var/run/hbase
		chown -R hbase:hadoop /var/run/hbase
		chmod -R 755 /var/run/hbase

	(Need to use distributed cache) Copy all the lib jars from <b>/home/rq/rocq/sparq_backend/lib</b> to <b>/usr/lib/hadoop-mapreduce/</b>
	** add <b>/usr/lib/rocq</b> folder & <b>/usr/lib/hadoop-mapreduce/</b> folder jars while adding new datanodes or any other node to the cluster

- ONLY on the namenode, format and start the namenode
		
	  su $HDFS_USER
	  hdfs namenode -format

	 ```
	 /usr/lib/hadoop/sbin/hadoop-daemon.sh --config /etc/hadoop/conf start namenode
	```

	If the above commands execute properly, then the namenode setup is working.
	When namenode is running, see : http://static.136.xx.xxx.148.clients.your-server.de:50070/ 

- Execute these commands on the SecondaryNameNode:
	 
	  su $HDFS_USER
	 
	 ```
	 /usr/lib/hadoop/sbin/hadoop-daemon.sh --config /etc/hadoop/conf start secondarynamenode
	 ```

- Execute these commands on all DataNodes:
	  
	  su $HDFS_USER
	 
	 ```
	 /usr/lib/hadoop/sbin/hadoop-daemon.sh --config /etc/hadoop/conf start datanode
	 ```

- Create hdfs user directory in HDFS:
	  
	  su $HDFS_USER
	  hadoop fs -mkdir -p /user/hdfs

	Try copying a file into HDFS and listing that file:
	  
	  su $HDFS_USER
	  hadoop fs -copyFromLocal /etc/passwd passwd
	  hadoop fs -ls 
	
	If  you see the file listed, then it has been created properly.

- Start YARN
	Execute these commands from the ResourceManager server:
	  
	  su $YARN_USER
	  export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec
	  

	```
	/usr/lib/hadoop-yarn/sbin/yarn-daemon.sh --config /etc/hadoop/conf start resourcemanager
	```


	Execute these commands from all NodeManager nodes:
	  
	  su $YARN_USER
	  export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec
	 
	 ```
	 /usr/lib/hadoop-yarn/sbin/yarn-daemon.sh --config /etc/hadoop/conf start nodemanager
	 ```

- Start MapReduce JobHistory Server (on the namenode):
	We need to do this step on data-nodes before starting node manager, else it shows error on health check and shows unhealthy nodes.
	 
	  chown -R root:hadoop /usr/lib/hadoop-yarn/bin/container-executor
	  chmod -R 6050 /usr/lib/hadoop-yarn/bin/container-executor
		
	  $ su hdfs
	  $ hadoop fs -mkdir -p /mr-history/tmp
	  $ hadoop fs -chmod -R 1777 /mr-history/tmp
	  $ hadoop fs -mkdir -p /mr-history/done
	  $ hadoop fs -chmod -R 1777 /mr-history/done
	  $ hadoop fs -chown -R mapred:hdfs /mr-history
	  $ hadoop fs -mkdir -p /app-logs
	  $ hadoop fs -chmod -R 1777 /app-logs 
	  $ hadoop fs -chown yarn /app-logs
	  $ exit

	
	  $ su mapred
	  $ export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec/ 
	  $ /usr/lib/hadoop-mapreduce/sbin/mr-jobhistory-daemon.sh --config /etc/hadoop/conf start historyserver

- Browse the Recource Manager :
	
	  http://static.136.xx.xxx.148.clients.your-server.de:8088/

- Start a sample MapReduce Job:
	 
	  su hdfs
	  $ /usr/lib/hadoop/bin/hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples-2.2.0.2.0.6.0-101.jar teragen 10000 /tmp/teragenout 
	  $ /usr/lib/hadoop/bin/hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples-2.2.0.2.0.6.0-76.jar terasort /tmp/teragenout /tmp/terasortout

- If there are no errors, then the setup is working!

- For stopping follow this order

	  Job History Server
      NodeManager
	  ResourceManager
      DataNodes
	  SecondaryNameNode
	  NameNode