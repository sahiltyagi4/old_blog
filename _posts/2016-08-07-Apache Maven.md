---
layout:     post
title:      Apache Maven setup and installation
date:       2016-08-07 00:30:17
summary:    This post describes how to setup Apache Maven an a Unix-based Operating Environment (Ubuntu, Debian, Mac)
categories: Apache Maven
---

Apache Maven is an open source build automation tool based on the concept of project object model (POM). Maven addresses two aspects of building software: first, it describes how software is built, and second, it describes its dependencies. An XML file named pom.xml describes the software project being built, its dependencies on other external modules and components, the build order, directories, and required plug-ins. Following are the steps to install and setup apache Maven in a :

- Download the maven tar.gz file with the following command:

	```
	# wget http://mirror.fibergrid.in/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
	```

- Untar the downloaded file as:

	```
	# tar -xzvf apache-maven-3.3.9-bin.tar.gz
	```

- Check the environment variable value:

	```
	echo $JAVA_HOME
	```

- Add the path of apache maven <b>bin</b> directory to PATH:

	```
	export PATH=/apache-maven-3.3.9/bin:$PATH
	```

- And you have installed Apache Maven on your system!

- Check the version of maven installed with the command <b>mvn -v</b>.