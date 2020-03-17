﻿# 关于接口

# 引言

　　本文讨论有关接口(interface) 及其设计的目的论话题。

# 概念

　　[接口](https://zh.wikipedia.org/zh-cn/接口)是交互(interaction) 实现的抽象手段。具体的接口可能从属于：

* 接口设备(interface device) ，一种具有连接功能的物理实体，如插座(socket) 。
* 可编程接口(programmable interface) ，一种以可组合描述的形式提供的可复用设施，如抽象基类(abstract base class) 。
* 用户界面(user interface) ，一种实现隔离的隐喻，如图标(icon) 。

　　注意中文翻译“接口”和“界面”的原文是一个词。分辨不同的说法可以减少使用完整说法的必要性，但刻意区分其不同，在需要了解 interface 的共性时，一定程度上是有害的，这在下文讨论。

# 目的

　　普遍接口的概念具有非常强的普适性，而不容易抽象地概括。但它们之间显然存在目的上的共同点。

　　广义的接口设计遵循一个一致的目的：适配(adapting) 。

　　接口的适配特性在物理上通过连接具有可分辨截然不同的物理特性的不同系统来体现，即接口和对应的适配器(adaptor) 。例如，插头是插座对应的适配器。实际上，就物理设备而言，适配器是外延更广的实体，而接口设备是适配器的一类。但因为设备制造等原因，接口往往更容易被标准化，而被单独强调。

　　由于标准化接口的可用性便利，类似的方法也被引入可复用方案的其它设计，特别是可编程接口的设计中。这是很自然的，因为最早期的一些可编程接口就是基于具体系统的物理特性归纳的规范约定，进一步地也是一种标准的物理接口设备（如穿孔纸带）。这并行地演化为不依赖物理设备的不同接口：强调可复用的可编程接口和强调可用性的用户接口。这个过程中，接口的存在意义（或者说原始目的）被保留了，但又演化出了不同的特性：不像物理设备，用户可以通过接口的可配置机制来“生产”新的接口组合方案，而产生了不同的用法。

# 规范

　　为了适配使用，接口必须要具有一定可组合性。为了可预期的可用性(availabilty) ，这些组合还应是保持兼容(compatible) 的。接口的这些核心特性通过规范(specification) 明确。在物理接口设备中，这些规范是自发明确的，否则相对容易地就能发现不能生产或使用；但非物理实体保证的接口，是否能适配则有更加复杂的情形，具体的兼容性往往随着具体目的没有在规范中足够明确而不能清楚地描述，是否符合规范也可能极其依赖具体测试条件的选取。这种动机复杂性给接口的统一带来了困难，而往往没有足够地被重视。

# 角色

　　因为演化的原因，接口的分类实际上为不同更具体的目的服务。这种目的体现在接口的用户角色(user role) 上。

　　物理接口设备是人机交互界面(HMI, human-machine interface) 的一个基本组成部分，但也是更一般的自动化的组件之一。它的用户是抽象的物理实体，且用户一般没有重新和接口协商而替换具体接口的能力。遇到兼容性问题，物理接口设备的用户往往倾向于替换所有关联的适配器。这实际上经常是一种不仅适用于物理接口设备的稳妥的做法，只是就物理接口设备的用户通常没有其它选择（因为自己不能生产设备）所以成为唯一的没有争议的做法。

　　与此不同，可编程接口服务的用户是能对系统进行操作，使用系统的可编程性(programmability) 解决问题的用户。这样的用户一般是人或者人维护的计算机程序。虽然可编程性本质因为物理实现的可配置性有限而可以明确边界，但其组合具有远超物理接口设备的数量上的复杂性，而其评价标准也更多样且受制于用户对需求和可行解的主观认识不同而有所差异，设计者原则上无法穷尽可用配置来避免这里潜在的设计缺陷问题。因此评价什么是应该响应的缺陷就显得更加重要。由于用户需求及其总结的接口设计规范一般仍然使用不总是无歧义的自然语言描述，从源头上因为理解偏差而引入缺陷的可能性，这一般是无法避免的；这因为可编程接口使用的非严格地其它非自然语言描述（如不具有形式语义的编程语言和图形）而放大了。

　　明确的可编程接口，如应用程序接口(API, application programming interface) 和应用程序二进制接口(ABI, application binary interface) 因为维护兼容性的复杂和专业性，要求用户具有一定程度的可编程能力，因此在事实上划分了开发者(developer) 和非开发者用户。但排除演化的问题，这里的复杂来自对接口设计方法论的普遍应对不足，因此这个划分不一定是必要的。[关于操作系统](about-operating-systems.md)的讨论中，已经指出许多不同形式的可编程接口来自设计者的局限性，也提到有作者认为区分（最终）用户和开发者是不必要的——因为用户最终都在进行广义上的可编程操作。这样的局限性显然也不是个别系统接口设计者的问题，而是更普遍的认知不足。这甚至根植于“理论计算机科学”的一些普遍认识之中——例如，大部分用户并不了解计算模型和体系结构模型之间的距离：

> 我要关心抽象机，我首先确保可表达性（关于实现可计算性的性质），其次关心其一阶导子（抽象能力），因为这是我能在设计上做文章的地方。

> 允许的表达中如何有效地刻画算法复杂度之类的计算实例的局部性质，只是这种系统的抽象能力服务的对象，是抽象机的“用户”的活，而不是我要做的东西。这样的具体应用，对设计计算模型时要关心的问题来讲，这是很 specific 而边缘化的性质。

> 所以作为计算模型的作者，我也没怎么看得起理论上“整算法的”——即便连算法分析都不会的更该被鄙视。

> 题外话，**我不认为倒腾 API 的开发者和倒腾 UI 的用户有多大界限，但是理解模型存在意义的用户和没天赋理解模型存在意义的用户之间的区别显然是更大的**。大部分折腾体系结构问题的搬砖的，属于后者中的后者；因为后者中的前者基本去倒腾数学灌水了。当然，我没说倒腾数学就一定更高级和有意义，不过这是另一回事。

> 关心所谓“速度”的实践是派生出来的东西罢了，因为“速度”的定义就是通过渐进性质度量来刻画的。物理学在这上面也是弟弟，因为物理学意义上的速度这些基本物理量也是模型上的简化，是一种实现。其实“时间”也一样——时间复杂度从来只依赖渐进步骤，而不需要是物理时间。认为不基于物理学定义的抽象就没意义显然是一种在这个鄙视链下的弟弟理解。

> 根本地，物理学概念为何有用？因为它可以实现。但这不表示不存在物理实现的模型就是没用的。因为模型本身可以通过抽象的变换修改到具有更良好可实现性质的变体而实现，这个过程并不总是保证向更接近物理（具体）的方向。没有这个天赋修改模型理解不了，所以不得不只能想象和倒腾足够具体（接近物理允许）的模型很正常，就是因为理解“意义”能力的低下而被鄙视罢了。

　　要求任意的用户都具有深刻的专业理解自然是现实不可能的；但用户至少应该做到，他们要么不关心他们以外的角色划分，要么就承认既有的划分是因为历史偶然和局限性的因素造成的，而固守角色划分不会直接带来价值，反而可能阻碍有意义的系统解决方案的进一步演化。这种演化的整体上的主要方向是**削减可编程性本身的专业性**，使原先只有少数开发者可以完成的可编程任务扩展到更多的用户可以轻易完成，也能进一步使更多用户把专业性的注意力集中在领域特定任务上，提升这些领域通过可编程的方案解决问题的可用性和效率。这在另一方面上也意味着，**“我不是开发者”在越来越多的情况下并不能作为不会使用可编程方案解决问题的借口**，只是时代的局限性导致这样的基准暂时仍然还不甚严格，使这种对解决问题无能的情况暂且被广泛容忍。尽管缓慢，这样的基准事实上一直在变动，例如近几十年文盲的定义就从“不会认字”升级到“不会使用计算机”了。这个意义下，**“开发者”的划分基准也不可能是绝对的**，没确定很具体的问题领域这个前提，就不存在精确的二分法划分“开发者”和“非开发者”。而考虑使用配置文件甚至使用配置图形界面都属于越来越常见的可编程操作，也印证了一般用户越来越不可能完全被排除出开发者之外的现实演化方向。

　　而用户界面则在用户角色划分上存在类似可编程接口的更落后的问题。可编程接口的用户因为历史上的专业性，通常可以比较容易理解一些可用性问题（如特定种类兼容性）的来源和整体解决方案（及其局限性），而用户界面服务的对象是传统的“非开发者”，通常并不能清晰地了解这些问题的背景。
但这些用户的认知偏差并不能支持这些问题的解决。更糟糕的是，其中的特定不同专业领域的用户就开发任务而言仍有显著认知差距距离，却似乎认为自定义了合理性，在用户界面的用户群体内部制造了用户阵营的割裂问题。具体症状最显著的是对[命令行界面](https://zh.wikipedia.org/zh-cn/%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%95%8C%E9%9D%A2)(CLI, command-line interface) 和[图形用户界面](https://zh.wikipedia.org/zh-cn/%E5%9B%BE%E5%BD%A2%E7%94%A8%E6%88%B7%E7%95%8C%E9%9D%A2)(GUI, graphical user interface) 的无谓的对立，这种对立显然无助于用户界面方案的演化。有几个很浅显的要点可以鉴别这些用户是否真的足够理解这里本应他们关心的宏观问题：

* 用户界面和可编程性有什么联系？
* 什么时候需要用户界面？
* 什么时候适合使用 CLI ，什么时候适合使用 GUI ？

　　这些问题的答案和用户角色的划分有直接的联系。总的来说，如果需要某种用户角色，那么就有某类接口存在的理由，仅此而已。那些认为凡是界面就是指图形用户界面的用户，显然并没有理解“界面”（作为接口）存在的意义，很可能也完全不理解自身作为系统的涉众(skateholder) ，在一类普遍解决方案中的位置。

　　造成这种认知偏差的一个可能的直接来源是某些 GUI 因为经典的少数有效设计及刻意加强了入门用户引导的机制的原因，其初始学习经验可能带来“无师自通”而“低门槛”的错觉——以致于学习成功的用户忽略了熟练使用 GUI 仍需学习的基本事实；而经验表明，这种对学习投入资源的“低门槛”基本上仅存在于少数成功的足够通用的经典辅助抽象（特别是 [WIMP 隐喻](https://zh.wikipedia.org/zh-cn/WIMP_%28%E9%9B%BB%E8%85%A6%29)），这并不保证用户能同等容易地接受这些经典要素以外的 GUI 元素。后者的这种情形可能有很强的客观理由：例如，用户仍然很可能需要另外花费一些精力形成移动设备上使用 GUI 的习惯，而不能直接复用传统桌面 GUI 的经验，因为设备物理尺寸的限制是不被习惯约束而转移的，只能被迫适应。另一个常见的问题是用户接受的差异性导致的幸存者偏差——要知道还有一些用户甚至对所有经典或非经典的要素的可用性提示都不敏感而不会成功地自主学习使用。同时，在 GUI 开发（包括交互式验证）上复杂性和各种 GUI 实现方案的低可用性，也明显揭示了这里存在更多没有被克服的可编程模型上的显然困难——即便是专业开发者投入大量精力都不能很好地解决这里的问题，特别是和可扩展性有关的时候。后者也反过来使一些用户认为，GUI 在表达能力和适应性上总是（相较于 CLI ）更弱的，这也是一个技术上显然不正确的偏见。

　　作为专业领域，用户界面设计涉及很多 HMI 设计中没有很好解决的应对需求变动和一致复用的理论问题。因为现有用户习惯和预期不明确等限制，这些问题一般接口设计解决起来更加困难。通过合并可编程接口和用户界面服务的用户角色，在一定程度上可能成为新的解决此类问题的方向，但可以预料即便可行也需要非常长的时间才能自然地演化到现实可用的程度。不过，不可能解决短期问题也并不妨碍值得一试。

# 原则

　　作为接口设计动机复杂性的应对，评价接口完善程度的基准，应不仅仅限于服务于用户的具体的特设的(ad-hoc) 的原始可用性指标，还应该符合被接口设计者普遍接纳的附加规范。用户不需要理解这些规范的意图，但是应当注意到，原则上不遵守附加规范的接口设计方式，为接口可能的演化带来了困难，损害既有接口设计适应用户需求变化的能力，因此应当摒除。

　　这类评价接口完善程度的附加原则不依赖具体用户需求描述作为输入，由不同的问题领域总结，如：

* 单一职责原则：做好一件事。
* 关注点分离原则：强调重点。
* ……
