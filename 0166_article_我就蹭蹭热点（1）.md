# 我就蹭蹭热点（1）

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 110
> **评论**: 9
> **时间**: 1624293233
> **原文**: [https://zhuanlan.zhihu.com/p/382739151](https://zhuanlan.zhihu.com/p/382739151)

---

最近各个IC相关自媒体突然学会了一种发文模式，就是直接用浏览器的翻译模式翻译外网的热点网文，然后直接起个标题党转载起来。

坦白的说，也不是坏事，毕竟让更多民众和领导看看业界的情况，扫除铁流之余的文革荼毒，利大于弊。弊端一个是机器翻译毕竟还是有些毛病，二个是某些自媒体喜欢掐头去尾，引用某些章节然后加戏自high。也没多大事，毕竟上过本科，还能读懂英语，真感兴趣可以读一读原文，写一写心得，进一步解读一下，说不定某天领导看到了帖子问起来还能对答如流。

[Jim Keller：在指令集上辩论是一件悲哀的事情](http://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/Nan90UWX7bJc8fwxvgVpgA)

上周的一个热点莫过于jim keller的采访，上面这一篇算是最有媒体界节操，整理得比较清晰并且也没太加戏的。什么机器之心、新智元的，就XXOO了。

这篇采访原文很长，要真认真读一下，其实可以看出jim在所有技术问题上是比较偏中性的，并没有特别表达某种指向性的观点。包括什么“指令集的辩论是一件悲哀的事情”，他就是不想去引战的。当然，其中讲了很多因推斯汀的story，是非常有意思的，例如他更希望自己在ZEN的定位不是父亲，而是亲爱的王叔叔…………嗯 也许很到位哦(ಡωಡ)

jim讲了很多关于他的个人特质和对工作的理解，很多地方让我不谦虚地觉得和他有不少相似点。对各种技术方向喜欢用相对用中立的方式看待（某种意义和稀泥）；即使做架构也想着去写RTL（我写RTL时也算是人肉综合器了）；很尊重体系结构的各个抽象层次（随意破坏层次的跳线一定是自食恶果）；喜欢做新的东西，而不喜欢在成熟产品上纠结（我也喜欢，成熟的产品更迭充满了政治的味道，而新领域的难度天然就隔离了很多内卷怪）。

但唯有最后一点我对他的选择稍有异议，也许是没缺乏他那种自由自在的资本和游侠精神吧。要取得最后的胜利，并不仅仅是初始令人兴奋的冲锋，此后的固守、拉锯、攻坚，直至登顶，也许伤痕累累，亦得咬牙坚持啊。世事如此。

![](../images/e6c4041ac59b0cad6cc664224db55114.jpg)![](../images/e6c4041ac59b0cad6cc664224db55114.jpg)

随感而发，还是回到技术上吧，JIM谈到了他目前正在做的新东西，看他在新的一张白纸上画了啥? 一个CPU+DSA的架构的摸索。我们应该看一下这个，才可以更方便理解他在采访中表达的技术点背后的含义。

[https://www.youtube.com/watch?v=Id3enIOAY2Q](http://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DId3enIOAY2Q)

他在tenstorrent新公司搞的新玩意儿，如他所说，这是一个AI领域的spatial computing，最新的叫wormhole，如果算开发时间，JIM从他在intel的在职期间就开始兼职搞这玩意儿了耶（我冒昧一下猜RAJA是这东西的王叔叔? ）。

wormhole有点像graphcore及cerebras的产品，空间级模型并行，相比前者在核的尺寸上更大，相比后者增加了主存，并且用某些技巧减少了片间的数据带宽（即不用wafer scale）。这款芯片已经回片，预计在2021年年中将把sample给到客户（有谁知道他们的客户到底是谁吗？ 完全没听说）。

![](../images/bde818fec98facefbca9ecc932c8bd01.jpg)![](../images/bde818fec98facefbca9ecc932c8bd01.jpg)![](../images/2db74033c13b5479038202a60fe6d3db.jpg)![](../images/2db74033c13b5479038202a60fe6d3db.jpg)

die photo长的是下面这个样子

![](../images/a7a8c24189161ea2f02ee5b9b78227c7.jpg)![](../images/a7a8c24189161ea2f02ee5b9b78227c7.jpg)

主要特征解读一下：

1、4个ARC core负责控制面和OS，数据面是80个tensix core。

2、芯片算力大约是184T FLOPS FP32算力，130W。

3、tensix core比其他graphcore和cerebras的都大，这也许就是他在采访中提到的，AI的计算单元不应该像GPU那么小的说法。这个core很像是CPU的结构上改的，因为算下来每个core的算力大约是2TFlops/GHz的样子，很像是每个cycle两个64B cacheline，也就是32x32的FP16矩阵乘法。

![](../images/781c608a3fe697c6f43912a2e85c435e.jpg)![](../images/781c608a3fe697c6f43912a2e85c435e.jpg)

4、芯片floorplan是up/down出IO，和介绍中的2D MESH组网并不相同，很奇怪逻辑上如何映射。

5、采用place & routing的方式编译计算任务，是典型的spatial computing模式，奇怪的是，所谓spatial讲道理就得有所谓的空间感，即越近的tile之间交换带宽越高，越远的带宽越低，那么，访问DDR的带宽在compiler上是怎么识别的?

6、在tensix core上执行的任务叫做mini tensor，讲道理应该是他们定义的一种DSL，这种DSL抽象得比较好，能够简洁地描述大多数AI计算的算子。

7、大概率和昇腾架构一样，mini tensor并不是图灵完备的，有很多算子无法计算或者是DSL无法描述，所以一定需要几个标准CPU来辅助AI完整任务的执行。

8、坊间传言JIM在intel期间主持收购了netspeed，为了革命intel的互联技术，这个收购现在看上去是失败的，那么，这颗芯片的NOC的技术是从何而来?

9、RISC-V你赢了，JIM确实喜欢RISC-V。基于第7点，tenstorrent需要性能更好的CPU来完善其tensix core的不完备，所以他们在下一代将放弃ARC架构的CPU，而选择RISC-V，嗯就是最近风口浪尖的sifive的x280 core。

[https://www.sifive.com/press/tenstorrent-selects-sifive-intelligence-x280-for-next-generation1](http://link.zhihu.com/?target=https%3A//www.sifive.com/press/tenstorrent-selects-sifive-intelligence-x280-for-next-generation1)![](../images/1791b805f4ca8e9e55819e64e8fdae98.jpg)![](../images/1791b805f4ca8e9e55819e64e8fdae98.jpg)

嗯，断章取义的自媒体你赢了。

![](../images/b99c6c434086a49472a019e7fc4bc50a.gif)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='308'%20height='269'></svg>)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
