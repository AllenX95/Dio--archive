# 从GPU谈异构（12）

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 547
> **评论**: 40
> **时间**: 1639287574
> **原文**: [https://zhuanlan.zhihu.com/p/444271740](https://zhuanlan.zhihu.com/p/444271740)

---

如果你真的是jim keller的粉丝，你一定不会错过关于他最新的动态。

![](../images/593eb640d5e8a1d8ccb9e7bff78f3645.jpg)![](../images/593eb640d5e8a1d8ccb9e7bff78f3645.jpg)

是的，就前两天，tenstorrent的芯片在数据中心上线了。

[https://twitter.com/tenstorrent](http://link.zhihu.com/?target=https%3A//twitter.com/tenstorrent)

上线的再前两天，JIM还特地到YouTube上对他家芯片的架构思路做了简短的描述，他这种非PR通稿的访谈，你能得到相当多信息，例如他用了相当长时间赞美模块化设计，更进一步你能想到，这应该是他在intel想要而又得不到的。(･｀ｪ´･)つ

[https://www.youtube.com/watch?v=KOHQQyAKY14&t=1s](http://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DKOHQQyAKY14%26t%3D1s)

～～～～～

**先说结论: 虽然说一万个人有一万个哈姆雷特，但我认为当前tenstorrent的芯片是世界上最好的AI DSA芯片。**

～～～～～

JIM确实是非常强大的芯片架构师，不过我并不崇拜他，我更多的是持续尾行他（和游戏一样的，想办法推倒）。我特别反感国内某些自媒体，给人家戴个什么芯片之神、AMD ZEN之父之类的帽子，到处转发，然后站在道德制高点上指着国内同行，“还不跪下！” 极其SB，其实他们并没有去了解JIM具体做了什么，包括，JIM并不是ZEN之父，创造ZEN的是clark，这并不是什么秘密，那是另一位值得尾行的强者。

不过我倒认为JIM可能算得上这个tenstorrent之父的，讲道理从芯片上线反推其开发时间，架构的定义应该是2019年，此时JIM还在intel就职，但如果你查一下tenstorrent的投资者信息，JIM的参与这个公司应该是相当早的，他应该是参与了这个芯片的架构设计，并且认识到其竞争力的领先，然后才选择了离开intel，纯属猜测，这逻辑没法证明。

～～～～～

tenstorrent有两颗芯片，推理的grayskull和训练的wormhole，架构上是大同小异。因为某些进度上的原因，目前都还不是完全体。其control CPU还用的是ARC，实际上下一代应该都会替换成高性能的RISC-V core（买SIFIVE和自研两路并行，稳的一比）。下面一张图把tenstorrent的技术理念展现得非常充分，懂得自然懂。

![](../images/fdeaf038c13f11413b536c5b262b3bf3.jpg)![](../images/fdeaf038c13f11413b536c5b262b3bf3.jpg)

这个图展示不了力量，那他的强大如何表现?

我比较喜欢看训练的大家伙。

**1、他用12nm的工艺，做到了几乎世界第一的FP16的TFLOPS/W的能效。**

**2、全RISC-V生态，通用C语言编程，开放生态。**

**3、AOT compiler + 轻量级runtime。**

～～～～～

先说第一点，我们可以打开MIT比较有名的一篇survey: [https://arxiv.org/pdf/2109.08957.pdf](http://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2109.08957.pdf)

![](../images/0a72aa156c0578f912ae1f931f293439.jpg)![](../images/0a72aa156c0578f912ae1f931f293439.jpg)

有一张全球AI芯片能效比较图，其实这张图不是特别准确，因为他把训练、推理、单机、系统的数据都揉在一起，但大的趋势是没有特别问题，值得参考。

![](../images/1672003943516bf8715f03ebb282aaff.jpg)![](../images/1672003943516bf8715f03ebb282aaff.jpg)

图中越偏向左上角的芯片能效越强，我们可以看到很多有名的公司和芯片，包括: TPU、A100、V100等等，可以看到tenstorrent在相当靠左上的位置（这里只统计了它的推理芯片）。与他能相比的，例如mythic等其实都是数模混合AI芯片，你懂的，而万亿美元市值公司nvidia的A100和V100在什么位置都是清晰可见的。简单列一下官网的数字嘛，nvidia A100的FP16能效是312Tflops/400W，而tenstorrent的wormhole是180Tflops/160W（or 130W），而国内要吊打nvidia的寒武纪/燧原什么的做的训练芯片，算力和功耗是多少知乎都能查到的，我就不多说了。反正，你在这个行业坐久了会理解，**1Tflops/W的FP16能效（单芯片100T以上）**，是AI训练芯片一个巨大的台阶，能跨过的人，很少很少，tenstorrent很轻松地跨过了。

～～～～～

第二点，全RISC-V生态，虽然我更喜欢ARM+RISC-V，但全RISC-V心里也能接受。

<https://zhuanlan.zhihu.com/p/376409878>

通常大家都认为，能效高的芯片芯片编程能力就弱，如果兼顾编程友好度，那么能效就不够好。

很多领导不是都喜欢下面这句话吗？

![](../images/e56ed59a0f570b39d1a4c94e409b21cb.gif)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='400'%20height='224'></svg>)

咯，给你给你，敢不敢要（抄）啊?

![](../images/234435c344e93433e41b9350e90c9f83.jpg)![](../images/234435c344e93433e41b9350e90c9f83.jpg)

全开源生态，从Python、tensorflow到linux、开放存储、RISC-V开源指令编译器。这编程友好度不香吗？

很多人不明白RISC-V怎么做AI，下面这张图很清晰，基于计算和控制的分工，你需要大核跑OS和STACK，小核跑compute。

JIM在第一代芯片没有搞定大核（要是我就买ARM N2），他用了ARC（后续换RISC-V），而小核负责计算，用的是5个RISC-V core共享AI matrix ISA扩展的思路，其实这个思路，和TESLA的DOJO是同出一脉，tesla的计算核其实也是RISC-V，不同的是DOJO是SMT4，而tenstorrent是5cores，其实啊，在RISC-V这个轻量级core下SMT4/8和multi-core已经没有太多区别了（intelXe GPU除了指令集不同，微架构的选择也是如此的）。

![](../images/308d9fc85a2910b1b31e788d5d18d7e5.jpg)![](../images/308d9fc85a2910b1b31e788d5d18d7e5.jpg)

～～～～～

如果单纯讲tenstorrent就天下第一，好像并不能服众。同样能打的，还有tesla的DOJO、和AWS最近刚推出的EC2 trn1，水准都能排的上号。但是DOJO嘛，他选择了做SOW，做一个大机器训练大模型。只有telsa这样的富婆包养，讲好了少奋斗二十年才能玩得起啊，而AWS的TRN1呢，他和Google的TPU是一脉相承的路数，他是牺牲了一定可编程性，用大号的systolic获得的能效，systolic相比内积或者外积最大的优势是频率可以做很高，是的，AWS直接奔3GHZ了，但问题就是，tiling不好做啊，要塞满一个大尺度的matrix，编程很麻烦，不知道AWS这是怎么考虑的，但是AWS足够大，也许通过某些特别的办法? 不知道。

gyrfalcon、mythic是一个流派，PIM

Google、AWS是一个流派，大矩阵

AMD、Nvidia是一个流派，GP

tenstorrent、tesla、cerebras，是一个流派，多核空间计算

即使打开下面这个帖子，坊间热捧的诸家AI公司也大体如此。

<https://zhuanlan.zhihu.com/p/443789867>

最前面的sambanova不属于上面任意一种，而是CGRA的路子，这个路子嘛，我个人不看好，水平不够，看不懂为啥呢这么高估值。

graphcore和cerebras其实和tenstorrent都是manycore+spatial computing的路数，但是前两者都是自定义的封闭ISA，不喜欢。

groq呢，其实是在Google路数上更加激进的做法，他把整个芯片做成了一个单核CPU的样子……对于这种SM级变态，我当前变态的程度还不够，接受不了。

所以，综上来讲，如果寻遍诸子百家，我还是最喜欢tenstorrent，JIM的路数。如果要我失业了来做一颗AI芯片，如果不是考虑GPGPU要做兼容并包，只对AI做DSA的话，毫无疑问tenstorrent是唯一的学（抄）习（袭）对象。

不过我稍微还是有点点疑问? 为什么这么久，国内的AI公司就没人学习他呢? 看不上吗？

**能效吊打国内（含nvidia）、编程方便、jim keller站台，还有一个极大的隐含甜点: 这是加拿大公司，加拿大技术，开源生态，无论是收购、投资或者技术授权都比美丽国的公司或者技术的可行性高了数百倍。 为啥⊙ω⊙ ?**

![](../images/86ae50a98b64386a5c4ea620bda10baf.jpg)![](../images/86ae50a98b64386a5c4ea620bda10baf.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
