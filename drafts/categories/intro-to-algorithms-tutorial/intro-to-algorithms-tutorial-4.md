动态规划（Dynamic programing）常常用来求解最优化问题，对于决策的帮助还是很大的，有好的谁愿意要不好的呢？可是怎样确定哪个是最好的，或者最好的之一？动态规划就可以帮助你啦。

<!--more-->

但是，动态规划并不是一个算法，应该是一种思想。和divide-and-conquer一样，这是一种和马列主义一样的伟大思想。所以，它除了能解决一些最优化问题以外，肯定还有其它的优势。

看看小伙伴们对动态规划的理解：

>1. 我一向以为，贯穿这无数美妙算法之中的算法思想归根到底来自于两点：一是分治，二是递归（其实严格来讲，分治的算法思想也能解决递归问题）。分治的思想是把原本庞大的问题分解为几个较小的问题，分而治之后再合并，最为典型的例子自然是归并排序了。递归的思想是把原来的问题转换为一个递推式，如果我们想求原问题，那么我们需要先解决子问题，在求解子问题的时候我们发现先需要解决子问题的子问题，然后依此类推。递归算法中的翘楚自然就属于动态规划了。(来自[这里](http://blog.csdn.net/adaifire/article/details/6653496))
>2. 动态规划的实质是分治思想和解决冗余，因此，动态规划是一种将问题实例分解为更小的、相似的子问题，并存储子问题的解而避免计算重复的子问题，以解决最优化问题的算法策略。（来自[这里](http://blog.csdn.net/strollerv/article/details/1535708)）
>3. 动态规划的思想就是先拿起第一个，然后确定拿不拿第2个，拿不拿第2个又取决于拿不拿第3个，整个就是一个递归的过程，最后只剩一个的时候，到达了递归的边界，于是哗啦啦逆向把前面所有的结果算出来。最终能够得到到底要拿哪些。说的专业点，就是把一个大问题拆分成很多小问题，小问题再拆成更小的问题，到最后一个的时候，拆不动了，可以直接做出选择，于是前面的问题也就也就全部解决了。说白了，实际就是把所有可以拿的可能性都遍历了一遍，自然能够选择出最好的了。那我直接遍历就可以了，到底为什么要用这种方式呢，把问题拆成小问题的过程中，其实还有一个步骤，我们一直在不停缓存问题的结果，当下次再遇到这个小问题的时候（在拆分的过程中，会多次遇到同一个问题），就可以直接得到结果，于是大大提高了计算的速度。 而动态规划算法的精髓其实很简单，第一：拆分，第二：缓存。（来自[这里](http://www.iteye.com/topic/324704)）

额，动态规划就是一方法，可以理解为分治思想的增强版，解决了重复计算的问题。为什么每次听到这个名字都感觉很高深呢？！我试图找到动态规划（Dynamic programing）的由来，为什么分治法的增强版不叫“分治法增强版”，反而叫做“动态规划”！MIT公开课（计算机科学及程序导论）给出的解释让我惊呆了：

>Don't try to figure out why it's called dynamic programming. It makes no sense at all. It was invented by a mathematician called bellman, and he was at the time being paid by the defense department to work on something else, and he didn't want them to know what he was doing. So he made up a name that he was sure they would have no clue what it meant. Unfortunately, we now have lived with it forever. So don't think of it as actually being anything dynamic particularly. It's just a name.

就是一个名字而已，小伙伴们不要想多了。动态规划有多难，看看就知道了。

Design technique: Dynamic Programing
===

在开始动态规划之前，需要先了解一下基本概念（来自[这里](http://www.cnblogs.com/sdjl/articles/1274312.html)）：

1. 最优子结构：子问题最优时母问题通过优化选择后一定最优的情况叫做“最优子结构”。
2. 子问题重叠：母问题与子问题本质上是同一个问题的情况称为“子问题重叠”。
3. 边界：子问题在一定时候就不再需要提出子子问题的情况叫做边界，没有边界就会出现死循环。
4. 子问题独立：当前被选择的子问题两两互不影响的情况叫做“子问题独立”。

这就是动态规划，具有“最优子结构”、“子问题重叠”、“边界”和“子问题独立”。如果要解决的问题具备这四个性质的话，就可以使用动态规划方法了。

Matrix Chain Multiplication
---

有一个蛋疼的人不好好吃饭，提出了一个这样的问题：有n个矩阵相乘，$A_1 \cdot A_2 \cdot \dots  A_n$，如何才能使得计算的次数最少，如下图。

![](矩阵连乘.jpg)

对于矩阵的乘法，具体计算的次数是和矩阵的大小有关的，试想一下，如果中间的某两项乘出来的结果是一个1x1的矩阵，这多爽啊，我们肯定要先让这两个计算。

所以，原问题就变成了如何给这n个矩阵加括号，我们先计算括号里面的。PS：矩阵乘法满足结合律。

那我们该如何拆分，即找到最优的一种加括号方法？经过仔细观察和思考，我们发现，可以把A[i..j]分为两部分，A[i..k]和A[k+1...j]，也就是加两对括号，把原问题拆分为两个子问题。那k的哪个位置呢？我们先假设k是一个最优的位置，使得花费的代价最少。

我们已经将很难解决的母问题分解成两个子问题，而子问题还是使用同样的方法来分解，即子问题重叠。

现在可以写出这种花费的递推关系：$Cost(i..j)=Cost(i..k)+Cost(k+1..j)+p\_{i-1}p\_{k}p\_{j}$，其中$p\_i$表示$A\_i$的列值。则这个式子中我们没有考虑到底有多少矩阵相乘，反正给我们“一段”矩阵，从i到j编号，我们就能够得到所有元素相乘次数的和。能这样做的原因在于子问题重叠。

我们期待的最优的情况是Cost最小的情况，重新表示上面的式子：$OPT(i,j)=OPT(i,k)+OPT(k+1,j)+p\_{i-1}p\_{k}p\_{j}$。

现在回到k的问题，我们之前假设了k的位置是一个最优的位置，但仅仅是假设。怎样才能最优呢？用下面的式子可以很好的表示：

$OPT(i,j)= \begin{cases}0 & i=j \cr min\_{i \leq k<j} \lbrace OPT(i,k)+OPT(k+1,j)+p\_{i-1}p\_{k}p\_{j} \rbrace & \text{otherwise} \end{cases}$

现在再看一下这个式子，感觉就不一样了。它很神奇的具有了前面提到的几个特性：最优子结构（这个是通过min来选择最优的子问题，来达到母问题的最优）、子问题重叠（前面提到了，子问题和母问题都是同一个问题，只是规模不一样）、边界（当拆分到不能再分的时候，即i=j的时候，到达边界）、子问题独立（拆分后的两个子问题直接互不干扰）。

剩下的就是编写程序，实现一下就好了。当然，算法的结果不是矩阵相乘的结果，而是一种最优的加括号方式。

Implementation: Trial 1
---

使用字典的方式存放OPT_VALUE，如{'2-8':10}，表示$A_2 \cdot A_3 \dots A_8$相乘的最小花费是10。OPT(i,j)的值会不断的更新，存放当前找到的最小花费，实现代码如下。

```
    import random
    import sys
    OPT_VALUE = {}
    def OPT(i, j, value=None):
        splitter = '-'
        if value:
            OPT_VALUE[str(i)+splitter+str(j)] = value
        else:
            if OPT_VALUE.has_key(str(i)+splitter+str(j)):
                return OPT_VALUE[str(i)+splitter+str(j)]
            else:
                return sys.maxint
    def MatrixChainMult(p, i, j):
        if i == j:
            return 0
        for k in range(i,j):
            q = MatrixChainMult(p, i, k) + MatrixChainMult(p, k+1, j) + p[i-1] * p[k] * p[j]
            if q < OPT(i, j):
                OPT(i, j, q)
        return OPT(i, j)
    if __name__ == '__main__':
        LENGTH = 10
        p = [ random.randint(0,100) for i in range(LENGTH) ]
        p.append(random.randint(0,100)) # save the first matrix's row value
        print p
        MatrixChainMult(p, 0, LENGTH-1)
        print OPT_VALUE
```

算法已经实现了，不禁要问，这样的算法好吗？我们该不该相信动态规划算法？这个算法能不能好过穷举法呢？（好多疑问啊！）

先计算一下时间复杂度吧，看看到底是个什么情况。

$T(4)=T(1)+T(3)+1+T(2)+T(2)+1+T(3)+T(1)+1$，如下图。其中1代表一次计算，经过这一次计算才能知道拆分开的矩阵在最终相乘的时候需要进行几次乘法。

![](矩阵连乘方法一.jpg)

所以，我们能够得到$T(n) \geq 1+\sum_{k=1}^{n-1}(T(k)+T(n-k)+1)$。

这个次没法用主定理了。我们使用一种代入的方法，先假设$T(n) \geq 2^{n-1}$。如果把假设的值代入上面的递推关系仍然成立，那就说明我们假设对了。为什么假设成这个值，万一我假设成别的呢？这个...那就多假设几次吧，有点投机的方法。

代入之后，发现$T(n) \geq 2^{n-1}$仍然成立！但是，这个时间复杂度没人会喜欢的。但至少比穷举法好吧，穷举法的话，随着n的变大，加括号的方式会有迅猛的增长，可以证明是Catalan数。什么是Catalan数？组合数学里见到过它，还有一个是Stirling数，它们是好朋友...

我们继续寻找更好的方法...

Implementation: Trial 2
---

经过认真观察，仔细思考，我们发现了“新大陆”，如下图。

![](矩阵连乘方法二.jpg)

注意红彤彤的小球球！这些是不是重复计算了呢~之前提到说，动态规划是分治+去重复（或者说分治+缓存）。如果去除这些重复的计数，性能将大大的提高。如果去重？把之前计数的缓存起来就可以了呗！我们为什么能够去重？因为动态规划子问题重叠的特性，即遇到相同问题的时候我们不再进行重复计算。

看一下新版的算法，实现代码如下：

```
    import random
    import sys
    OPT_VALUE = {}
    EMPTY = sys.maxint
    def OPT(i, j, value=None):
        splitter = '-'
        if value:
            OPT_VALUE[str(i)+splitter+str(j)] = value
        else:
            if OPT_VALUE.has_key(str(i)+splitter+str(j)):
                return OPT_VALUE[str(i)+splitter+str(j)]
            else:
                return EMPTY
    def MatrixChainMult2(p, i, j):
        if OPT(i, j) != EMPTY:
            return OPT(i, j)
        if i == j:
            return 0
        for k in range(i,j):
            q = MatrixChainMult2(p, i, k) + MatrixChainMult2(p, k+1, j) + p[i-1] * p[k] * p[j]
            if q < OPT(i, j):
                OPT(i, j, q)
        return OPT(i, j)
    if __name__ == '__main__':
        LENGTH = 10
        p = [ random.randint(0,100) for i in range(LENGTH) ]
        p.append(random.randint(0,100)) # save the first matrix's row value
        print p
        MatrixChainMult2(p, 0, LENGTH-1)
        print OPT_VALUE
```

可以看到，代码几乎没有变动，就加了一条判断，测试结果又让小伙伴们惊呆了，如下图。分别测试了8、10、12、14、16个矩阵相乘寻找最优计算顺序的时间耗费。PS：本来是想测LENGTH=20的时候花费的时间，等了好久都没出来结果啊！

![](运行结果对比.jpg)

使用python的timeit模块进行测试，测试代码如下：

```
    OPT_VALUE = {}
    EMPTY = sys.maxint
    LENGTH = 0
    p = []

    def test1():
        global OPT_VALUE
        OPT_VALUE = {}
        MatrixChainMult(p, 0, LENGTH-1)
    def test2():
        global OPT_VALUE
        OPT_VALUE = {}
        MatrixChainMult2(p, 0, LENGTH-1)
    def test():
        global p
        global LENGTH
        from timeit import Timer
        p = [ random.randint(0,100) for i in range(LENGTH) ]
        p.append(random.randint(0,100))
        t1 = Timer("test1()","from __main__ import test1")
        t2 = Timer("test2()","from __main__ import test2")
        return t1.timeit(3), t2.timeit(3)
    if __name__ == '__main__':
        for i in range(4, 9):        
            LENGTH = i*2
            print 'CHAIN LENGTH:', LENGTH
            print test(), '\n'
```

时间复杂度是多少呢？可以观察得出，有$C(n+2-1,2)=O(n^2)$个不重复的子问题。挺好观察的，我们要计算OPT(i,j)，其中$1 \leq i \leq n, i \leq j \leq n$，使用允许重复的组合公式即可。而每个子问题需要$O(n)$的代价来解决，所以时间复杂度为$O(n^3)$。

可以看到，性能有了相当大的提高。但我们并不满足于现在，期待更快些的方法。

Faster implementation: Trial 3
---

还是需要观察，经过观察，我们发现求解的依赖关系是有层次感的。可以先求红色的部分，再求绿色的部分，以此类推，如下图。

![](矩阵连乘方法三.jpg)

这样，就能够得到一个非递归的实现方式，实现代码如下：

```
    import random
    import sys
    OPT_VALUE = {}
    S_VALUE = {}
    def OPT(i, j, value=None):
        splitter = '-'
        if value != None:
            OPT_VALUE[str(i)+splitter+str(j)] = value
        else:
            if OPT_VALUE.has_key(str(i)+splitter+str(j)):
                return OPT_VALUE[str(i)+splitter+str(j)]
            else:
                return sys.maxint
    def S(i, j, k=None):
        splitter = '-'
        if k != None:
            S_VALUE[str(i)+splitter+str(j)] = k
        else:
            return S_VALUE[str(i)+splitter+str(j)] 
    def PrintOptimalParens(i, j):
        if i == j:
            print 'A',
        else:
            print '(',
            PrintOptimalParens(i, S(i,j))
            PrintOptimalParens(S(i,j)+1, j)
            print ')',
    def MatrixChainMult3(p, LENGTH):
        for i in range(LENGTH):
            OPT(i, i, 0)
        for step in range(1, LENGTH):
            for i in range(LENGTH-step):
                j = i + step
                for k in range(i, j):
                    q = OPT(i, k) + OPT(k+1, j) + p[i-1] * p[k] * p[j]
                    if q < OPT(i, j):
                        OPT(i, j, q)
                        S(i, j, k)
    if __name__ == '__main__':
        LENGTH = 10
        p = [ random.randint(0,100) for i in range(LENGTH) ]
        p.append(random.randint(0,100)) # save the first matrix's row value
        print p
        MatrixChainMult3(p, LENGTH)
        PrintOptimalParens(0, LENGTH-1)
```

以上代码同时将之前没有解决的一个问题给解决了——打印出一种最优的加括号方式。之前都是在求如何把OPT(i,j)降低到最小，没有考虑最终打印结果的问题，应该把最优的k值保存起来的。可以看到，这种实现方式的时间复杂度显然是$O(n^3)$。人类是永不满足，又有小伙伴弄出来了个$O(nlogn)$的算法，唉！（Hu and Shing 1981）

折腾了半天，一直在解决矩阵链乘法的问题。这和动态规划有毛线关系啊？

捋一捋...第一种方法，我们用递归的方式求得了最优的加括号方式，但是这种方法不好，有太多重复计算的地方了。动态规划则特别喜欢这样的地方，即子问题重叠，它可以做到不重复计算，大大减少求解的次数。目前来看，动态规划只是递归+去重。第三种方法没有使用递归，因为我们观察发现，这种依赖很有层次感，可以通过循环来依次求解每一层的结果。第三种的特点是采用了自底向上的求解方式，而前两者则采用自顶向下的方式。

所以，第一种递归的方式没有第二种好，因为第二种是递归的加强版，增加了去重。第二种和第三种达到的效果差不多，但是第三种没有使用递归，性能上应该会好些。

那，究竟什么才是动态规划呢？这是一种思想，在这种思想的指导下可以解决很多问题，比如刚才的矩阵链乘法。它都指导我们做了哪些事情呢？

1. 寻找最优子结构：我们找到了拆分问题的方法，当子问题最优的时候，母问题选择一下也能达到最优。
2. 递归定义最优值：经过了第一步，我们很容易的写出了递推关系。
3. 计算最优的值：有了递推关系，我们就可以写代码了，求出来的就是最优值，即OPT，一般采用自底向上的方法解决。
4. 计算最优解：有时候我们不仅仅需要OPT，比如上面的例子需要知道具体如果加括号，需要增加一个变量S_VALUE来解决，也就是就是S_VALUE。

有小伙伴总结了穷举、递归、动态规划的关系：

>1. 穷举=树型搜索
>2. 递归=树型搜索+剪枝
>3. 动态规划=树型搜索+剪枝+节点合并
>4. 因此算法效率：动态规划>递归>穷举。（来自[这里](http://wantnon.blog.163.com/blog/static/123404907200981045659697/)）

现在想想，动态规划指导的算法不一定非要递归实现的，只是一个思想。还是它和divide-and-conquer的关系，不仅仅是动态规划=分治+去重那么简单。分治法的子问题可能和母问题没什么关系，而动态规划则不是，它要求子问题重叠。只能说动态规划也借鉴了分治的思想吧，它更多的用来解决最优化的问题，去重是为了优化，最优子结构是优化的前提。

最后，强烈推荐一篇讲动态规划的[奇文](http://www.cnblogs.com/sdjl/articles/1274312.html)。

`-- EOF --`
