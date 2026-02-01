# 数据中心的暗战

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 357
> **评论**: 26
> **时间**: 1607179444
> **原文**: [https://zhuanlan.zhihu.com/p/331804131](https://zhuanlan.zhihu.com/p/331804131)

---

![](../images/55358f0f7734a648ade3051d9cd68a8d.jpg)![](../images/55358f0f7734a648ade3051d9cd68a8d.jpg)

蓝色的Intel是一家CPU公司。（他们和IBM撞色了）

绿色的Nvidia是一家GPU公司（没想通一个华人老板为啥喜欢绿色……）。

表面上，这是两家在不同赛道的公司，从绝大多数人来看，nvidia和intel联合霸占了整个IT行业绝大多数的利益，也代表了美帝对我兔的科技霸权。谁家没有买个intel CPU+nvidia GPU的PC呢，火热的云及数据中心也是以他们俩为主体。（说AMD yes毕竟还是非主流，咋没人说intel yes呢？）

![](../images/8d054bc140bda4f151ad72177d174628.jpg)![](../images/8d054bc140bda4f151ad72177d174628.jpg)

诚然，他们各自占据了一个技术高地，但这并不代表他们就愿意携起手相亲相爱。先不说intel卖CPU送GPU并且向重回独显GPU市场，nvidia想在AI领域摁死intel这些明线，这些都是肉眼可见的冲突，我还是更喜欢讲一讲台下的一些阴谋诡计的故事，这些事情往往没有被人注意，但更有趣味。

嗯，以下的内容都是我瞎扯的，不负任何责任……编呼嘛，编出你的故事。

![](../images/5cc415a90a1ed32187eed8bbaec05fab.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='146'%20height='176'></svg>)

**计谋1：**

intel一直在宣传上暗示一个潜意识，那就是CPU为中心，包括多年来机箱上的intel inside这种广告词，其实都是在植入一种潜意识，就是IT基础设施的心脏是CPU。这种意识为CPU销售上的高溢价是很有帮助的。但为了维持保持这种中心化的形态，intel干了不少私底下的坏事。PCIE接口是intel发起的，这个接口规范组织PCI SIG基本上也是被intel把持，近五年来，因为自家挤牙膏能力不足，为了避免有device甚至DSA喧宾夺主，intel故意阻碍了PCIE接口的演进，明明提频到16G以及32G都是早就能做的了，它就硬是要拖着大家只能在PCIE3的8Gbps不增，PCIE4也是同理。这事其实很多人都注意到了，并且也在反复逼迫intel提速。

**计谋2：**

nvidia其实一直都在反抗CPU为中心的思路，他想搞以CUDA为中心，bypass CPU，毕竟谁愿意做配角呢？cuda的存在从第一天起对CPU就是不友好的，因为CUDA在GPU内部有私有内存，并且通过CudaHostAlloc要求CPU强行pin住部分host DDR给他用，一个大颗粒的计算任务，在GPU内部独立完成CPU原本承载的任务调度和内存分配工作，在CPU隔壁建立起一个完整的独立王国（这和opencl的CPU为中心具有本质差异）。

intel不好好搞PCIE提速，对发挥GPU的性能影响是很严重的，所以NVIDIA不得不搞了自己的nvlink来互联GPU之间的交换，并且快速把速率提升到25G（可以看出PCIE的延缓是多么故意）。不过呢，NVLINK需要额外的单板支持才能互联，大多数单板只有PCIE槽位，很难大范围推广。所以NVIDIA只能持续挖潜，大搞PCIE上的GPU direct技术，这技术是干嘛的呢? 就是利用PCIE peer2peer直通，bypass CPU，这里面包含了SSD的 direct，还有和Mellanox合作的RDMA DIRECT。

**计谋3：**

intel也是狠角色，在skylake开始，PCIE peer2peer的性能只有2GB（满带宽32GB）。intel说这是他们设计上的一个bug（可能是印度外包背锅），虽然是bug，长期驻留erreta，但作为bug就一直没被修正。

**题外话**：鲲鹏920的PCIE peer2peer性能是28.5GB

**计谋4：**

这个bug让nvidia欲哭无泪，要direct就得再在单板上加装一颗Pcie switch，得多不少钱……迫于intel淫威，nvidia直接购买了网卡公司Mellanox，可以预见的未来，所谓DPU DOCA一定会增加某种接口或者手段，保证GPU和网卡之间的高效直通，继续向bypass CPU的道路前进。

更新一下，刚写完就看到doca这份宣言，我靠，AWS这么说也就罢了，人家是客户。NV你这么说，我要是intel我绝对不能忍好吧。

![](../images/a8c1704d1737e203c1f78e8d87422279.jpg)![](../images/a8c1704d1737e203c1f78e8d87422279.jpg)![](../images/0cfe1aae66ab05b0bc66f932317fa7ba.jpg)![](../images/0cfe1aae66ab05b0bc66f932317fa7ba.jpg)

**计谋5：**

但intel对nvidia的若干反抗其实早有伏笔。它推出了CXL，这个协议真的不简单，原本我们很多人都被他人畜无害的样子欺骗，原本以为它只是intel应对CCIX的临时不完备方案。但是认真打开看，其设计者是真正的聪明人，这个协议的特征1：简单，现有PCIE 设备商和switch商都可以在现有设计基础上做有限的，不太难的改动即可。特征2：bias一致性设计，特点是完全以host cpu为中心，所有设备的数据交互都必须通过host memory，连基于switch的直通的可能性都直接被关闭了。

CXL是一个非常典型用小支点翘起大杠杆的杰作，甚至喊出了不支持CXL就会被生殖隔离的说法，真是一把尖刀直捅nvidia。

Nvidia该咋办? 其实我也没想到NV的下一步。咋办呢？ 一个3000亿美元市值的公司被一个2000亿美元的公司捅刀子啊，不还手怎么忍? 不拆台能叫自由市场么？狗咬狗真够喜庆的。

![](../images/0fc5080d1f1354fbafd065927a370523.gif)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='402'%20height='402'></svg>)

