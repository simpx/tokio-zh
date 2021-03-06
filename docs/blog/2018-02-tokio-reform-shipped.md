# Tokio改革发货，通往0.2

大家好！

我很高兴地宣布，今天，[改革RFC]中提出的改变已经有了
被发布到[crates.io]作为`tokio` 0.1。

主要变化是：

* 添加*default*全局事件循环，无需设置和
  在绝大多数情况下管理自己的事件循环。

* 从Tokio中解除所有任务执行功能。

## 新的全局事件循环

到目前为止，创建事件循环是一个手动过程。即便如此
绝大多数Tokio用户会设置反应堆来做同样的事情，
每次都必须这样做。这部分是由于那里的事实
是在Tokio reactor的线程上运行代码之间的显着差异
或者从另一个线程（如线程池）。

Tokio改革的关键洞察力是Tokio
反应堆实际上不必是执行人。换句话说，在这些之前
改变，Tokio反应堆将为I / O资源**和**管理提供动力
执行用户提交的任务。

现在，Tokio提供了一个驱动I / O资源的反应器（如`TcpStream`和
`UdpSocket`）与任务执行器分开。这意味着很容易
从* any *线程创建Tokio支持的网络类型，使其易于创建
单线程或多线程Tokio支持的应用程序。

对于任务执行，Tokio提供[`current_thread`]执行程序，它
行为与内置的tokio-core执行器的行为类似。计划是
最终将这个执行者移到[`期货`]箱子里，但现在却是
由Tokio直接提供。

## 通往0.2的道路

Tokio改革的变化已经发布为0.1。依赖（[`tokio-io`]，
[`期货`]，[`mio`]等...）没有增加他们的版本。这个
允许'tokio`箱子在最小的生态系统中断的情况下被释放。

计划是让此版本中的更改在之前得到一些用法
承诺给他们。任何需要重大更改的修复都可以
在向所有其他板条箱发布的同时完成。目标是
这将在6-8周内发生。所以请试试今天发布的变化
提供反馈信息。

## 快速迭代

这仅仅是个开始。 Tokio有雄心勃勃的目标来提供额外的
功能，以获得建立异步的伟大“开箱即用”体验
Rust中的I / O应用程序。

为了尽可能快地达到这些目标而不会造成不必要的
生态系统中断，我们将采取一些步骤。

首先，类似于`期货`0.2版本，`tokio`箱子将是
转变为更多的立面。特征和类型将被分解为一个
子箱数量并由`tokio`重新出口。申请作者将是
能够直接依赖`tokio`，而图书馆作者将挑选
他们希望将特定的Tokio组件用作其库的一部分。

每个子箱都将清楚地表明其稳定性水平。显然，有一个
期货0.2即将发布的突破性变化，但在此之后，
基本构建模块将致力于保持稳定至少一年。更多
实验箱将保留在a处发布重大变更的权利
更快的步伐。

这意味着`tokio` crate本身将能够以更快的速度迭代
图书馆生态系统保持稳定的步伐。

前0.2期也将是一段实验期。额外
功能将以实验能力添加到Tokio。在0.2之前
发布，将发布一个RFC，涵盖我们想要的功能
包含在该版本中。

## 打开问题

剩下的一个问题是如何处理`tokio-proto`。它被发布了
最初的Tokio发布的一部分。从那以后，焦点已经转移了
箱子没有得到足够的重视。

我发布了一个问题，讨论如何处理该箱子
[这里]（https://github.com/tokio-rs/tokio/issues/118）

## 期待

请尝试今天发布的更改。再次，接下来的几个月是一个时期
在我们提交下一个版本之前进行实验。所以，现在是时候尝试了
出来并提供反馈。

在此期间，我们将整合这项工作，以建立更高层次
[塔]中的原语，由生产运营需求驱动
[Conduit]项目。

[reform RFC]: https://github.com/tokio-rs/tokio-rfcs/blob/master/text/0001-tokio-reform.md
[crates.io]: https://crates.io/crates/tokio
[`tokio-io`]: https://github.com/tokio-rs/tokio-io
[`futures`]: https://github.com/rust-lang-nursery/futures-rs
[`mio`]: https://github.com/carllerche/mio
[`futures` 0.2 release]: #
[Tower]: https://github.com/tower-rs/tower
[Conduit]: https://github.com/runconduit/conduit
