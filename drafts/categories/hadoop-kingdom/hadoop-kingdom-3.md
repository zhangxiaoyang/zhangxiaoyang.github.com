月蚀之日，金象飞天。

<!--more-->

Hadoop程序
===

上一节给出了一个执行Hadoop程序的例子：

```
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep /input /output 'dfs[a-z.]+'
```

该命令的功能是，让Hadoop执行一个`jar`文件，文件里写了具体的业务逻辑，`grep`、`/input`等是jar文件中main函数接受的命令行传参。

通过这个例子也可以看出，执行Hadoop程序实质上是给hadoop命令传一个jar包。而jar包其实就是一个符合Hadoop要求的java的类，所谓的“要求”就是要符合MapReduce的编程模型。

所以，Hadoop开发的步骤：

1. 写一个java类
2. 打包成jar包
3. 喂给Hadoop执行

Hadoop开发
===

安装插件
---

选择了编程语言，也就基本确定了开发环境。为了提高开发效率，我们使用优秀的IDE——Eclipse。

我们希望Eclipse可以进行Hadoop程序的开发，即能够一体化的完成上面提到的Hadoop开发的三大步骤。所以，这就要求Eclipse可以调用Hadoop命令，可以与HDFS通信，可以打包好写好的代码并提交给Hadoop执行。

还好，已经有前辈做了一款Eclipse插件——[hadoop2x-eclipse-plugin](https://github.com/winghc/hadoop2x-eclipse-plugin)，满足我们的需求。

![](hadoop-eclipse.jpg)

插件的安装虽不麻烦，但也得花些时间。所以，奉上快速安装该插件的[小脚本](https://github.com/zhangxiaoyang/hadoop-installer/tree/master/scripts)。

使用插件
---

插件安装成功后，还需要打开Eclipse进行一点点的配置。可以参考[这里]<http://www.linuxidc.com/Linux/2015-01/112369.htm>。

需要注意的是，DFS Master的端口应该填写`9000`（除非你修改了Hadoop的配置文件）。在运行程序之前，要保证HDFS已经被格式化过，并且运行`sbin/start-dfs.sh`。

一切就绪后，可以把Hadoop自带的WordCount源码贴过来运行。

可以到[这里](http://www.apache.org/dyn/closer.cgi/hadoop/common/)选择一个镜像地址，下载与安装的Hadoop版本一致的源代码包，比如`hadoop-2.6.0-src.tar.gz`

可以使用以下命令，找到WordCount文件。

```
tar xzvf hadoop-2.6.0-src.tar.gz
cd hadoop-2.6.0-src
find . -name WordCount.java
```

`-- EOF --`
