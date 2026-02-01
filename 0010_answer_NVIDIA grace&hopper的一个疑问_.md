# NVIDIA grace&hopper的一个疑问?

> **类型**: 回答
> **作者**: Dio-晶
> **赞同**: 0
> **评论**: 7
> **时间**: 1651374443
> **原文**: [https://www.zhihu.com/question/529177040/answer/2465569783](https://www.zhihu.com/question/529177040/answer/2465569783)

---

姿势的荒漠……只能自言自语么？

我有一些进展，但是还未找到准确的答案。

有同学认为所谓设备化这事并不关键，毕竟手机这么多年，也没见谁把arm mali gpu给设备化了。其实这个的一个大区别是Android和Linux对设备的管理方法不一样，在infrastructure级别，硬件软件还是一种开放分层的方式，这就依赖于各个部件需要建立一套大家认可的框架关系。当然你一定要说nvidia未来走全封闭生态，也不是不可以，但discreted gpu市场还在啊？做两套软件那……有点废人。

生态，那是比女人还难拿捏，能玩死你的。

![](./images/e8cfb08482dda0ac1b6c1899a49ac03b.jpg)![](./images/e8cfb08482dda0ac1b6c1899a49ac03b.jpg)

---

真正的进展是有人私信我当年ibm和nvidia的summit也是标准的设备化方案，不是私有的，可能grace是一样的。这提醒了我。

我再把summit的信息翻了一遍，如下：

1. **超算Summit上power9 cpu和v100 gpu是通过nvlink互联而不是pcie。**
2. **参与项目的同学告诉我软件是标准的linux+cuda driver，但是OS打了一个补丁，也就说不是全标准化。但打完补丁之后，一切正常使用，网上下载通用cuda driver及更新都没有问题。**
3. **今年初nvidia有大量驱动被黑客放到了网上，里面包含了power9 nvlink上的驱动。**
4. **ibm有一篇论文讲power+nvlink performance的论文，google上搜索作者ibm npu team即可看到（艹，家里手机翻不了墙）。**
5. **基于3+4挖掘的信息，是ibm在cpu侧的nvlink端口上模拟了pcie device（模块名叫npu，哈哈），但功能不完备，只有80%左右，有限能用，这个模块叫做npu，ibm还为此专门成立了一个team，但是这个team在summit超算之后再无音讯，大概率是完犊子了。**

![](./images/c0127bd1450c21d6a1a71a05ae5bc7a9.jpg)![](./images/c0127bd1450c21d6a1a71a05ae5bc7a9.jpg)

**事情变得更复杂了。**

**从ibm和nvidia合作来看，nvidia应该也是坚持pci device模式兼容性的，但是这个功能是ibm在cpu侧做的，相关设计很可能并没有被nvidia继承的。会不会是nvidia把ibm npu team 给收了？有这个可能性，但如果是我我不会，毕竟power的isa和arm的isa并不相同，模块规格也许能继承，微架构差异性很大，而且nvlink c2c明确讲了它用的amba chi……**

**那么grace到底怎样发现和枚举hopper呢？**

**猜不到啊！**

![](./images/3c0bd6dc050c6436e09e114c5336ea08.jpg)![](./images/3c0bd6dc050c6436e09e114c5336ea08.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
