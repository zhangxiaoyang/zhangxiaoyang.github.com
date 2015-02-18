算法设计——程序设计中的艺术。

<!--more-->

Introduction
===

为什么这么说呢？

V. Vazirani在[《Approximation algorithm》](http://cs.gzu.edu.cn/lab/upload/cont/资料中心/近似算法.pdf)一书中提到：

>Our philosophy on the design and exposition of algorithms is nicely illustrated by the following analogy with an aspect of Michelangelo’s art. A major part of his effort involved looking for interesting pieces of stone in the
quarry and staring at them for long hours to determine the form they naturally wanted to take. The chisel work exposed, in a minimalistic manner, this form. By analogy, we would like to start with a clean, simply stated problem
(perhaps a simplified version of the problem we actually want to solve in practice). Most of the algorithm design effort actually goes into understanding the algorithmically relevant combinatorial structure of the problem. The
algorithm exploits this structure in a minimalistic manner. The exposition of algorithms in this book will also follow this analogy, with emphasis on stating the structure offered by problems, and keeping the algorithms minimalistic.

此书的封面中也显示了一些米开朗基罗的手稿。

![](图书封面.jpg)

可以看到，V. Vazirani很强调算法设计的艺术性。而算法课的卜东波老师也引用V. Vazirani的话，作为本门课的开篇。

原来以为算法就是解决问题的方法，我们学算法就是学一下别人设计好的很有名的算法。

现在想想，其实不然。

我们应该学会设计有艺术性的算法，美丽的算法。

就像《Algorithm design》中所说的：非常clean，同时我们要证明算法是正确的，并且算法所花费的时间是我们可以接受的。具体的算法设计过程如下：

![](算法设计过程.jpg)

感觉这样想还是太笼统抽象了~最好有一个例子，让我们感受一下有艺术细胞的算法。

果不其然，卜老师给出了第一个例子——Stable Matching。这可不是一个普通的算法，这个可是获过诺贝尔奖的！

OMG！赶紧收拾一下心情，看一看艺术大师的作品！

Representative problem - stable matching
===

算法的提出可能是为了解决某些现实问题，也就是这个natural and practical concerns。

有一天，David Gale对好基友Lloyd Shapley说：能不能设计一个算法，用来帮助学校招生或者公司招聘之类的问题，对了，关键是要self-enforcing！

Shapley困惑了有木有：什么是self-enforcing？这个月的补贴还没发呢你这是要闹哪样啊！再说了，现在的招生和招聘有什么问题么？

Gale必须得说服Shapley一起来解决这个问题，因为N年以后是要获诺贝尔奖的啊！

Gale：Shapley贤弟，莫着急，且听我慢慢道来...

你想想看，你去烧饼店和肉火烧店应聘，由于你能力出众，他们两家都决定聘用你。而你心里更喜欢肉火烧这个行业，怎么办？你肯定会拒绝烧饼店的聘用了，挥一挥衣袖去肉火烧店工作。
这样还没完，如果和你一起去面试的小黄也拒绝了烧饼店，去肉火烧店呢？
......
这样想一下，烧饼店岂不是很可怜！最后没招到人，老板一个人兼任所有职务，还有没有天理，不得把他累死吗！

Shapley：貌似有点道理，那你说的self-enforcing应该就是一个美好的结局呗，老板招到人了，而应聘者也去了他们想去的地方。

Gale：差不多这个意思吧。帮助社会谋福利，还不加入我！

Shapley：好吧，为人民服务。对了，我学过算法课，面对问题不要慌，应该先捋一捋。给出一个clean and simple statement of a problem。那我就把这个问题叫做stable matching problem吧。

下面我来描述一下这个问题：

找一个稳定的匹配（stable matching），稳定匹配的两个人愿意白首偕老，在茫茫人海中他们两个都觉得对方是最合适的。（可能他们两个中的某人觉得人群中还是有更好的，但是那个更好的人不会成为小三，因为他也心有所属了。）

Gale：我也上过算法课！描述完问题了，下面由我来建模（Formulation）吧！

Input：n个男生和n个女生，他们每一个人的脑海中都心动女生、男生的排行，也就是说他们心里其实都对异性编好号了，最喜欢的编号为1，第二喜欢的编号为2，以此类推。

Output：稳定的匹配，即生成n对情侣，他们里面没有不稳定的情况存在（如，两对情侣见面后，某男生和对方的女生好上了，如下图）。

![](不稳定匹配.jpg)

Gale：3对情侣的情况，我们也需要考察一下，看看什么叫做不稳定的匹配。其中，Xavier、Yancey、Zeus为三位男生，Amy、Bertha、Clare为三位女生。用两张表来分别表示男生和女生对异性的排名。

![](3对情侣的不稳定匹配.jpg)

这里面有不稳定的匹配吗？有！Xavier和Bertha就是，估计他们俩一见面就会好上，原来的男朋友女朋友就都不管了，这就是爱情，唉。

Shapley：貌似这是在考眼力啊，完全可以PK找你妹了！怎样才能确定不包含不稳定的匹配，即找到稳定的匹配呢？讨论了半天，也不给个算法，能不能先设计个alpha版的算法啊！

Gale：我倒是想出来了个**improve strategy**，且听我絮叨絮叨。直接看伪代码吧，很简单，Basic idea就不再赘述啦。
 
```
随机的把男生、女生配对
while 存在不稳定的匹配
    随便选择一个不稳定的匹配
    执行一次SWITCHING操作，即把当前的不稳定性变成稳定
```

举个例子吧，这个算法成功将unstable变成了stable，如下图。

![](算法成功的情况.jpg)

Shapley：有成功的例子但不能说明你的算法一定就正确啊！我就找到一个不成功的例子，如下图。你看，经过各种SWITCHING，最终回到了初始状态，死循环！

![](算法失败的情况.jpg)

Gale：啊？这都被你找到了！那怎么办啊，呜呜呜！

Shapley：没事！你的算法给了我很多启发啊，我搞了个**increment strategy**，看看这个有没有破绽。

Basic idea：与improved strategy不同，这个算法是从局部的匹配一步步增量扩展到全部都稳定匹配。（Improved strategy是先把所有的都匹配起来，不管稳定不稳定，然后去消除不稳定的匹配，即SWITCHING。）

具体的方法是抽象出一种propose-engage的过程，男生负责求婚，女生负责接收和拒绝。也就是说，男生依次从自己的偏好列表中选一个妹子去求婚，如果妹子答应了，男生就要负责到底。而女生在已经接受求婚的情况下仍然可以接受更好男生的求婚，原来的求婚就作废了。这时候，男生只能继续寻找适合自己的妹子，曾经抛弃自己的妹子他不会再去打扰了，祝他们幸福。具体的伪代码实现：

```
    for m = 1 to M //存放男生是否已经配对
        m_state[m] = FREE
    for w = 1 to W //存放女生是否已经配对
        w_state[w] = FREE
    while TRUE
        if 男生都配对了
            return
        随便选择一个没有配对的男生
        w = 男生还没有求过婚的妹子（从偏爱列表中依次查找）
        if w_state[w] == FREE //这个女生没有配对
            m_state[m] = w //男生已经和w配对
            w_state[w] = m //女生已经和m配对
        else if w更喜欢m，准备抛弃原来的w_state[w]
            m_state[w_state[w]] = FREE //男生被抛弃了，继续光棍
            w_state[w] = m //女生向新的男朋友示爱
            m_state[m] = w //新的男生顺利挤掉女生的旧爱
        else //女生已经有男朋友了，各方面条件要比现在这个求婚的要好
            //什么都不做，表示拒绝求婚
```

Gale：NB啊！我还真没发现有什么破绽，这个propose-engage的思想很赞啊！

Shapley：嘿嘿，多亏了你的启发~既然一眼看不出来对不对，那就证明一下吧！

我们分两步来证明该算法的正确性。

1. 证明所有的男生、女生都将配对  
证明：假设有一个男生直到算法结束也没有找到女朋友。既然有一个男生处于单身状态，那么肯定也有一个女生处于单身状态（总不能有某个男生找了2个女朋友吧）。既然有一个男生和一个女生都是单身了，他们还没有在一起，只能说明这个男生没有去和这个女生求婚（如果求过婚，女生肯定永远都处于有男朋友的状态）。不对啊，通过伪代码我们看到，男生会像所有的女生求婚的，直到找到一个女朋友，这样就矛盾了。这样，通过反证法，我们得到了第一个结论，即最终男女都将配对。

2. 证明这种配对是稳定的  
证明：假设有一个男生直到算法结束也没有找到稳定的匹配。也就是说，这个男生喜欢一个比现在的女朋友更好的女生，而刚好那个女生也喜欢这个男生。我们需要分情况讨论一下了：  
(1) 男生没有和这个女生求过婚。捋一捋，男生和当前和他在一起的妹子求过婚，和那啥他更爱的女生没求过婚。这不矛盾了吗，因为男生肯定是按照偏好列表的顺序依次求婚，先和谁求就说明更喜欢谁。  
(2) 男生和这个女生求过婚。既然求过婚了又没在一起，说明那个女生有更喜欢的人了。既然有更喜欢的人了，怎么可能和这个男生玩暧昧呢？依然是和假设矛盾。

所以，我们的算法是正确的。

其实，还有一个问题：我们每次产生的稳定匹配是不是都一样呢，会不会出现两种稳定匹配的情况？答案是不会，即稳定匹配是唯一的，此处不给予证明了。

Gale：果然是正确的算法，性能怎么样啊？让我来求一下时间复杂度吧！

通过伪代码可以看到，当男生都配对的时候就会结束算法。所以，最坏的情况是延长男生配对的时间，也就是增加男生求婚的次数。求婚次数最多最多也就是$n^2$，即每一个男生都求n次婚。所以，时间复杂度为$O(n^2)$。

就这样，好基友不搞基搞出了诺贝尔奖。当然，具体的科研过程肯定不是我调侃的这样啦~

通过第一个伟大的例子——stable matching可以学习到算法设计的基本思路。

大致是：寻找一个问题 → 简单清晰的描述问题 → 建模 → 设计算法（basic idea和伪代码） → 证明算法的正确性 → 分析算法的复杂度。

附录
===

Stable matching python实现代码，测试数据[点我](http://pan.baidu.com/s/15Tlyf)下载。

```
    import random

    def ReadToDict(filename):
        preferred_dict = {}
        try:
            file = open(filename)
        except:
            print 'can not find file:', filename
            exit(-1)

        for line in file:
            name = line.split(':')[0].strip()
            
            preferred_list = []
            for preferred in line.replace(name+':', '').strip().split(' '):
                preferred_list.append(preferred)
            preferred_dict.update({name:preferred_list})
        file.close()
        return preferred_dict

    def FindABoy(boys_state):
        while True:
            index = random.randint(0, len(boys_state)-1)
            if boys_state[index] == 'FREE':
                return 'B' + str(index)
                
    def FindAGirl(preferred_list):
        for i in range(len(preferred_list)):
            if preferred_list[i][0] == 'G':
                preferred_list[i] = preferred_list[i].lower()
                return preferred_list[i].upper()

    def StableMatching(boys_dict, girls_dict):
        boys_state = [ 'FREE' for i in boys_dict ]
        girls_state = [ 'FREE' for i in girls_dict ]

        while True:
            if 'FREE' not in boys_state:
                return boys_state
            boy_name = FindABoy(boys_state)
            boy_index = int(boy_name.replace('B',''))
            girl_name = FindAGirl(boys_dict[boy_name])
            girl_index = int(girl_name.replace('G',''))
            if girls_state[int(girl_index)] == 'FREE':
                girls_state[girl_index] = boy_name
                boys_state[boy_index] = girl_name
            elif girls_dict[girl_name].index(boy_name) < girls_dict[girl_name].index(girls_state[girl_index]):
                boys_state[int(girls_state[girl_index].replace('B', ''))] = 'FREE'
                girls_state[girl_index] = boy_name
                boys_state[boy_index] = girl_name
            else:pass
        return 'Never be here'
    if __name__ == '__main__':
        boys_dict = ReadToDict('boys_rankings.txt')
        girls_dict = ReadToDict('girls_rankings.txt')
        print StableMatching(boys_dict, girls_dict)
```

`-- EOF --`
