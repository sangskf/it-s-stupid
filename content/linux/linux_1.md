# Linux几个基本命令

## 系统监控工具
1.vmstat - 虚拟内存统计

vmstat 命令报告有关进程、内存、分页、块 IO、中断和 CPU 活动等信息。

```
# vmstat 3
```
输出示例：

```
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
 r b swpd free buff cache si so bi bo in cs us sy id wa st
 0 0 0 2540988 522188 5130400 0 0 2 32 4 2 4 1 96 0 0
 1 0 0 2540988 522188 5130400 0 0 0 720 1199 665 1 0 99 0 0
 0 0 0 2540956 522188 5130400 0 0 0 0 1151 1569 4 1 95 0 0
 0 0 0 2540956 522188 5130500 0 0 0 6 1117 439 1 0 99 0 0
 0 0 0 2540940 522188 5130512 0 0 0 536 1189 932 1 0 98 0 0
 0 0 0 2538444 522188 5130588 0 0 0 0 1187 1417 4 1 96 0 0
 0 0 0 2490060 522188 5130640 0 0 0 18 1253 1123 5 1 94 0 0

```

2.找出占用内存资源最多的前 10 个进程

```
ps -auxf | sort -nr -k 4 | head -10
```

3.找出占用 CPU 资源最多的前 10 个进程


```
ps -auxf | sort -nr -k 3 | head -10
```

## JPS-Java进程状态工具

#### 列出PID和Java主类名

```
jps

2017 Bootstrap
2576 Jps
```

#### 列出pid和java完整主类名

```
jps -l

2017 org.apache.catalina.startup.Bootstrap
2612 sun.tools.jps.Jps
```

#### 列出pid、主类全称和应用程序参数

```
jps -lm

2017 org.apache.catalina.startup.Bootstrap start
2588 sun.tools.jps.Jps -lm
```

#### 列出pid和JVM参数

```

jps -v

2017 Bootstrap -Djava.util.logging.config.file=/usr/local/tomcat-web/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dfile.encoding=UTF-8 -Xms256m -Xmx1024m -XX:PermSize=256m -XX:MaxPermSize=512m -verbose:gc -Xloggc:/usr/local/tomcat-web/logs/gc.log-2014-02-07 -XX:+UseConcMarkSweepGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Xnoclassgc -Djava.endorsed.dirs=/usr/local/tomcat-web/endorsed -Dcatalina.base=/usr/local/tomcat-web -Dcatalina.home=/usr/local/tomcat-web -Djava.io.tmpdir=/usr/local/tomcat-web/temp
2624 Jps -Dapplication.home=/usr/lib/jvm/jdk1.6.0_43 -Xms8m
```

#### 和【ps -ef | grep java】类似的输出

```
jps -lvm

2017 org.apache.catalina.startup.Bootstrap start -Djava.util.logging.config.file=/usr/local/tomcat-web/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dfile.encoding=UTF-8 -Xms256m -Xmx1024m -XX:PermSize=256m -XX:MaxPermSize=512m -verbose:gc -Xloggc:/usr/local/tomcat-web/logs/gc.log-2014-02-07 -XX:+UseConcMarkSweepGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Xnoclassgc -Djava.endorsed.dirs=/usr/local/tomcat-web/endorsed -Dcatalina.base=/usr/local/tomcat-web -Dcatalina.home=/usr/local/tomcat-web -Djava.io.tmpdir=/usr/local/tomcat-web/temp
2645 sun.tools.jps.Jps -lvm -Dapplication.home=/usr/lib/jvm/jdk1.6.0_43 -Xms8m
```


## AB压力测试工具

#### 安装

```
yum -y install httpd-tools
```
#### 检测

```
ab -V
```

```
This is ApacheBench, Version 2.3 <$Revision: 1430300 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
```


#### ab参数说明

有关ab命令的使用，我们可以通过帮助命令进行查看。如下：

```
ab --help
```

下面我们对这些参数，进行相关说明。如下：

-n在测试会话中所执行的请求个数。默认时，仅执行一个请求。

-c一次产生的请求个数。默认是一次一个。

-t测试所进行的最大秒数。其内部隐含值是-n 50000，它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。

-p包含了需要POST的数据的文件。

-P对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。

-T POST数据所使用的Content-type头信息。

-v设置显示信息的详细程度-4或更大值会显示头信息，3或更大值可以显示响应代码(404,200等),2或更大值可以显示警告和其他信息。

-V显示版本号并退出。

-w以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。

-i执行HEAD请求，而不是GET。

-x设置<table>属性的字符串。

-X对请求使用代理服务器。

-y设置<tr>属性的字符串。

-C对请求附加一个Cookie:行。其典型形式是name=value的一个参数对，此参数可以重复。

-H对请求附加额外的头信息。此参数的典型形式是一个有效的头信息行，其中包含了以冒号分隔的字段和值的对(如,"Accept-Encoding:zip/zop;8bit")。

-A对服务器提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即,是否发送了401认证需求代码)，此字符串都会被发送。

-h显示使用方法。

-d不显示"percentage served within XX [ms] table"的消息(为以前的版本提供支持)。

-e产生一个以逗号分隔的(CSV)文件，其中包含了处理每个相应百分比的请求所需要(从1%到100%)的相应百分比的(以微妙为单位)时间。由于这种格式已经“二进制化”，所以比'gnuplot'格式更有用。

-g把所有测试结果写入一个'gnuplot'或者TSV(以Tab分隔的)文件。此文件可以方便地导入到Gnuplot,IDL,Mathematica,Igor甚至Excel中。其中的第一行为标题。

-i执行HEAD请求，而不是GET。

-k启用HTTP KeepAlive功能，即在一个HTTP会话中执行多个请求。默认时，不启用KeepAlive功能。