-------------------------

**平行世界1：**

可能有人认为我故意夸大了冲突。并不是呢，intel和nvidia这种互相拆台对我们这些追赶者是大好事一件。如果他们能团结协作能怎样呢？ 一个参考系是summit超算，nvidia和IBM做了一次非常厉害的合作。nvlink被做进了power9 CPU，在一个2+6的系统中，8颗芯片做到了global address以及近似全一致性，GPU虽然不能cache CPU memory但CPU可以cache GPU memeoy，GPU可以直接调用CPU的linux malloc的VA地址（不用pin住），CPU可以调用GPU的cudaHostAlloc和cudaMalloc的VA地址，说白了一句话，一个kernel，可以任意选择在CPU或GPU上执行，内存可以任意选择CPU或GPU内存。

这事真的牛逼大了。IBM为了支持nvlink做了一个很复杂的NPU模块管理nvlink与内部SMP的差异，工作量蛮大的。

不过这事也是一代而亡了。power10不再支持nvlink。原因嘛，我理解其实很简单，power9辛辛苦苦把nvlink做好，但超算summit一算账，IBM卖CPU大概是5000$一颗，nvidia卖GPU大概是10000$一颗，每台机器是2颗CPU+6颗GPU，大致是卖GPU送CPU的程度，你说IBM图了个啥? 图了个寂寞！

![](../images/f62d1d32da817af3bd1d217e7be967f1.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='393'%20height='413'></svg>)

所以，intel永远不会和nvidia合作做出CPU+GPU在如上这种用户收益最大的高效模式。

**题外话**：华为的鲲鹏+昇腾基本上是做到了和IBM+NVIDIA的程度，某些地方弱一些，某些地方还更强，毕竟一家人，互联方案也是我主要在负责，再说我也还没想到怎么埋bug不被领导打死的方法……

------------------------------

**平行世界2：**

说了这么久intel和nvidia，那么AMD在干嘛？ AMD既有CPU又有GPU，咋办? 自家先打? 肯定不行嘛。CPU还翻不了intel的盘，也只能听intel的啊。反正ZEN2/3的PCIE peer2peer能力也是有点bug（可能和intel共享背锅侠），不过ZEN2/3如果接自家的CDNA GPU，有可能bug会自动消失（还没实测，猜的）。

**题外话**：如果CPU厂商愿意认真做PCIE peer2peer，这世界上根本就不应该存在PLX这类PCIE SWITCH公司存在的价值，你想想，ZEN2/3的IODIE，有128lane PCIe，如果P2P性能没BUG，本身就是PCIE SWITCH了。当然鲲鹏920就是这个逻辑，我司根本不需要买PCIE switch。

至于AMD的GPU侧，打不过nvidia啊，rocm都只能伪装CUDA来工作，还有啥奇招么？ 嘿，还真有。AMD GPU正式推出一种IF over GPU over Pcie的硬件架构，鹅妹子嘤，神奇啊，AMD，你苏妈妈就这样把多块GPU给硬生生连上了啊，不用Pcie switch，也不操心CXL的事，也就差一个NIC DIRECT看怎么连上了。真是你妹啊。感觉硬件还能多卖一份钱。破费啊！

![](../images/88ff40a006f0169bc655ec57769b3d0b.jpg)![](../images/88ff40a006f0169bc655ec57769b3d0b.jpg)

AMD永远是神奇的公司。

![](../images/f77291db280328dbb4cdd703332e221f.jpg)![](../images/f77291db280328dbb4cdd703332e221f.jpg)

---

*由知乎爬虫生成于 2026-02-01 15:39:01*
