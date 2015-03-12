对于一个实际的Python项目，如何发挥敏捷开发的作用？

<!--more-->

要想进行敏捷的开发，除了要管理好人，也要管理好整个软件开发过程。持续集成不仅仅可以使开发者更加敏捷，也起到了规范的作用。比如，可以把代码审查、覆盖率检测等加入到自动构建的脚本中去，强制开发者写测试等等。

[Jenkins](http://jenkins-ci.org/)是一个开源的持续集成系统，支持插件扩展，非常易于上手。

![](jenkins-logo.png)

在Python项目中使用Jenkins
===

安装Jenkins插件
---

为了能够从Git仓库拉取代码，我们需要安装以下Jenkins插件：
- GIT plugin
必装
- GitHub API Plugin
连接GitHub
- GitBucket Plugin
连接Bitbucket

为了能够使用代码测试、评估的结果，需要以下Jenkins插件：
- xUnit plugin
调用nose结果
- SLOCCount Plug-in
调用sloccount结果
- Violations plugin
调用clonedigger、pylint结果
- Warnings Plug-in
调用pyflakes结果
- Cobertura Plugin
调用coverage结果

安装Python库
---

执行代码测试、评估，需要以下Python库：

- nose
单元测试
- clonedigger
重复代码检测
- pylint
代码分析
- pyflakes
代码分析
- coverage
覆盖率计算

测试、评估结果导入到Jenkins
---

在Jenkins中新建一个项目，其`Execute shell`填写如下：

```
#!/usr/bin/env bash
echo `pwd`
bash -x jenkins-for-python.sh
```

其中，`jenkins-for-python.sh`需要放在版本库的根目录。内容可以参考[此处](https://github.com/zhangxiaoyang/jenkins-test-repo)。

其中主要的功能是，批处理执行各种测试、各种生成评估报告。其中，生成的文件名要和Jenkins项目中的构建后操作的配置一致。

- Scan for compiler warnings
File pattern填写`pyflakes.log`，Parser选择`PyLint`
- Publish Cobertura Coverage Report
填写`coverage.xml`
- Publish JUnit test result report
填写`nosetests.xml`
- Publish SLOCCount analysis results
填写`sloccount.sc`
- Report Violations
cpd填写`output.xml`，pylint填写`pylint.log`

通过上面的配置，就可以把命令行的输出文件与jenkins连接起来了。可以在jenkins的管理界面看这些赏心悦目的图表，而非黑黑的命令行。

还可以做更多
---

还可以配置接收邮件，使得开发者能够了解到构建的情况。

还可以与其它项目管理平台连接，如`redmine`。

好了，万事俱备，只欠一个好的Leader和团队了。

参考链接：

1. <http://www.ibm.com/developerworks/cn/java/j-lo-jenkins/>
2. <http://www.wallix.org/2011/06/29/how-to-use-jenkins-for-python-development/>
3. <http://www.hustlzp.com/post/2014/08/jenkins>

`-- EOF --`
