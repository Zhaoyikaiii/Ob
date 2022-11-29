---
doc_type: hypothesis-highlights
url: 'https://zhuanlan.zhihu.com/p/63179839'
---

# 如果这篇文章说不清epoll的本质，那就过来掐死我吧！  （1）

## Metadata
- Author: [zhuanlan.zhihu.com]()
- Title: 如果这篇文章说不清epoll的本质，那就过来掐死我吧！  （1）
- Reference: https://zhuanlan.zhihu.com/p/63179839
- Category: #article

## Page Notes
## Highlights
- 计算机执行程序时，会有优先级的需求。比如，当计算机收到断电信号时（电容可以保存少许电量，供CPU运行很短的一小段时间），它应立即去保存数据，保存数据的程序具有较高的优先级。 — [Updated on 2022-11-28 15:39:07](https://hyp.is/vZIMBm7vEe2dM1Pvr-6B4Q/zhuanlan.zhihu.com/p/63179839) — Group: #Public

- 一般而言，由硬件产生的信号需要cpu立马做出回应（不然数据可能就丢失），所以它的优先级很高。cpu理应中断掉正在执行的程序，去做出响应；当cpu完成对硬件的响应后，再重新执行用户程序。中断的过程如下图，和函数调用差不多。只不过函数调用是事先定好位置，而中断的位置由“信号”决定。 — [Updated on 2022-11-28 15:39:22](https://hyp.is/xrwOlG7vEe2Xys88_FJyKw/zhuanlan.zhihu.com/p/63179839) — Group: #Public






