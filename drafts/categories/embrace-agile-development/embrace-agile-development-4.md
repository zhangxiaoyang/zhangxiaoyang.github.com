持续集成并非万能，仍需本地测试。

<!--more-->

通常每次往代码库提交代码时，都会触发服务器持续集成。在提交代码之前，仍少不了本地测试。

本地测试的工具和持续集成所使用的工具几乎一样，可以先在本地测试一下，保证没问题以后再提交代码。

关注代码“健康”
===

我们通常关注代码的以下几个指标，以求一个健康的代码库：

- 圈复杂度：代码之间的调用关系越复杂，圈复杂度越大，我们不希望它太大。
- 覆盖率：被测试代码覆盖的程度，拥抱敏捷开发，便拥抱了测试，多写测试，以求百分百覆盖率。
- 代码规范：代码风格越统一，程序员就越好交流。
- 代码重复：代码重复意味着编写的不够好，要尽可能的减少重复。

使用工具计量
===

SLOCCount（圈复杂度）
---

安装：

```
sudo apt-get install sloccount
```

使用：

```
sloccount --duplicates --wide --details FILE > sloccount.sc
```

其中，`--duplicates`表示计算重复的文件，`--wide`用于指定格式，`--details`显示详情。`FILE`可以是`.`（当前目录所有层次的文件），也可以是一个文件夹的名称，或者是文件名。

Coverage（覆盖率）
---

安装：

```
pip install coverage
```

使用：

```
find . | grep ".py$"| xargs coverage run
coverage xml --omit=/usr/local/lib/*,/usr/share/*
```

coverage的使用分两部，先选择要执行的文件，然后使用`coverage xml`生成结果文件。`--omit`表示要排除的文件，通常要排除依赖的库文件。也可以在命令行查看结果，`coverage report`。

Pylint（代码规范）
---

安装：

```
pip install pylint
```

使用：

```
rm -f pylint.log
for f in `find . | grep ".py$"`
do
    pylint --output-format=parseable --reports=y "$f" >> pylint.log
done
```

其中，`--output-format`指定输出的格式，本地测试可以指定为`text`。`--reports`可以选`y`或`n`，为`y`是表示既输出源代码分析部分，也含报告部分。

要对多个文件做规范检查时，可以使用`for`来完成，然后追加到同一个文件。

Clonedigger（代码重复）
---

安装：

```
pip install clonedigger
```

使用：

```
find . | grep ".py$" | xargs clonedigger --cpd-output
```

对于多个文件的重复比对，可以使用`xargs`。其中，`--cpd-output`，用于和jenkins集成。


参考链接：

1. <http://wobfei.iteye.com/blog/706875>
2. <http://www.python.org/dev/peps/pep-0008/>
3. <http://www.dwheeler.com/sloccount/sloccount.html>
4. <http://nedbatchelder.com/code/coverage/>
5. <http://www.pylint.org/>
6. <http://www.ibm.com/developerworks/cn/linux/l-cn-pylint/>
7. <http://clonedigger.sourceforge.net/>

`-- EOF --`
