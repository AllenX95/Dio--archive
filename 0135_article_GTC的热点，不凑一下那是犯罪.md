# GTC的热点，不凑一下那是犯罪

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 271
> **评论**: 40
> **时间**: 1648378988
> **原文**: [https://zhuanlan.zhihu.com/p/487389526](https://zhuanlan.zhihu.com/p/487389526)

---

其实这个热点已经热了好几天，网上各种惊诧、机翻、演绎的文章也是满天飞了。

但所谓人类的本质……

![](../images/e18996bbf7ee1a5e722424e1d95e47ec.jpg)![](../images/e18996bbf7ee1a5e722424e1d95e47ec.jpg)

如果只是简单把GTC发布的内容翻译成中文，或者断章起义引起噱头，那这个热点我也没必要再来复读一下……

而我，人生最引以为傲的两大能力，除了逻辑记忆力，还有就是写轮眼了。

---

露在水面上的，不过是冰山的一角。

还有诸多老黄或明或暗展示了，但是并未直接表达的细节和秘密，都还潜藏在水面之下……

![](../images/64c459a01953edd16f6730c272ed18af.jpg)![](../images/64c459a01953edd16f6730c272ed18af.jpg)

*我凑热点，只讲讲水面下的一些内容，水面上的，包括GTC的演讲和中文翻译网上有很多copy可供参考，不再累述。*

---

## 1、NV逐渐在DSA化甚至支持spatial计算

做AI DSA的同学是第一时间感到诧异的，怎么NV做做得越来越像自己？ 例如昇腾的同学一看，怎么tensor尺寸又向CUBE接近了? TMA什么鬼d(ŐдŐ๑)，不就是达芬奇架构的MTE吗？

引用一下某大神的帖子……

[如何评价英伟达 3 月 22 日发布的全新 GPU H100 ？](https://www.zhihu.com/answer/2406955411)

因为他也是做DSA的，所以表达的相对中性偏乐观。我向来比他偏激，NV这一次要么是在吸毒，要么是要生态革命。

架构是什么? 真正的架构，并不是简简单单做出一颗好芯片，而是一个可以长期演进的生命力，或者说逻辑上存在的演化路径。

![](../images/1896b2c75630b33666be4c771dc159cf.jpg)![](../images/1896b2c75630b33666be4c771dc159cf.jpg)

好的架构具有的是旺盛的生命力，例如X86、ARM，他们存活和生长了几十年，也赚了几十年的钱。这才是架构。

像坊间某些DSA架构（CIM你不要躲），即使能够成功，很可能也是昙花一现。

回到NV，如果说最初引入tensorcore是兴奋剂，那引入TMA这些特征差不多有点吸毒的意思了。

最简单的问题，TMA如何提供给用户? 如果直接在CUDA中以指令方式编程，那好，你如何保证今天TMA的代码在十年后NV的GPU上运行，并且行为一致? X86的MMX指令的故事还历历在目。

**架构也是一种承诺，那很可能是一辈子。**

当然，也有一种可能，就是NV的DSA不再支持CUDA，反正DSA嘛，只提供框架下的库调用，然后架构跟着框架走，大不了未来GPU再分裂一个AI专线也不是不行……

最后说一点，H100支持了SM-2-SM的数据流，并且支持了大量的异步编程机制，例如async barrier等，这其实代表NV在DSA上逐渐支持了类似graphcore、tenstorrent的spatial computing，虽然说SM-2-SM约束在一个GRP之内，但是小尺度的sub-graph是有可能可以直接launch了。

*PS: 有人说这个机制只是做算子融合，我认为这有点难，手写可以（那TMA就暴露给CUDA了），要编译器识别各个L1地址，强人所难了。*

最后说一句，强如NV，也没有任何自大，依旧在虚心学习，博览众家所长为我所用，而坊间动不动就颠覆NV的同学们，加油啊！

---

## 2、Grace大概率是ARM N3

grace是一颗ARM公版core芯片，采用的是什么版本的core?

基本上坊间可见的答案都是 N2

不是！

首先分数对不上，倚天710采用的N2是128core下440分，你NV这升级到144core的740分，核不变，直接增加70%，让人家PTG的同学咋混? 大家都是硅农也不必搞能力歧视，一个脑袋两条腿，能做出来的都不是弱者。

![](../images/6352130ee9a04e69e6aaf79dbe1abf55.jpg)![](../images/6352130ee9a04e69e6aaf79dbe1abf55.jpg)

但更重要的理由是！

他是黄师傅啊！

我，老黄，站在行业顶峰，高处不胜寒的男人。

你让我接盘一个去年PTG就玩过的二手core，而且还要明年才能生下来……

没有比这更辱黄的了，信不信用显卡核平了你。

![](../images/ac107e42139e28e4cf31f923680943b9.jpg)![](../images/ac107e42139e28e4cf31f923680943b9.jpg)

**GRACE的CPU core，大概率是N3！**

对，很多对ARM路标很熟悉的人会跳出来，时间不对，N3要明年才发布！

敢问你和ARM是什么关系? 人家NV和ARM虽然最后棒打鸳鸯没领到证，可人家差不多算同居了一年半了，那可描述和不可描述的事情都发生得差不多了。

---

## 3、有三个NVLINK，4和Network和C2C

很多自媒体在high的时候，其实就没搞明白对象！

当然老黄在演讲中也有点故弄玄虚。

但实际上，这一代nvlink有三个！分别是**nvlink4（你也可以说这是总的generation代号）、nvlink network、nvlink c2c**。

nvlink4，或者你也能叫他nvlink native，就是上一代nvlink的升级，支持了DXG框内8颗GPU的互联，只是link升级从4x50G升级到2x100G。

nvlink network是256 super pod范畴内的东西，下图中，最下一层细线互联就是DGX内的nvlink4，而我圈红的粗线部分，实际上是nvlink network（当然你把他归属为nvlink4也行），但他确实和框内互联是具有巨大差异的。框内互联是基于物理地址访问，如果出错了一筐重启也没事。但到pod层面，整个策略完全不一样，他是基于类似于IP地址的路由了，而且DGX的任何错误也不能被扩散到POD，这是一个蛮重大的升级呢。不负责任的猜测是NV收购了MLX，把MLX在IB上的部分know how迁移到了nvlink的产物。

![](../images/551c0c97bcd9a34f95e066ad6002cd9a.jpg)![](../images/551c0c97bcd9a34f95e066ad6002cd9a.jpg)

但最值得一谈的是nvlink c2c了，从某种意义上讲，他其实只是名字叫nvlink，当完完全全是另一个东西。

nvlink c2c就是NVIDIA的UCIE，NV宣称它是**an ultra-fast chip-to-chip and die-to-die interconnect。**

他实质上物理层是类似chiplet MCM级别的接口，而协议层是AMBA CHI（NV你和ARM同居还干了啥）！

嗯，有人跟我说这还是nvlink serdes吧！

不是啊，你看这个接口的描述。

![](../images/9ca09e2f6c68173a867cf4b1ba4161d4.jpg)![](../images/9ca09e2f6c68173a867cf4b1ba4161d4.jpg)

多的不说，相比PCIE PHY 25x能效提升，那么PCIE PHY能耗是多少? 大约10pj/bit，25倍，那**nvlink c2c的能效是0.4pj/bit。**这个数字是什么概念，可以对照一下UCIE spec的pj/bit参照值，比cowos差但略好于MCM。面效有兴趣的同学也可以一算。

![](../images/cf1df99754e6aebf4d3f43621a819f1e.jpg)![](../images/cf1df99754e6aebf4d3f43621a819f1e.jpg)

好了，现在你觉得他是什么接口?

## 4、续3，有两颗H100，传统和superchip

很多人其实没注意Nvidia发布的的产品分档。

为什么GTC刚开始就讲了H100，为啥后面superchip又讲一遍?

因为这里其实有两个H100产品（完整说是三个）。

一个是我们传统意义理解的H100，它的形态有是PCIE或SXM两种，通过18组nvlink4的900GB带宽与nvswitch互联。

![](../images/21b32d8c430f4e26dd2f7f42bc0078a8.jpg)![](../images/21b32d8c430f4e26dd2f7f42bc0078a8.jpg)

另一个是superchip的H100，它通过900GB的nvlink c2c与grace互联。

![](../images/53b9e89621ad9b23278fe0a90eb4f5fa.jpg)![](../images/53b9e89621ad9b23278fe0a90eb4f5fa.jpg)

这里有两种可能：

一种是H100有两个版本，分别出了900GB的nvlink4或nvlink c2c。

一种是H100同时存在900GB的nvlink4和900GB的nvlink c2c，即总共1800GB的对外带宽。

你肯定会问，如果是后者，那不是既可以接nvswitch，也可以接grace啊?

(๑•́ωก̀๑) 美如画

但换作是你，要是手握一把好牌，我为什么要一下子全打下去? 我今天写NV也不会把我知道的所有都讲出来啊。

![](../images/5d193986a0b4315de1074d98158d5a99.jpg)![](../images/5d193986a0b4315de1074d98158d5a99.jpg)

---

## 5、续3，做UMA的变态又多了一个

NV的grace提供了superchip形态，两颗芯片采用nvlink c2c获得900GB互联带宽（一种猜测是类似MCM的基板或高级PCB做介质）。

![](../images/5c85e697a8c55eac0f17b28042707446.jpg)![](../images/5c85e697a8c55eac0f17b28042707446.jpg)

这是一个2S server形态吗？ 在老黄的描述里，他不是的。

900GB的带宽远超了传统2S server的诉求，正常来讲，做NUMA，两颗芯片之间带宽，参考INTEL吧，大致是10G x 20LANEs x 2dir x 2port = 100GB。

只有做UMA的大变态才会把带宽做到DDR带宽同级。

所以Grace这颗superchip是UMA，用户在逻辑上看到的是完整144个CPU（72/die）和单一的1TB（500GB/die）内存带宽。

另一个变态是apple M1 ultra。

*ps：grace没有GPU所以带宽没有apple 2.5TB那么高。*

<https://zhuanlan.zhihu.com/p/482450390>

这世道，变态还都成双成对了。

在传统体系结构的cache coherency上，因为跨socket的带宽延迟受限，所以往往不得不引入多一级cache hierarchy来解决CC-NUMA，这个技术蛮难的，intel的UPI/QPI就是，技术上也是禁止出口中国的，菊厂为此自研了HCCS的CC-NUMA协议，真蛮难的，我曾经形容需要找到七颗龙珠才能做好。

没想到啊，没想到啊。有些个变态，他不找龙珠，他直接屠龙啊！ 不讲武德！

![](../images/356b50402e63a0a498df2bab23e0cb4a.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='400'%20height='298'></svg>)

## 

---

## 6、续3，NV对支持UCIe是偏负面的

这帖子让我挺无语的。

[英伟达宣布支持UCIe，推出144核CPU和800亿晶体管GPU](http://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/Ha4CKQopZ9KJ0-Ko-ZcaBA)

我在youtube完整地听完了GTC全篇发言，我咋就没听到老黄说他宣布支持UCIE呢？

我下来查了一下信息源

在NCLINK-C2C的官方稿件中有下面这段话

[https://nvidianews.nvidia.com/news/nvidia-opens-nvlink-for-custom-silicon-integration](http://link.zhihu.com/?target=https%3A//nvidianews.nvidia.com/news/nvidia-opens-nvlink-for-custom-silicon-integration)

![](../images/010e99ed42874cb2183985af2123dc8b.jpg)![](../images/010e99ed42874cb2183985af2123dc8b.jpg)

在商言商，这是一句相当商业化的说法。

***“我们推荐利用nvlink c2c，但如果你一定要Ucie，（只要你砸钱够），我可支持，但还是提醒你nvlink c2c比UCIE好那么、那么、那么多………………”***

还有一个源头是GTC之后的记者会的答记者问，有兴趣的人可以去查查原文。

***Huang：我是UCIe和PCIe的信徒（不是CXL的），如果UCIe成为标准，我会兼容它来对接broadcom、TI、marvel的chiplet（就是没有intel），这一天会到来，UCIe大概还需要五年时间才能成熟（nv-c2c已经成熟了）。***

括号里面的字是我补充的，老黄怎么说也是儒家文化出身，东方文明在表达内心通常相对委婉、点到而止的，即使要骂人，那有一个脏字也就是输了。

![](../images/546d81d53e2b192c259c4a9a49e8cd0c.jpg)![](../images/546d81d53e2b192c259c4a9a49e8cd0c.jpg)

## 

---

## 7、tensorcore的尺寸，x4

这是一个很趣的点。

tensorcore演进了三代了。

从最初的volta的4x4x4，到ampere的4x8x8，到hopper的4x8x16。

![](../images/b9738f9fd8f7eb50d7337d8d9e066847.jpg)![](../images/b9738f9fd8f7eb50d7337d8d9e066847.jpg)

他始终保留了一个纬度是4。

这事很早我想过，针对某些小尺寸的NN网络，如果卷积的kernel小，或者channel数量少，在转换为矩阵乘的时候，就会有一个纬度的利用率会上不去，因此，保留一个纬度为4，做不对称尺寸的tensorcore对MAC利用率是有好处的。

这个事情我没法确认，因为NN的算法发展太快，现在的情况和我几年前的理解可能早就千差万别了。

## 8、疑是虚假宣传的DDR ECC

grace CPU采用了LPDDR。

另一个在PC/server级别CPU用LPDDR的是apple。

变态的品位都是一样的……

但大家都知道LPDDR没有ECC，而且我们可以数一数老黄展示的效果图上LPDDR的数量，8颗，没有额外的颗粒，并不能做到我们传统的64+8 ECC。

![](../images/53b9e89621ad9b23278fe0a90eb4f5fa.jpg)![](../images/53b9e89621ad9b23278fe0a90eb4f5fa.jpg)

我不能确信这里老黄的ECC指什么，如果打开LPDDR5协议来看，确确实实有新增link ecc特性，但是这个特性的目的，是在LPDDR的高速传输链路上保证数据不出错，并不能保证数据存放在颗粒内部后的错误。

此外还有一种inline ECC的技术，直接复用已有的带宽和容量做扩展，如果是这个，那老黄宣传的带宽和容量就是假货了，而带宽和容量又恰好很关键。

所以，我不负责任猜测老黄是虚假宣传，大概率用link ecc来隐藏其LPDDR缺乏ECC保护的真实。

## 9、世界第一的交换机

被核弹压制的，不仅仅是敌人，还有自己人！

![](../images/3ed3bcebffc3990e426064770a1008fe.jpg)![](../images/3ed3bcebffc3990e426064770a1008fe.jpg)

你如果不是做交换机芯片的，这张仅仅画在一个给GPU用的交换机产品的右上角的一小张图，打个哈欠可能就过了。

做交换芯片的悲哀……

51.2T交换带宽，这是世界第一的水准！

如果按我个人的理解，现有工艺下，单芯片是做不出来的。需要用chiplet了。

如果看图上数据，和H100一样的N4工艺，H100是800亿晶体管，而他是1000亿晶体管！ 整体规格比H100还巨大25%！ 搞得不好这颗芯片是当今世界晶体管数量最多的单芯片！---下来查了一下最大规模单芯片是另一个变态，M1 ultra。

当然图上放大了看确实也像是多个DIE的组合，只是不知道这些组合有没有使用nvlink c2c了。

这颗芯片，如果它在任意一家通信芯片公司，例如broadcom，会成为众星捧月的焦点。但在NVIDIA，我有新核弹才是关键，这个就是常规武器正常升级，基本操作，勿念勿念……

但还是要向这个了不起的团队致敬一下。

---

## 10、定制供电模块路径的失败

具有美术素养的同学……

看这一次GTC有没有觉得有什么画风变化了?

…………

我贴出上一代ampere的图供复习一下，对照前面

![](../images/529f99a4096fecdb5be095619ee3d4ee.jpg)![](../images/529f99a4096fecdb5be095619ee3d4ee.jpg)

噢，这令人迷恋的土豪金色，不见了！

老黄你H100是不打算卖给中东客户了吗？

很多人可能不知道ampere这个金色的长方体是什么，这是美国一家公司vicor给nvidia定制的电源转换模块，相比传统的供电方案，他能用48V直接转1V，还能保持相当出色的转换效率。

我还不是很清楚发生了什么，但是股价从不撒谎。

![](../images/9d840d67baeb61a6ec7d99fec8a4d6f6.jpg)![](../images/9d840d67baeb61a6ec7d99fec8a4d6f6.jpg)

大概率，还是定制方案最终败给了白盒方案！

就是不知道这两年那某些看着ampere激动，也定制vicor类似电源模块的同学接下来咋办(・o・)

---

## 11、恐惧，才是最让人担忧的

这次GTC最大的伤害，其实是对挑战者们的信心。

2000T FP16，即使不算稀疏，那也是1000T，除以700W，这已经到了令人绝望的1.43Tflops/W了。

DSA赖以超车的定制，Nvidia快速地复制了。

一直以来坊间传闻hopper会采用chiplet来提升性能，最终它没有。这并不是nvidia没有chiplet的能力，他可能只是单纯觉得单手操作就能赢……

我能理解……

在GTC之后，今年的AI芯片公司会有很多新说法吧。

“正面干不过没关系，我们可以出奇制胜或者弯道超车……”

“以后我们不要比拼单纯的数字指标了，我们另外定一个benchmark……”

这就像面对100KG的对手，70KG的你，已经放弃锻炼自己的体魄，而沉迷于“猴子偷桃”。

也许一次能行，长期呢……

IC之所以被成为硬科技，那是因为一招一式，直拳、勾拳，取不得巧。

无论对手有多么强大，我们绝不可放弃内心的追求。

王侯将相宁有种乎！

**在相同的条件和目标下，他们能做到的，我们也要能！ 来吧，大战三百回。**

![](../images/0d2d9f8c942cbe96df0b50633dc2b192.jpg)![](../images/0d2d9f8c942cbe96df0b50633dc2b192.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
