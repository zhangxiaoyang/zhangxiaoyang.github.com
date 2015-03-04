
Hadoop已经不仅仅是一颗灵丹妙药，更是一片沃土，各种生物在此斗艳争芳。其中一只是——Hive。

<!--more-->

Hive简介
===

Hive之生
---

有了Hadoop，就可以编写我们自己的MapReduce程序，分析大数据了。

但是，针对每一个问题都写一个MapReduce程序，实在是太不方便。这种感觉就像是，每次分析数据都直接拿C/C++等语言来写，开发效率不高。所以，就有人开发了一些工具，比如`grep`、`awk`、`sed`等等。

同理，现在有了Hadoop这个“筐子”，可以基于它做很多工具，简化开发。

Hive的源于Facebook的日志分析需求，现在属于Apache的顶级项目。

>Apache Hive is a data warehouse infrastructure built on top of Hadoop for providing data summarization, query, and analysis.

![](hive-logo.jpg)

Hive安装与配置
---

写了一个小脚本[hive-installer](https://github.com/zhangxiaoyang/hadoop-installer/tree/master/scripts)，可以用它来一键安装。

运行Hive之前要确保Hadoop平台已准备好（`start-dfs.sh`，已经格式化过的HDFS，参考[这里](https://github.com/zhangxiaoyang/hadoop-installer)）。

Hive之小试牛刀
---

数据分析的手段有很多，其中一种是使用数据库。

我们经常这样做：把数据导入数据库中，使用SQL语句做一些分析处理：

```
select count(*) from test where age=18
```

Hive的灵感即来源于此，提供类似SQL的HiveQL语句。Hive可以把HiveQL转换成MapReduce程序执行，比单独编写MapReduce要爽很多。

需要注意的是，Hive中并没有数据库，数据是一个本地或HDFS的纯文本文件。

如何把纯文本文件交给Hive？如何把纯文本文件当做数据库的表来用？下面是一个例子。（HiveQL不区分大小写。)

第一步，创建表：
```
hive> create table user (id int, age int, gender string, occupation string, zipcode string)
row format delimited
fields terminated by '|';

```

可见，Hive通过指定分隔符（这里是“|”），来模拟出一张表。所以，Hive输入的文件应该是结构化的。

第二步，把文件导入到表中：

（`u.user`到[此处](http://files.grouplens.org/datasets/movielens/ml-100k.zip)下载。)

```
# 加载本地文件
load data local inpath '/home/USERNAME/u.user' overwrite into table user;

# 加载HDFS文件
load data local inpath '/FILE' overwrite into table user;
```

第三步，尽情的执行HiveQL语句吧：

```
select count(*) from user;
```

Hive之技能补充
---

把HiveQL写进一个脚本文件，如`test.hive`，可以直接调用Hive执行该脚本：

```
hive -f test.hive
```

导出Hive的执行结果：

```
hive -f test.hive > LOCAL_DIR
```

或者，在`test.hive`中使用以下方式导出结果：

```
# 导出到本地
insert overwrite local directory '/home/USERNAME/LOCAL_DIR' select count(*) from user;

# 导出到HDFS
insert overwrite directory '/DIR' select count(*) from user;
```

当然，HiveQL内建的函数毕竟有限，所以，提供了UDF（User-Defined Functions）定制自己的函数。

数据仓库
===

数据仓库和数据库，是两个经常被提及的概念。Hive虽然模仿了SQL，但它本身却是一个数据仓库的架构。

个人认为，区分两者主要看事务。数据库是进行事务操作的，比如增删改查。而数据仓库里往往放的是比较干净且内容不太会发生变化的数据，主要用于查询分析。

所以，Hive是一个数据仓库的基础架构，算是Hadoop之上的一个App。

为啥要起名为Hive（蜂巢）？看了下面这张图就能感受到Hive设计者的想象力了。

![](数据仓库蜂巢.jpg)

参考链接：

1. <https://cwiki.apache.org/confluence/display/Hive/GettingStarted>
2. <http://grouplens.org/datasets/movielens/>
3. <https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF>
4. <https://github.com/zhangxiaoyang/hadoop-installer>

`-- EOF --`
