# mesi协议的约束范围？

> **类型**: 回答
> **作者**: Dio-晶
> **赞同**: 0
> **评论**: 0
> **时间**: 1494029435
> **原文**: [https://www.zhihu.com/question/53138654/answer/165407478](https://www.zhihu.com/question/53138654/answer/165407478)

---

如某些答案，这个RMW的行为并不是cache coherency的范畴，而是memory consistency的范畴。体系结构的概念一定要清晰。

在CC范畴步骤5的行为就会错误。

真实情况可以参考LINUX的的共享变量处理，如果CPU支持atomic指令（包括ARM的exclusive指令），OS会调用atomic指令完成RMW，这类指令可以保证RMW的过程中CACHE Coherency纬度状态不变。

如果CPU不支持atomic指令，则会采用spinlock机制，即额外用锁锁定访问权限，让RMW操作对象为独享方式完成。

---

*由知乎爬虫生成于 2026-02-01 15:39:00*