-q如果处理的请求数大于150，ab每处理大约10%或者100个请求时，会在stderr输出一个进度计数。此-q标记可以抑制这些信息。


#### ab性能指标

在进行性能测试过程中有几个指标比较重要：

1、吞吐率（Requests per second）

服务器并发处理能力的量化描述，单位是reqs/s，指的是在某个并发用户数下单位时间内处理的请求数。某个并发用户数下单位时间内能处理的最大请求数，称之为最大吞吐率。

记住：吞吐率是基于并发用户数的。这句话代表了两个含义：

a、吞吐率和并发用户数相关

b、不同的并发用户数下，吞吐率一般是不同的

计算公式：总请求数/处理完成这些请求数所花费的时间，即

Request per second=Complete requests/Time taken for tests

必须要说明的是，这个数值表示当前机器的整体性能，值越大越好。

2、并发连接数（The number of concurrent connections）

并发连接数指的是某个时刻服务器所接受的请求数目，简单的讲，就是一个会话。

3、并发用户数（Concurrency Level）

要注意区分这个概念和并发连接数之间的区别，一个用户可能同时会产生多个会话，也即连接数。在HTTP/1.1下，IE7支持两个并发连接，IE8支持6个并发连接，FireFox3支持4个并发连接，所以相应的，我们的并发用户数就得除以这个基数。

4、用户平均请求等待时间（Time per request）

计算公式：处理完成所有请求数所花费的时间/（总请求数/并发用户数），即：

Time per request=Time taken for tests/（Complete requests/Concurrency Level）

5、服务器平均请求等待时间（Time per request:across all concurrent requests）

计算公式：处理完成所有请求数所花费的时间/总请求数，即：

Time taken for/testsComplete requests

可以看到，它是吞吐率的倒数。

同时，它也等于用户平均请求等待时间/并发用户数，即

Time per request/Concurrency Level

#### ab实际使用

ab的命令参数比较多，我们经常使用的是-c和-n参数。

ab -c 10 -n 100 http://www.xxx.com/index.php

-c10表示并发用户数为10

-n100表示请求总数为100

http://www.xxx.com/index.php表示请求的目标URL

这行表示同时处理100个请求并运行10次index.php文件。


## TOP命令查看CPU利用率

linux下用top命令查看cpu利用率超过100%，这里显示的所有的cpu加起来的使用率，说明你的CPU是多核，你运行top后按大键盘1看看，可以显示每个cpu的使用率，top里显示的是把所有使用率加起来。

按下1后可以看到我的机器的CPU是双核的。%Cpu0，%Cpu1，%Cpu2......

![输入图片说明](https://images.gitee.com/uploads/images/2018/1009/090959_66c1e25f_87650.png "9Y70@H`2}N7@LB6SOXKGF)E.png")

这里我们也可以查看一下CPU信息：在命令行里输入：cat /proc/cpuinfo


这里可以看到cpu cores : 11

## find用法

Linux中find常见用法示例

·find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \;

find命令的参数；

pathname: find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录。
-print： find命令将匹配的文件输出到标准输出。
-exec： find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' { } \;，注意{ }和\；之间的空格。
-ok： 和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。

-print 将查找到的文件输出到标准输出
-exec   command   {} \;      —–将查到的文件执行command操作,{} 和 \;之间有空格
-ok 和-exec相同，只不过在操作前要询用户
 
原文：https://www.cnblogs.com/archoncap/p/6144369.html

## linux修改默认SSH端口

linux SSH默认端口是22，不修改的话存在一定的风险，要么是被人恶意扫描，要么会被人破解或者攻击，所以我们需要修改默认的SSH端口。
```
vi /etc/ssh/sshd_config 
```
默认端口是22，并且已经被注释掉了，打开注释修改为其他未占用端口即可。

开启防火墙端口并重复服务即可。
```
systemctl restart sshd.service
```

## 查看某个文件夹的总容量

```
du -sh 
```

## 通过ps及top命令查看进程信息

通过ps及top命令查看进程信息时，只能查到相对路径，查不到的进程的详细信息，如绝对路径等。这时，我们需要通过以下的方法来查看进程的详细信息：

/proc
Linux在启动一个进程时，系统会在/proc下创建一个以PID命名的文件夹，在该文件夹下会有我们的进程的信息， 
其中包括一个名为exe的文件即记录了绝对路径，通过ll或ls –l命令即可查看。 

```
ll /proc/PID
```

比如，我们查看mongo使用以下命令：

```
[root@rmpapp local]# ps -ef|grep mongo
root      9466  6380  0 13:41 pts/1    00:00:00 grep --color=auto mongo
root     16053     1  0 8月14 ?       06:45:52 ./mongod --config mongodb.conf
```
然后：

```
ll /proc/16053
```

## Linux远程拷贝同步命令

通常情况下我们需要在两个Linux服务器之间拷贝文件，比如定时备份。

以博客为例，网站目录定时打包比如一周或者一个月，远程同步到备份服务器。

博客服务器输入一下命令，然后输入远程主机密码，即可进行同步拷贝：

```
scp -r /mnt/domains/blog.52itstyle.com_20181024.tar.gz  root@115.29.143.135:/home/backups/
```

如果想增量拷贝，我们可以使用rsync命令。
```
rsync -avz  /mnt/domains/blog.52itstyle.com  root@115.29.143.135:/home/backups/
```

如果出现：
```
RSA host key for [ip address] has changed and you have requested strict checking
```
可能是系统重装后，本地机和服务器内部ssh对不上导致错误，因此，只需要删除本地机ssh缓存信息，即可恢复。 
在本地机输入一下命令行：

```
ssh-keygen -R IP
```




