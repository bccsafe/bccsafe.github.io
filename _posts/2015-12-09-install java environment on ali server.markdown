---
layout: post
title: 阿里云上安装java环境
categories:
- mess
---

最近因为项目需要，需要台服务器，新浪SAE限制好多，还有点学习成本，就考虑尝试下阿里云

以学生价9.9一个月买的，顺手就点了ubuntu64位，可能是因为初中时折腾过吧，眼熟！

然后就开始了装java运行环境的坎坷路...

## 安装jdk7

* 先去官方下载适合自己的版本 
	[http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk7-downloads-1880260.html)
因为官方的下载点用wget下载不到，就得自己下载到本地再传过去  

```
	scp -d /Users/BccSafe/Downloads/jdk-7u79-linux-x64.gz root@121.42.174.229:/root/
``` 

* 解压

```	
	tar zxvf /root/jdk-7u79-linux-x64.gz -C /usr/local/
	cd /usr/local/
	ln -s /usr/local/jdk1.7.0_79/ java 
```

* 配置环境变量 vi ~/.bashrc 不熟悉的需要百度下vi的操作方法 

```	
	export JAVA_HOME=/usr/local/java/ 
	export JRE_HOME=${JAVA_HOME}/jre	
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
	export PATH=${JAVA_HOME}/bin:$PATH 	
```

* 刷新

```
	source ~/.bashrc
```

* 测试

```
	java -version
```
	
## 安装mysql

* 阿里云内网下有安装包，直接下载

```	
	wget http://oss.aliyuncs.com/aliyunecs/onekey/mysql/mysql-5.5.35-linux2.6-x86_64.tar.gz
```

* 解压

```
	tar zxvf mysql-5.5.35-linux2.6-x86_64.tar.gz  -C /usr/local/ 
```
 
* 安装

```	
	cd /usr/local/
	ln -s /usr/local/mysql-5.5.35-linux2.6-x86_64/ mysql 	groupadd mysql
	useradd -g mysql mysql
	chown -R mysql:mysql /usr/local/mysql
	chown -R mysql:mysql /usr/local/mysql-5.5.35-linux2.6-x86_64
	cd /usr/local/mysql
	./scripts/mysql_install_db --user=mysql  
	cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld 
	chmod 755 /etc/init.d/mysqld
	sudo apt-get install chkconfig 
	echo "export LC_ALL=C" >> /root/.bashrc
	source /root/.bashrc 	chkconfig --add mysqld
	ln -s /usr/lib/insserv/insserv /sbin/insserv
 	chkconfig --add mysqld
	chkconfig --level 3 mysqld on 
	cp /usr/local/mysql/support-files/my-huge.cnf /etc/my.cnf
	mv /usr/local/mysql/data /var/lib/mysql
	chown -R mysql:mysql /var/lib/mysql
```

* 启动

```
	service mysqld start
```

* 设置密码，任何人都能从外网访问

```	
	/usr/local/mysql/bin/mysqladmin -u root password (Your Password!)
	/usr/local/mysql/bin/mysql -uroot -p(Your Password!)
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '(Your Password!)' WITH GRANT OPTION;
	FLUSH   PRIVILEGES; 	
```

## 安装tomcat

* 下载

```
	wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.65/bin/apache-tomcat-7.0.65.tar.gz
```

* 解压

```	
	tar zxvf apache-tomcat-7.0.65.tar.gz -C /usr/local/
```

* 安装 

```	
	cd /usr/local/
	ln -s /usr/local/apache-tomcat-7.0.65/ tomcat 
	cd /usr/local/tomcat/
	chown -R root .
	chgrp -R root .
```

* 配置

```
	vi /etc/profile
	CATALINA_HOME=/usr/local/tomcat 
	export CATALINA_HOME
	source /etc/profile
	cd $CATALINA_HOME/bin
	vi catalina.sh
	/#OS specific support.  $var _must_ be set to either true or false.
	CATALINA_HOME=/usr/local/tomcat
	JAVA_HOME=/usr/local/java 
```

* 安装服务

```
	cp catalina.sh /etc/init.d/tomcat
```

* 启动

```	
	service tomcat start
```





 