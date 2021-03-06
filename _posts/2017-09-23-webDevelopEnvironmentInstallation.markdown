---
layout:     post
title:      "centos7作web服务器搭建环境(jdk1.8、Tomcat7.0、MySQL5.7)"
subtitle:   "centos7 for web server build environment (jdk1.8, Tomcat7.0, MySQL5.7)"
date:       2017-09-23 21:00:00
author:     "石头人m"
header-img: "img/about-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Linux
    - Java
---


centos7作web服务器搭建环境：jdk1.8、Tomcat7.0、MySQL5.7、nginx、maven


# 1.安装jdk配置java环境

检验系统原版本
```shell
[root@storm ~]# java -version
```
进一步查看JDK信息：
```shell
[root@storm ~]# rpm -qa | grep java
```
卸载：
```shell
[root@storm etc]# rpm -e --nodeps [所有的java包]
```

### 安装JDK
上传新的jdk-8-linux-x64.rpm软件到/usr/local/或者：
	wegt http://.....(网上找一个地址rpm下载链接)
然后执行以下命令安装jdk：
```shell
[root@storm local]# rpm -ivh jdk-8-linux-x64.rpm
```
JDK默认安装在/usr/java中

### 配置环境变量
```shell
vi + /etc/profile
```
向文件里面追加以下内容：
```shell
JAVA_HOME=/usr/java/jdk1.7.0
JRE_HOME=/usr/java/jdk1.7.0/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
```

使修改生效
```shell
[root@storm local]# source /etc/profile   //使修改立即生效
[root@storm local]#        echo $PATH   //查看PATH值
```

查看系统环境状态
```shell
[root@storm ~]# echo $PATH
/usr/local/cmake/bin:/usr/lib64/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/usr/java/jdk1.7.0/bin:/usr/java/jdk1.7.0/jre/bin:/root/bin
```

执行以下操作，查看信息是否正常：
```shell
[root@storm bin]# java -version
[root@storm bin]# java
[root@storm bin]# javac
```


# 2.安装Tomcat

获取压缩包
```shell
[root@storm local]# wget http://apache.fayea.com/apache-mirror/tomcat/tomcat-7/v7.0.57/bin/apache-tomcat-7.0.57.tar.gz  
```
解压压缩包  
```shell
[root@storm local]# tar -zxvf apache-tomcat-7.0.29.tar.gz 
```
修改字符集为utf-8：
	vi conf/server.xml
用 “/8080”搜索，在Connector标签里插入：URIEncoding="UTF-8" 

启动Tomcat:到Tomcat安装目录的/bin下，执行命令
```shell
[root@storm bin]# ./startup.sh
```


# 3.安装MySQL

### （1） 配置YUM源
下载mysql源安装包
```shell
shell> wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
安装mysql源
```shell
shell> yum localinstall mysql57-community-release-el7-8.noarch.rpm
```
检查mysql源是否安装成功：
```shell
yum repolist enabled | grep "mysql.*-community.*"
```

### （2）安装MySQL
```shell
yum install mysql-community-server
```

### （3）启动MySQL服务
```shell
systemctl start mysqld
```

### （4）设置为开机启动
```shell
systemctl enable mysqld
systemctl daemon-reload
```

### （5）修改root默认密码
mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：
```shell
shell>grep 'temporary password' /var/log/mysqld.log
shell> mysql -uroot -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
或者
```mysql
mysql> set password for 'root'@'localhost'=password('MyNewPass4!'); 
```
<b>注意：</b>mysql5.7默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示ERROR 1819 (HY000): Your password does not satisfy the current policy requirements错误

通过msyql环境变量可以查看密码策略的相关信息：show variables like '%password%';
validate_password_policy：密码策略，默认为MEDIUM策略  
validate_password_dictionary_file：密码策略文件，策略为STRONG才需要  validate_password_length：密码最少长度  
validate_password_mixed_case_count：大小写字符长度，至少1个  validate_password_number_count ：数字至少1个  
validate_password_special_char_count：特殊字符至少1个  上述参数是默认策略MEDIUM的密码检查规则。

#### 修改密码策略：
在/etc/my.cnf文件添加validate_password_policy配置，指定密码策略
 选择0（LOW），1（MEDIUM），2（STRONG）其中一种，选择2需要提供密码字典文件
validate_password_policy=0

如果不需要密码策略，添加my.cnf文件中添加如下配置禁用即可：
validate_password = off

重新启动mysql服务使配置生效：
systemctl restart mysqld

### （6）添加远程登录用户
默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须修改root允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，我添加一个新的帐户：
```mysql
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

### （7）配置默认编码为utf8
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：
```shell
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
```
重新启动mysql服务，查看数据库默认编码如下所示：
```mysql
show variables like '%character%'
```

### （8）默认配置文件路径：  
配置文件：/etc/my.cnf  
日志文件：/var/log//var/log/mysqld.log  
服务启动脚本：/usr/lib/systemd/system/mysqld.service  
socket文件：/var/run/mysqld/mysqld.pid


# 4.安装nginx

Nginx依赖安装：
```shell
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel 
```
yum直接安装：
```shell
yum install nginx
```
启动Nginx：
```shell
service nginx start  
```
然后反向代理配置。。。



# 5.安装maven

下载压缩包：
```shell
wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
```
解压：
```shell
tar -zxvf apache-maven-3.3.9-bin.tar.gz 
```
配置环境变量：
```shell
vim /etc/profile
```
  添加：
```shell
M2_HOME=/opt/tyrone/maven （注意这里是maven的安装路径）
export PATH=${M2_HOME}/bin:${PATH}
```
重载/etc/profile这个文件
```shell
source /etc/profile
```
检查安装是否成功：
```shell
mvn -version
```