# zen2 CPU的IO 芯片上有没有L4缓存 ？

> **类型**: 回答
> **作者**: Dio-晶
> **赞同**: 0
> **评论**: 8
> **时间**: 1580702941
> **原文**: [https://www.zhihu.com/question/369262185/answer/996281247](https://www.zhihu.com/question/369262185/answer/996281247)

---

从目前的信息看，没有。

首先是各种latency测试没有看到这一个分级，cache的主要收益首先在于latency。

IO DIE中部看到的明显规则化的block在desktop的IODIE上也存在。说明这两个block如业界猜测，是IO hub，并无特殊。

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
