
Hadoop之上有一只灵兽——飞天猪（Pig）。

<!--more-->

Pig简介
===

Pig之生
---

Pig和Hive一样，都是Hadoop之上的一个App，都可以把它们可以识别的语言转换MapReduce程序执行，Pig可以识别的语言称为Pig Latin。

Pig源于Yahoo!，同样是Apache的顶级项目。

>Apache Pig is a platform for analyzing large data sets that consists of a high-level language for expressing data analysis programs, coupled with infrastructure for evaluating these programs. The salient property of Pig programs is that their structure is amenable to substantial parallelization, which in turns enables them to handle very large data sets.

![](pig-logo.jpg)

Pig安装与配置
---

写了一个小脚本[pig-installer](https://github.com/zhangxiaoyang/hadoop-installer/tree/master/scripts)，可以用它来一键安装。

运行Pig之前要确保Hadoop平台已准备好（`start-dfs.sh`，已经格式化过的HDFS，参考[这里](https://github.com/zhangxiaoyang/hadoop-installer)）。

Pig之小试牛刀
---

第一步，加载文件：
（需要先把`u.user`放到HDFS根目录。`u.user`到[此处](http://files.grouplens.org/datasets/movielens/ml-100k.zip)下载。)

```
grunt> A = load '/u.user' using PigStorage('|') as
(
    id:int,
    age:int,
    gender:chararray,
    occupation:chararray,
    zipcode:chararray
)
```

可以看到，Pig和Hive很像，都是通过指定分隔符的方式来区分字段。

第二步，尽情的执行Pig Latin语句吧：

```
B = foreach A generate id;
```

第三步，导出数据：

```
store B into '/DIR';

```

Pig补充
---

Pig有两种模式：本地模式、MapReduce模式。

执行`pig`默认进入MapReduce模式，所以脚本的路径均指HDFS的路径。

可以通过`pig -x local`进入本地模式，则路径指本地路径。

同样可以让Pig执行一个脚本文件：

```
pig -f test.pig
```

Pig、Hive分不清
---

Pig和Hive是功能很相似的两个工具，网上有很多人在讨论它俩的区别。

在[这里](http://stackoverflow.com/questions/10279942/what-is-the-difference-between-apache-pig-and-apache-hive)找到一个很不错的解释。

原文如下：

>Apache Pig and Hive are two projects that layer on top of Hadoop, and provide a higher-level language for using Hadoop's MapReduce library. Apache Pig provides a scripting language for describing operations like reading, filtering, transforming, joining, and writing data -- exactly the operations that MapReduce was originally designed for. Rather than expressing these operations in thousands of lines of Java code that uses MapReduce directly, Pig lets users express them in a language not unlike a bash or perl script. Pig is excellent for prototyping and rapidly developing MapReduce-based jobs, as opposed to coding MapReduce jobs in Java itself.

>If Pig is "scripting for Hadoop", then Hive is "SQL queries for Hadoop". Apache Hive offers an even more specific and higher-level language, for querying data by running Hadoop jobs, rather than directly scripting step-by-step the operation of several MapReduce jobs on Hadoop. The language is, by design, extremely SQL-like. Hive is still intended as a tool for long-running batch-oriented queries over massive data; it's not "real-time" in any sense. Hive is an excellent tool for analysts and business development types who are accustomed to SQL-like queries and Business Intelligence systems; it will let them easily leverage your shiny new Hadoop cluster to perform ad-hoc queries or generate report data across data stored in storage systems mentioned above.

我的理解是，Pig是对Hadoop写脚本，而Hive是对Hadoop做查询。写脚本是要有编程基础的，但也更灵活。做查询似乎对编程的要求要低一些，但也不够灵活。尽管Pig和Hive都提供了UDF扩展，但似乎也是少数人触碰的吧。

由于是来自Facebook和Yahoo!两个不同的公司，功能肯定会有重合，一点也不奇怪。就像支付宝、百付宝、微信支付，唉。

Pig会飞
===

在Pig的官网上有对Pig哲学的解释，大抵是说看似笨笨的Pig其实是充满智慧的！

- Pig可以吃任何东西（处理任何数据）——有容乃大
- Pig会出现在任何地方（对数据并行处理，不依赖并行处理框架）——自力更生
- Pig是家禽（容易被使用，被优化）——亲和力
- Pig会飞（可以飞快的处理数据，并持续提高性能）——上进心

参考链接：

1. <http://pig.apache.org/docs/r0.14.0/start.html>
2. <http://stackoverflow.com/questions/10279942/what-is-the-difference-between-apache-pig-and-apache-hive>
3. <http://pig.apache.org/philosophy.html>
4. <https://github.com/zhangxiaoyang/hadoop-installer>

`-- EOF --`
