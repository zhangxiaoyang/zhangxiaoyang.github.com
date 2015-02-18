什么是关联规则？关联规则就是两个事情经过同时发生，并且在事情A发生的时候往往会发生B，于是我们就有了A=>B这条关联规则，比如谈恋爱=>结婚。我们为什么非老大劲去挖掘关联规则呢？因为关联规则里隐藏着“不能说的秘密”，有些秘密是有价值。还是谈恋爱=>结婚这条规则，就说明了谈恋爱最后基本都结婚了，让我们又相信了爱情，这还没价值吗？？如果是结婚=>谈恋爱，这就没什么意思了。

<!--more-->

关联规则挖掘，挖掘到的是关联规则。分类的挖掘，挖掘到（预测到）的是分类标号。

分类（Classification）
===

分类预测的是分类标号，什么是分类标号呢？

分类标号是离散值，比如把一个群体分成屌丝、土豪、高富帅三类，分类标号则分别是屌丝、土豪、高富帅。再比如把土豪分为小土豪、大土豪、高级土豪，可以根据月收入来分，5000以下的为小土豪，5000到0000的为大土豪，10000以上的为高级土豪。所以，分类标号需要离散化。

分类是预测的一种，预测的是分类标号，即把没有分类标号的群体分到有分类标号的群体里，也就完成了分类，具体如何分类则需要分类算法。

不管使用什么算法，分类过程主要有两个步骤，如下图表示。

![](分类过程.jpg)

决策树（Decision Tree）
---

决策树应该是最最最简单的一个分类算法（老师这么说的，我咋觉得也不太easy呢）。

决策树的过程可以用找对象的例子进行说明。把妹子分成喜欢的和不喜欢的，把喜欢的分成能有对象的和没对象的，把没对象的分成能追上的和不能追上的，可以表示为树的形式，如下图。

![](决策树形式.jpg)

这就是一棵决策树，也就是说根据这棵树进行决策的制定。其中，每一个内部节点（不是叶子的节点）表示一个属性上的测试，每一个分枝表示一个测试输出，每一个叶子节点表示一个类划分。

可以发现，决策树就是分类规则（分类规则不一定非得是我们一下子能看得懂的规则），或者说决策树就是学习到的模型（映射、函数）。只要有了决策树，再给我们一些新数据，就可以对这些新数据进行分类。问题是如何得到这棵决策树呢？

时光倒流到还没有这棵树的时候，我们只有一些事务记录，如下图。

![](事务记录.jpg)

原始数据的Name表示姓名，Like表示对其喜欢的程度评分，Mate为当前女生的配偶姓名，Object为追求的简易程度。我们要做的第一步就是对原始数据离散化，得到了预处理后的数据。要知道，决策树是在训练集的基础上学习出来的，所以，我们需要有一个靠谱的训练集，而训练集的格式采用预处理格式的要求。训练集中的Class表示类标号，有Yes类和No类。这样得到决策树后，我们拿未分类的数据进行分类，被分到Yes类的就是我们可以追求的女生（和分类的准确度有关，肯定有分的不合适的）。

通过上面的训练集我们可以得到决策树吗？决策树是一棵树，首先要确定的就是根节点，我们的根节点是哪个属性呢？女神、有对象、好追求？最开始给出的决策树使用的是喜欢吗（也就是女神这个属性），凭什么选这个作为根节点？

所以，就有了决策树的构造算法。只要构造出每个指定位置的节点，就完成了决策树的构造。

如何选择属性？或者说，如何选择当前的节点使用哪个属性进行测试？

所以就有了ID3算法、C4.5算法等。

ID3算法
---

ID3算法使用信息增益的方法来衡量一个属性对于决策的重要性，信息增益越大，属性就容易被优先选择。

如何计算信息增益呢？

对第k个属性计算信息增益：$gain\_k(D)= info(D)-info\_k(D)$。其中，$info(D)=-\sum\_{i=1}^{m} p\_i log\_2 (p\_i)$，$info\_k(D)=\sum\_{j=1}^{v} \frac{|D\_j|}{|D|}info(D\_j)$。看到这些公式估计不想再学数据挖掘，坚持坚持，挺住...

info求出来的是熵，为什么要用熵？熵可以表示某个随机变量出现的期望值，进而和信息增益相联系。

看公式容易头疼，还是直接看这个例子吧，依次求解一下各个属性的信息增益，如下图。

![](信息增益.jpg)

尽管公式比较麻烦，实际求解的时候还是挺简单的。“有对象”的信息增益要大于其它两个属性，所以我们应该把“有对象”的测试作为根节点。之前给出的决策树是不合适的，使用ID3算法得到的决策树如下图。

![](第一次分的决策树.jpg)

可以看到，经过第一次属性分解（根部的“有对象”属性），可以得到两个测试输出，而比较凑巧的是，左侧和右侧的输出正好全是yes和no。所以，我们无需再进行属性分解了，决策树就构造完了。

什么时候还需要继续进行属性分解呢？如果左侧的表格，Class值有yes和no，就需要继续分解，因为还没有得到明确的决策。属性分解还是使用计算信息增益的方法求解出当前需要分解的属性。

什么时候分解结束呢？有三种情况：第一种就是我们遇到的这种，属性都属于同一类；第二种是没有训练样本；第三种是没有剩余的属性可以分解了（可以看到，随着属性分解，表格会越来越小，列数越来越少），这时候就需要多数表决来确定，如下图。

![](多数表决.jpg)

注：这里分类的时候只分了yes和no两类，其实可以分成更多的类，而类标号也不一定非得用yes和no，名字随便起啦。所以，决策树也未必一定是二叉树。本例中的Name没有参与到分类中来，因为这仅仅是一个ID。决策树的构建过程其实是一个贪心的过程，OMG，贪心好厉害。

C4.5算法
---

ID3有偏向多值属性的缺点，所以就有了C4.5算法。什么是多值属性，比如主键ID，在每条记录中都不同，所以是多值的，ID3就偏向这样的属性。但是这样的偏向对于划分没有意义。

ID3是使用信息增益来选取要进行分裂的属性，C4.5则使用增益率来选取，即具有最大增益率的属性优先分裂。

所以，仅仅是公式不同而已。

$$split\underline{\text{ }}info(D)=-\sum_{j=1}^{v} \frac{D_j}{D}log_2 \frac{D_j}{D}$$

$$gain\underline{\text{ }}radio_k(D)=\frac{gain_k(D)}{split\underline{\text{ }}info_k(D)}$$

至此，我们终于挖掘出来了新的东西——决策树，这棵树可以帮我们分类哦！分类无处不再，尽情想象吧(╯▽╰)

感谢这篇博文：<http://www.cnblogs.com/leoo2sk/archive/2010/09/19/decision-tree.html>

补充
---

得到的决策树往往还需要剪枝，主要是为了避免决策树过分的拟合样本。什么叫做过分拟合样本？就是完全的根据样本得到决策树，完全考虑样本的每一个细节，哪怕样本里有错误决策树也完全的“服从”。

所以需要对决策树进行适当的剪枝，如何剪枝？参考这篇[博文](http://blog.sina.com.cn/s/blog_68ffc7a40100urn3.html)。

`-- EOF --`