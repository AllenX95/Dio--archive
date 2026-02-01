# 又被打脸了，apple M1 ultra

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 397
> **评论**: 41
> **时间**: 1647492471
> **原文**: [https://zhuanlan.zhihu.com/p/482450390](https://zhuanlan.zhihu.com/p/482450390)

---

![](../images/9479196e5b78fec52832c7c8ee7fe919.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='140'%20height='133'></svg>)

最近被打脸的次数有点多，脸都肿了……

apple M1 ultra面世差不多一周了，2.5TB的chiplet带宽，做过chiplet的人才能感受到其中令人绝望的力量，硬生生把物理上的两个die做成了逻辑上的一个die，残暴！壕无人性！

![](../images/45927d9474bdb09eddb25a64e1ab8b1f.jpg)![](../images/45927d9474bdb09eddb25a64e1ab8b1f.jpg)

apple怎么做的，从第一天开始，研发的竞分就展开了，非常迅速。25um pitch互联是其中最关键的一环，IO密度基本等价于带宽。

最初我们还在争论不休，但坊间一篇，唉，传遍四周的文章出来了！ 他以坚定不移的语气告诉大家是cowos-s……第五代……

[苹果芯片“拼装”的秘方，在专利里找到了](http://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/LkhShZ2JBeinPnxhVYDZMQ)

你知道，竞争分析这事，其实和玩狼人杀差不太多，很多玩家其实就是闭眼平民，只能通过已有知识加上经验和逻辑来推理，但是有些玩家身份高，他有信息，所以大多数时候，你得跟着预言家走。

当这篇文章传遍自媒体大地形成共识的时候，领导问你apple咋做的? 其实他内心已经被暗示了。你只能回答，可能是cowos-s吧，大概率apple和TSMC搞了第五代，把40um pitch优化到了25um? 当然也有做工程的领导不相信，这TMD是工程，可靠性什么时候验证的? 又不是写代码！

是啊…………

虽然我在UCIE的文章中也是以cowos-s标注apple的技术……

<https://zhuanlan.zhihu.com/p/480232426>

但是在刀了几个晚上之后，感觉有点不对劲啊。只能赶紧打自己脸了，再不打今晚死透了。

他该不是悍跳狼跳预言家吧！

![](../images/277168ce7c38bb41c724105426e3ad1a.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='158'%20height='106'></svg>)

专利这个东西从来就不是用作产品特性的对照的，谁家写专利按照产品规格写啊，不能这么做，这里面有坑的！

而且这位同学，他英文肯定比我强，但有没有做过chiplet我谨慎存疑吧。

---

**我是暴民我要起跳了！**

闭眼再盘一下逻辑，虽然我在说chiplet的物理层七国八制，但真正呢，也还好-\_-||

**130um是MCM的基线，AMD ZEN CPU在这里**

**55um是EMIB和HBM的基线，INTEL CPU以及UCIE的EMIB版本大概率在这里。**

**40um是FOP（RDL）的基线，鲲鹏920CPU在这里。**

**cowos-S比较奇特，你如果到TSMC官网查询，他标注的是<40um，这个 < 符号确实很坑，我做的时候也是按照40um做，因为这样可以兼容FOP，但标注< ，毕竟意味着至少TSMC测试过更小，但如果没有直接写出来，大概率没产品实操过。**

**然后我们再打开一个TSMC的一个产品，INFO-LSI，标注得很清晰，25um pitch! 尺寸也能对上，M1 ultra两个die尺寸加起来刚好800多，1X reticle size。**

![](../images/89602165f06885fd93bd768ec3684b1b.jpg)![](../images/89602165f06885fd93bd768ec3684b1b.jpg)

那么，亲爱的暴民们。你们相信apple是砸钱高密级和TSMC搞了一个Cowos-s的第五代优化呢？ 还是用了INFO-LSI呢？ 投票啦

![](../images/799c95a38330e47d0016d03cacd65f1b.jpg)![](../images/799c95a38330e47d0016d03cacd65f1b.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
