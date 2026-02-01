# 关于ARM和x86的能效

> **类型**: 文章
> **作者**: Dio-晶
> **赞同**: 40
> **评论**: 18
> **时间**: 1577714103
> **原文**: [https://zhuanlan.zhihu.com/p/100245649](https://zhuanlan.zhihu.com/p/100245649)

---

曾经回答过一个问题。

[夏晶晶：请问 X86 与 ARM 的功耗控制有什么区别？](https://www.zhihu.com/answer/886004257)

这事其实很多人是不信的，还有说我是AMD高端黑\_(:з」∠)\_

我换一个视角给一组数字供参考。

天底下各家foundry目前都是用12英寸的wafer(300mm)加工芯片，无论intel还是TSMC都是从日本购入单晶硅然后加工，单晶硅的费用大家都差不多，剩下就是光刻、侵蚀、切割、封装的费用。

下图是我桌上的wafer纪念品，一个MPW的，不是鲲鹏CPU的，仅供尺寸上做感官的参照。

![](../images/e7396bb465e89889e976105c71712e9f.jpg)![](../images/e7396bb465e89889e976105c71712e9f.jpg)

wafer的成本是一定的，虽然各家foundry最终出货到CPU制造商手中有差异（intel可能便宜），但从产业链健康发展的角度来看，各家最终wafer的合理价格是理应相当的。

以下数据暂不考虑良率，面积数据在公开网站图片均可测量，die num数据的计算在任意foundry都有提供计算公式可算得。

AMD的ZEN2是合封架构，TSMC 7nm工艺， CPU die的尺寸是10.3mm x 7.2mm，8核。每个wafer可以获得～800个die，即一个wafer可以获得6400个CPU。

intel skylake最大的XCC die，INTEL自家14nm+++++++工艺，die尺寸是32.3mm x 21.6mm，28核。每个wafer可以获得～75个die，即一个wafer可以获得2100个CPU。

菊花的鲲鹏920，也是合封的架构，TSMC 7nm工艺，CPU die尺寸是16mm x 9mm，32核。每个wafer可以获得～400个die，即每个wafer可以获得12800个CPU。

die面积越小，良率越高，但大家都有做partial good，所以影响并没有想象那么大，反而是在频率筛选intel良率更高（不展开）。所以计算基本上是可比较的。

intel自家建foundry，制造越多成本越摊薄，不造反而亏。但这其实也是一个伪命题。换一下视角这只是钱怎么计算的一种方式，钱都在这个产业链里面。

AMD的L3 cache很大，可能有人觉得不公平，但AMD把DDR PHY放到了IO DIE，少掉很多面积，而L3cache也是因为DDR延迟大不得已为之。所以也不算不公平。

总得来讲，从 core num per wafer来讲，数字就是差距这么大啊。

当然，ARM和AMD为了进一步提升性能，例如AVX宽度，未来肯定会再变大，但TSMC工艺也在演进啊(\*￣m￣)。

over

![](../images/a3fe4ab3296e06b6be1cbd2ee9c3d52a.jpg)![](data:image/svg+xml;utf8,<svg%20xmlns='http://www.w3.org/2000/svg'%20width='152'%20height='200'></svg>)

---

*由知乎爬虫生成于 2026-02-01 15:39:01*
