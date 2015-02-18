老师讲课的思路甚是清晰啊！第一节介绍算法的设计过程。第二节介绍算法的分析。从第三节开始讲算法的设计，divide and conquer、DP、greedy等等，一大波精彩算法即将到来！

<!--more-->

Design technique: Divide-and-Conquer
===

在不知道divide-and-conquer以前，用过一个很火爆的平台——分布式平台（Hadoop）。这个平台现在已经越做越大，像是一套有生命的生态系统啦。但是Hadoop基本的没有变，通过MapRreduce编程模型来实现分布式程序的编写。

MapReduce，Divide-and-Conquer？思想应该是一致的：将原问题分解为规模较小的子问题，这些子问题类似于原问题，通过合并子问题的解来建立原问题的解。《算法导论》一书中，提到通过递归的求解子问题，来构造原问题的解。所以，这两者还是有些区别，但分而治之的思想没有变。

想想自己写的程序，很少情况去分而治之。所以，这就有了另外一个策略，增量法（Incremental strategy）。咦，貌似前面见到过这东西。是的，第一节的stable matching算法就是用的这个策略，通过局部匹配来一步一步构建总的匹配。

增量法不错啊，用这个方法都获得了诺贝尔奖。而分治法也不错啊，Google的MapReduce多牛啊。看来，只要用好了，都能取得很不错的结果，各有千秋。

先看一下增量法（Incremental strategy），出场代表是插入排序（Insertion sort）。

Incremental strategy
---

对于插入排序，最典型的案例莫过于摸扑克牌。左手拿扑克牌，右手摸牌，摸到后到左手的地方选一个位置插入，如下图。

![](插入排序示例.jpg)

代码实现真心不难，比起快速排序，写这个代码花的时间几乎可以忽略不计了~

```
    import random
    def InsertionSort(arr):
        for i in range(len(arr)):
            key = arr[i]
            j = i - 1
            while j >=0 and arr[j] > key:
                arr[j+1] = arr[j]
                j -= 1
            arr[j+1] = key
            
    if __name__ == '__main__':
        LENGTH = 20
        arr = [ random.randint(0,100) for i in range(LENGTH) ]
        print arr
        InsertionSort(arr)
        print arr
```

很显然时间复杂度为$O(n^2)$，当然，我们也可以由$T(n)=T(n-1)+cn$很专业的得到最终的结果。对了，这个地方很明显不能用主定理吧，b应该大于1。

Divide-and-Conquer
---

归并排序（Merge sort）是使用divide-and-conquer思想排序的典型代表。归并排序把要排序的序列分解两个**独立的**子序列，即divide阶段；递归的把子对象分解，即conquer阶段；合并中间的结果，即combine阶段，如下图。

![](归并排序示例.jpg)

Divide阶段：把含有n个元素的序列分解成2个含有$\frac{n}{2}$个元素的子序列。

Conquer阶段：对子序列递归调用归并排序。

Combine阶段：把2个子序列合并成排好序的状态。


咦，MapReduce的过程是Map、Reduce、Combine，真是像！回到归并排序，我们实际上只需要divide函数和combine函数，conquer只是递归调用divide，实现代码如下。

```
    import random
    def Merge(arr, left, m, right):
        L = [ i for i in arr[left:m+1] ]
        R = [ i for i in arr[m+1:right+1] ]
        i = 0
        j = 0
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[left] = L[i]
                i += 1
            else:
                arr[left] = R[j]
                j += 1
            left += 1
        if i < len(L): arr[left] = L[i]
        if j < len(R): arr[left] = R[j]
        
    def MergeSort(arr, left, right):
        if left < right:
            m = (left+right)/2
            MergeSort(arr, left, m)
            MergeSort(arr, m+1, right)
            Merge(arr, left, m, right)
            
    if __name__ == '__main__':
        LENGTH = 20
        arr = [ random.randint(0,100) for i in range(LENGTH) ]
        print arr
        MergeSort(arr, 0, LENGTH-1)
        print arr
```

下面我们来证明一下该算法的正确性。我自己都晕了，前面的插入都没证明，这样的算法还需要证明么，必然正确啊！额，其实是想通过这个算法引出循环不变式（Loop-invariant），这个东西对于分析一些递归算法的正确性很有用。

但是，这个东西到底是什么东西呢？是一个式子？一个变量？一个想法？额，经过不停的搜索，和小伙伴讨论，得出的结论：

>原来循环不变式就是一个循环不变的东西，这样的东西可能有很多，但是想找到能证明最终的结论的却不太容易不要以为你找到的就是循环不变式，你要证明它在开始和中间都是不变的，要不然我为什么要相信这是一个循环不变的东东。

所以，循环不变式和数学归纳法一样，都是一种方法。要使用这个方法必须有前提条件。使用循环不变式的前提条件是，有这么一个量，很有可能是个变量，它在循环开始前和中间的过程中不变。不变的意思不是值不变，而是特性不变，对于归并排序来说，不变指的是一直处于排好序的状态，这个状态没有变。

只要我们证明的它在开始和中间都是不变的，就会得到一个结论，即循环结束的时候它还是不变的。对于归并排序，是指循环结束的时候仍然是排好序的状态。

当然，对于归并排序来说，直接证明了算法的正确性。其它的算法则未必，我们可能得到了结论对于证明算法的正确性有帮助，但不一定能直接证明算法的正确性。

了解了循环不变式，我们来证明归并排序算法的正确性。分两步来证明，即循环开始的前和循环执行的过程中，我们能得到循环不变式。

1. 初始化：arr[left..right]里面没有写入任何元素，即为空，可以认为是排好序了。
2. 保持：因为L和R是排好序的，每次都会把L[i]和R[j]中较小的装入arr中，所以arr还是处于排好序的状态。
3. 终止：我们可以得出在循环终止的时候，arr依然为有序。

所以，可以证明算法的正确性。同时，我们也能得到以下递推关系：

$T(n) = \begin{cases}c & n=2 \cr T(n/2) + T(n/2) + cn & \text{otherwise} \end{cases}$

通过主定理，我们可以很轻松的得到时间复杂度为$O(nlogn)$。（因为$n^{log_2 2}$和$cn$大小差不多，所以需要用主定理的第三条。）

`-- EOF --`
