驯象非易事，于是乎，我和它做了一个约定。

<!--more-->

喂食Hadoop
===

上一节给出了一个执行Hadoop程序的例子：

```
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar grep /input /output 'dfs[a-z.]+'
```

该命令的功能是，让Hadoop执行一个`jar`文件（本质是动态加载外部的class，这得益于Java的创新之一——类加载器）。`grep`、`/input`等作为参数传递给`jar`。

通过这个例子也可以看出，执行Hadoop程序实质上是给hadoop命令传一个jar包。而jar包其实就是一个符合Hadoop规范的Java的类，所谓的“规范”就是要符合MapReduce的编程模型，所以，我们写的Hadoop程序被称作MapReduce程序。

这就是我和驯服金象所做的约定——喂你jar包，替我耕地。

Hadoop开发的步骤可以归纳为：

1. 写一个Java类（MapReduce程序)
2. 打成Jar包
3. 喂给Hadoop执行

准备食物
===

安装插件
---

选择了编程语言，也就基本确定了开发环境。为了提高开发效率，我们使用优秀的IDE——Eclipse。

我们希望Eclipse可以进行Hadoop程序的开发，即能够一体化的完成上面提到的Hadoop开发的三大步骤。幸好，已经有前辈做了一款Eclipse插件——[hadoop2x-eclipse-plugin](https://github.com/winghc/hadoop2x-eclipse-plugin)，可以做到这一点。

![](hadoop-eclipse.jpg)

插件的安装虽不麻烦，但也得花些时间。所以，奉上快速安装该插件的[小脚本](https://github.com/zhangxiaoyang/hadoop-installer/tree/master/scripts)。

使用插件
---

插件安装成功后，还需要打开Eclipse进行一点点的配置。（最不愿意写配置的文章了%>_<%）

### 配置Hadoop安装目录

```
Window -> Preference -> Hadoop Map/Reduce -> Hadoop installation directory
```

### 配置Map/Reduce视图

```
Window -> Open Perspective -> Other-> Map/Reduce -> Choose OK
Window -> show View -> Other-> Map/Reduce Locations -> Choose OK
```

控制台会多出一个“Map/Reduce Locations”的Tab页，在该Tab页空白的地方右键，选择“New Hadoop location...”，弹出对话框“New hadoop location...”。将User name设置为安装Hadoop的当前用户。

![](hadoop-eclipse-配置1.jpg)

### 设置Map/Reduce Locations

![](hadoop-eclipse-配置2.jpg)

### 测试HDFS

在Project Explorer里的DFS Locations上右键，选择“Refresh”。如果能够显示HDFS上的文件，则表示配置成功。

### 新建Map/Reduce任务

```
File -> New -> Project -> Map/Reduce Project -> Next
```

上一节我们使用的是Hadoop自带的已经编译好的Jar包(WordCount)

现在，我们编写自己的WordCount。

WordCount源代码可以通过以下方式获得：

到[这里](http://www.apache.org/dyn/closer.cgi/hadoop/common/)选择一个镜像地址，下载与安装的Hadoop版本一致的源代码包，比如`hadoop-2.6.0-src.tar.gz`

使用以下命令，找到WordCount文件。

```
tar xzvf hadoop-2.6.0-src.tar.gz
cd hadoop-2.6.0-src
find . -name WordCount.java
```

### 运行Map/Reduce任务

右键当前项目，选择“Run As -> Run Confiugrations”。

需要注意参数的配置，要使用`hdfs://`这样的格式表示HDFS上的文件。WordCount会自动生成输出文件夹（这里是`/output`），运行前先手动删除之。

运行程序之前，要保证HDFS已经被格式化过，并且运行`sbin/start-dfs.sh`。

一切就绪后，就可以运行MapReduce程序了。

Hadoop执行情况可以在<http://localhost:50070/>查看。也可以使用File Explorer浏览HDFS生成的文件。

![](hadoop-eclipse-配置3.jpg)

参考链接：

1. <http://www.linuxidc.com/Linux/2015-01/112369.htm>
2. <https://github.com/winghc/hadoop2x-eclipse-plugin>
3. <https://github.com/zhangxiaoyang/hadoop-installer/tree/master/scripts>

`-- EOF --`
