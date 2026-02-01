# 极与极（2）

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 140
> **评论**: 32
> **时间**: 1651655787
> **原文**: [https://zhuanlan.zhihu.com/p/502950434](https://zhuanlan.zhihu.com/p/502950434)

---

![](../images/8bfa7821605c32b2040a7079a92f1501.jpg)![](../images/8bfa7821605c32b2040a7079a92f1501.jpg)

经历和感悟，也是一位合格的架构师所必须经历的历程。因为很多时候所谓的架构及其演进，往往是伴随着某些过去的边界条件发生的变化而触发的。

这个计算机体系结构风起云涌的黄金时代，闲来有空确实值得记录一下，那些偏执狂，他们在某单一维度的两极，在那最极端的边界疯狂试探的行为。

通过这些对比参照，我们至少可以知道，某些技术当前的上下限在什么地方。到不是说要去复制，熊猫人部落的特征是平衡，当我们看过了易筋经、又看过了葵花宝典，在把JJ关起来或者割掉之间，我们也能找到更加合理的存在。

---

第二个故事谈一下chiplet interface。

主要是刚好有人私信问我chiplet的事，想到哪儿写到哪儿……比较随性。

但巧合的是，还是那两个变态。艹

**Apple和Nvidia。**

他们代表了chiplet interface当下技术的两极。

Chiplet技术是把soc die拆分成多个partition die，然后再基于silicon or RDL or substrate通过某种interface互联起来的技术。

<https://zhuanlan.zhihu.com/p/484050565>

如果不考虑切割的刀工，那么就interface而言，技术分为两个大方向：

## **串口 or 并口**

没错，就和曾经计算机体系结构在HDD、pcie、甚至当下DDR接口的技术演（撕）进（逼）的故事是一样的。

那么，chiplet interface到底应该选择serial还是parallel呢？

当然，串口和并口并没有泾渭分明的界限。你说，UCIE的16G算并口还是串口呢？ 一般来讲，带独立时钟信号，IO的模拟化率低于例如20%，也算并口吧。并串一家人，不必纠结。

---

我想用一个容易理解比喻来形容chiplet interface，因为要是平铺直叙的话，涉及的数字维度有点多不易理解。

所谓chiplet就是一个大型城市（soc）被很多河流拆分成多个区（partition），就像重庆、武汉、广州，在区之间我们需要修桥来达成互联，这个桥就是chiplet interface。我们在搭桥的时候，在选择的技术上就存在了选择，你不可能十全十美，那么基于城市交通的诉求，你需要给出几个最合算的选择。

我们先形容区（die内）的总线是一个64车道，车速100km/h的公路，那么在河边，我们会遇到一个问题，车道的宽窄要变化（信号的pitch变化了）。

**Pitch（信号之间的距离）的变化是chiplet interface设计核心关键。**

*ps：决定pitch的是信号本身wire的宽度（线宽）和wire之间空白间隔（线距）。*

形容一下，就是64车道的BUS走到河边的时候，发现bridge的车道变成16车道，那么为了保障不塞车（虎门大桥），那么车速就需要从100km/s提速到400km/h，车速本身代表了动力（驱动力），虽然车道变少了，但每个车道之间的距离反而变宽了，举例来讲64车道的间隔是2um，16车道的间隔是100um，那么最惨的四辆车要先跑700um的横向距离才能找到所属车道合并加速上桥。

展开了说，chiplet interface有三个核心技术问题需要处理。

1. **signal fanout，片内pitch小，片间pitch大。signal从片内转到片间signal时需要额外的横向展开。片内的pitch基于不同设计需求大致是0.1um～2um，片间的pitch基于不同封装技术在2um～150um之间。相差20～80倍，有兴趣的同学可以算算如果信号数量不变300 signal横向展开的走线距离。**
2. **signal speedup，为了改善fanout的代价，如果提升片间信号的速率，那么fanout就可以减少，否则你看150um距离，300signal直接展开那就是45mm边长，这样的芯片尺寸是不可实现的。所以必须提速降低边长开销，假设片上总线是2GHz，那业界就有存在了2GHz~100GHz的1~50倍提速，提速带来的是信号数量减少以及串并转换，串并转换、高速信号的调制解调及就错，就成为了speedup必然的代价。speed up呢，也有一个缺点，那就是越快的速率，大致上pj/bit功耗会越大。**
3. **signal synchronize，车同轨、书同文，表面上很简单，但开车你也免不了男司机超速、女司机龟速啊，所以表面上两个 die是等速率，实际上会因为各种情况，例如独立PLL出现时钟偏差，IC是容不得偏差的，所以除了fanout和speedup之外，你往往需要额外的同步逻辑来保障双方的速率对齐，不能丢车。**

---

如果足够细心的同学，对上面数字足够敏感，已经会发现一些corner case。

---

是的，第一个变态apple，他变态之极在于，他完全不想承受上诉的三个困难的任何一个。

![](../images/c076c2d9bbfbe79a62b92690ef0faf19.jpg)![](../images/c076c2d9bbfbe79a62b92690ef0faf19.jpg)

Apple先选择在片上总线使用~2um的signal pitch（也可能是1um左右），这个是最高层metal走线的pitch，这个选项wire latency可以做到最低。与其对应，0.1um to 2um的fanout会前移到CPU to BUS之间。然后 apple选择了最极端的并口策略，并到极致。也就是选择了2um的 chiplet signal pitch。也就是一句话：**Apple的chiplet interface和内部bus一样宽一样快。**嗯，没有fanout，也没有speedup，实际上两个同样的die，基于某些同步技术他连synchronize也去掉了。

嗯，了解部分信息的同学会问。Apple采用的info\_lsi的pitch不是25um吗？对，那是pad的pitch，但是Pad是可以放多列，Pad之间还可以走线的，Apple放了多列信号，最终在20mm边长放置了2GHz（功耗低）的～10000 signals，2um/signal，总带宽20Tbit即2.5TB带宽。

**没有fanout、没有speedup、也没有异步，apple把chiplet做到了和片内总线在物理上的一致性，功耗还特别低。**

**要说缺点，那就是面积开销比较大，还有就是扩展性很差，必须依赖tsmc的info\_lsi技术获得Pad的25um pitch再叠加多列的技术获得极致的2um signal pitch。**

---

另一个变态，是Nvidia，他选择的是极致的串口，100Gbps serdes，总共900GB带宽。

![](../images/7b40ab58ccf902ede1e91fe731fc86d3.jpg)![](../images/7b40ab58ccf902ede1e91fe731fc86d3.jpg)

曾经有人跟我说nvidia用的是12G的并口，我也无从查证，但是从nvidia的GTC官方稿件看，他在表述上更多地是表达了其serdes属性（按时钟是内嵌而不是随路来看），更倾向于是一个串口。

[https://nvidianews.nvidia.com/news/nvidia-opens-nvlink-for-custom-silicon-integration](http://link.zhihu.com/?target=https%3A//nvidianews.nvidia.com/news/nvidia-opens-nvlink-for-custom-silicon-integration)![](../images/02a8ea10444eb2ad710bd86d0e416e8c.jpg)![](../images/02a8ea10444eb2ad710bd86d0e416e8c.jpg)

而且，还有一个隐藏的逻辑，也就是io pitch越小，其驱动能力越有限，对，IO驱动力和pitch是负相关。如果nvidia的chiplet io能够支撑从WSE到COWOS到PCB，那它的IO驱动力至少要满足PCB级诉求，pitch小不了，不可能是40um及以下级别。所以，我更倾向nvidia采用的是**112G可变驱动（可变功耗）pitch 150um以上的IO，PAM4但短距可去掉FEC。**

所以nvidia大概率选择了另一个极端，把speedup、synchronize做到单一位置最大代价化，fanout因为高串并比也压缩到另一个极致的最低。他内部大概率会用普通走线，0.1um pitch &～2GHz频率，但在chiplet interface选用150um pitch & 100GHz的频率，最终是50:1的串并比和30:1的fanout比。

Nvidia只用了72 lane大约10mm边长获得了900GB带宽，嗯，如果用满20mm大约能获得1.8TB，哈哈，边密比稍逊并口，毕竟不能做多列，但问题不大，nvidia的业务相对节制。

**Nvidia的得失与Apple完全相反，他得到的是chiplet的兼容性和灵活性，支撑各种封装形态，因为serdes不能做多列，面积是更优不少的。**

**NV的缺点是啥呢？ 毫无疑问，巨大的fanout、串并、异步处理代价，他的延迟会大很多，这会影响不少业务性能，但NV本身做DSA嘛，业务范围可控。此外功耗也会大不少，我甚至猜NV不做更多带宽一方面是够用，一方面还是功耗太大。**

---

**综上，做个小结。**

![](../images/df35be042638f32095ebf27381604fb9.jpg)![](../images/df35be042638f32095ebf27381604fb9.jpg)

**apple的极致并口和NV的极致串口，给了我们一个从上天到入海的广阔空间，而AMD、Intel（EMIB或UCIE）则是在广阔的天地之间，再寻找了其自身最恰当的选择。**

还是想起有人问我对国内的chiplet startup公司怎么看，我对外面了解太少，唯一知道的可能是北极雄芯吧。如果看今天讲的 chiplet interface，他们看上去是采用了Nvidia的路子，100G serdes，这个路子咋说呢，在国内这不成熟的产业链来看，scaleable肯定是第一要素，选very short distance serdes至少能兼容MCM到COWOS，能够服务的客户多，但问题还是延迟和功耗，反而又决定了这个路子有限的适用范围，做做DSA肯定是可以的，但更多的就不好说了，祝好运吧。

---

嗯，你要问我怎么选？

还是应用最开始的歌词……

**我曾经跨过山和大海**

**也穿过人山人海**

**……**

**直到看见平凡才是唯一的答案**

**平凡，不是平庸，而是人间烟火气，最抚凡人心。**

![](../images/15c88193a5864c0d654352044f540445.jpg)![](../images/15c88193a5864c0d654352044f540445.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
