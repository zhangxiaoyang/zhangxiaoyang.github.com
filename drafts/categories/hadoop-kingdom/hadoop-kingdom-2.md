以前耕地用牛，现在用大象。

<!--more-->

Hadoop简介
===

Hadoop之生
---

要想处理大数据，必须用“大机器”，传统的“牛拉车”已经不适用了。

![](老牛.jpg)

如果我们没有大机器，怎么办？——把小机器们组成一个“大机器”。

这组成之法便是金象——Hadoop，由Apache基金会开发，受启发自Google的MapReduce框架。

![](hadoop大象.jpg)

Hadoop之必杀技
---

>The Apache™ Hadoop® project develops open-source software for reliable, scalable, distributed computing.

>The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-available service on top of a cluster of computers, each of which may be prone to failures.

简而言之，Hadoop是可靠、可扩展的分布式计算框架。

就是说，Hadoop提供了一个编程模型——MapReduce，只要按照MapReduce的规范去编写程序，就可以直接丢给Hadoop执行。而具体的分布式执行过程、数据的分布式存储都是透明的，开发者无需了解。

而“可靠”的具体表现是，数据多机器备份，有机器Down掉或者硬盘挂了，都没事。“可扩展”是指，可以增删机器到Hadoop集群中。之所以说Hadoop是一个框架，是因为它是一个“筐子”，提供各种接口，可以开发上层应用，比如Hive。

![](hadoop生态.jpg)

Hadoop使用
===

从实践的角度来看，hadoop就是一个压缩包，解压以后经过一些配置，启动相关服务，就可以使用其提供的命令，比如`hadoop`、`hdfs`等。

安装Hadoop
---

Hadoop有三种模式：

1. 单机模式
完全在本地跑，不使用HDFS（分布式文件系统），也不加载Hadoop的任何守护进程，主要用于MapReduce的调试。
2. 伪分布式模式
和完全分布式模式的执行过程一样，只不过守护进程都运行在一台机器之上，可以调试代码、查看内存使用情况、检查HDFS输入输出、与守护进程交互。
3. 完全分布式模式
Hadoop的守护进程运行与集群之上。

守护进程是一种后台进程，Hadoop的守护进程主要的作用是保护这个“筐子”，发现问题并及时处理。

可以按照Hadoop的[官方文档](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html)来安装、配置。

自己用的话，伪分布式模式就足够了。为了方便大家安装Hadoop，写了个[小脚本](https://github.com/zhangxiaoyang/hadoop-installer)，可以选择安装单机模式或伪分布式模式。

Hadoop测试
---

伪分布式的一个测试样例：

```
cd hadoop-2.*/
mkdir input
cp etc/hadoop/*.xml input

bin/hdfs namenode -format
sbin/start-dfs.sh
bin/hdfs dfs -put input /
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep /input /output 'dfs[a-z.]+'
bin/hdfs dfs -cat output/*
sbin/stop-dfs.sh

```
参考链接：

1. <http://blog.fens.me/hadoop-family-roadmap/>
2. <http://blog.csdn.net/hitwengqi/article/details/8008203>
3. <http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html>
4. <https://github.com/zhangxiaoyang/hadoop-installer>

`-- EOF --`
