# Load/Store over ETH 乎？

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 278
> **评论**: 26
> **时间**: 1725275301
> **原文**: [https://zhuanlan.zhihu.com/p/717851262](https://zhuanlan.zhihu.com/p/717851262)

---

刷到了Zarbot一篇文章，看着挺有意思的 ：）

[HotChip2024后记: 谈谈加速器互联及ScaleUP为什么不能用RDMA](http://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/qLRC3dv4E93LwWXtuhQcsw)

想起前两个月和朋友喝茶，谈到这事，大致的观点还蛮相似。

首先，别惦记Nvlink。那是Nvidia征服世界的关键武器，Nvidian宁可开放Nvlink-C2C，也不会开放Nvlink，所以再好也没法选。其次，也先放下某些大公司大平台下才有的XX能力和YY能力，虽然我写过些主张，但那还是屁股决定的。。。。。。。

---

哇，我一睁眼，转生了。 我是一个AI GPU Startup公司的架构师，公司有200来号人，还有大约5亿RMB还能烧，额，还得为流片预留2亿，那也就烧个1年半的。。。。。。。互联怎么选。买？做？能不能白嫖？

业界通常把AI互联分割成Scale-up和Scale-out，前后者带宽相差10x，假定前者1TBps，后者100GBps吧。

其实这两个问题有一个公共的麻烦，就是Switch，若买Nvidia GPU，必须要买Nvidia的Switch，甚至于如果Scale-Out选择IB，也只能选Nvidia。所以你即使想要替代Nvidia，先不说能力够不够做一个Switch，即使能，搞不好客户就坚决反对了，绑定了一个还不够，还绑两个？当然，如果参考DGX，做个8卡 All2all全相连，对外再走ETH，Scale-Up是可以免Switch的，但这种Topology毕竟属于老派，若未来有更大的Scale-up空间也没法支持，还会被客户质疑持续演进的能力。

看一下可选项：） 没有Nvlink和UB，剩下的就是UEC、UAlink、ETH-X。

- UEC是基于标准ETH的协议，BroadCom、CISCO甚至Nvidia（MLX）都在里面，Switch的问题瞬间解决了，甚至国产也问题不大。其上的协议则是RDMA改，这个协议做Scale-out挺好，协议也倡导如此，但并没有解决Scale-up的问题。当然，ETH+RDMA只要做大带宽，也能做Scale-up（也就是Zarbot说的RDMA Scale-up）。但是，还好我有过一点设计网口的黑历史，我盘指一算，做一个1TBps的RDMA，经验告诉我，需要~4000人月（不算软件），把手上的钱和人全烧掉都做不出来。

- UALink是Broadcom的PCIe团队勾搭AMD诞生的，其底层其实是PCIe改，上层是Load/Store，当前没有cache一致性，这没关系，AI也用不上。嗯，不用设计复杂的RDMA，但Switch很明显有很多隐患，这Switch只有BroadCom一家可供，联盟里面有CISCO，但很明显此人没有真做类PCIe Switch的打算，想一想毕竟PCIe Switch和ETH Switch不太一样（和Silicon One差太多）。 除非有人楞一个出来看看，否则搞得不好这事又卡在Switch上了。

- ETH-X很多人说他是UEC的白手套，但你真聊下来有点不一样。这个协议添加了中国那种朴素的实用主义，把UEC引入到scaleup域了，所以他底层虽然是ETH标准协议，但上层其实包括了RDMA + Load/Store多种行为，可以协商再定制（好像最新看只有rdma没有loadstore？ 那个真是个彻底的错误）。这其实满关键了，Scale-out是可以买一个ETH-X的DPU走ETH+RDMA改的，而Scale-up，最佳的答案，就是基于ETH-X做Load/Store+ETH即可。Switch客户自己买兼容ETH-X的标准ETH Switch即可，国内好多Switch公司都表示小改一下就能出来。

站在Startup的角度，能白嫖就不要付一分钱。

统一Scale-Up/Scale-Out的底层，怎么看都还是ETH是最佳的选择，扁平、开放。

如果Scale-up做不出极高带宽的RDMA，用Load/Store就最简单了，反正Core和DMA都有Load/Store，增加跨片就好，不需要开发额外的网络传输模块，对Load/Store做一个Address Mapping和Packet Engine就好了。

所以呢

1. **Scale-out买张RDMA的网卡（ETH+RDMA改），云豹？或者就用客户自研的，火山？ 和对方公司相互站个台，双赢。**
2. **Scale-up基于ETH-X做ETH层打包Load/Store（ETH+LD/ST），估算一下得大约100人月来交付。**
3. **与业界的各家ETH Switch做做兼容性测试，当然关键的拥塞路由等等得看看客户说了算。**

---

- 什么，你说Load/Store缺乏重传机制，没法过光纤，无法组2K、8K的Scale-up大集群？ 哦，这就不是我Startup公司的菜，这种珠穆朗玛让其他人去攀登就好了，我们做到8卡、16卡，上探到像NVL32A那种，纯电互联，大约40~60KW的风冷/液冷兼顾边界的RACK规模才是主力的销售档位。

- 什么，你说ETH的包头负担太重，带宽利用率上不去。嗨，我先把系统兜起来，跑起来给客户才是关键，大公司比拼才纠结那5~10%的带宽利用率，小公司还不如好好打磨一下GPU Core，拿到的算力优化收益更大。

- 什么，你说ETH Switch延迟太大，嗯，也还好啦，国内有好几家ETH Switch都在朝着低延迟在优化，一些鸡鸣狗盗的玩法，至少是号称能够做到100~200ns级别了，如果到这个水准，和PCIe Switch相差也就不大了。

能白嫖就不想氪金。

![](../images/ddd269e366bfa4ac960c1311380cc7b6.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='196'%20height='194'></svg>)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
