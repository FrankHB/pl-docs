﻿# 决策的哲学

　　许多解决方案设计的问题上原则是通用的。本文档提纲挈领，汇总一些相关话题、通用的准则以及例子备考。

## 决定性问题

　　一类基本的决策问题是[决定性问题](https://zh.wikipedia.org/zh-cn/%E6%B1%BA%E5%AE%9A%E6%80%A7%E5%95%8F%E9%A1%8C)。

　　作为需要解决的问题，首先一般需要确定是否是可解的。而判定是否可解是一个决定性问题。

　　决定性问题是可解的算法问题中是相对容易的一类。这里的难易程度指容易分类或者规约到其它一类问题以采用算法步骤解决，不表示为了解决实际是否容易投入资源实现算法。

　　一般的算法问题有时可以规约为一系列等效的决定性问题，即分解问题为判定两种选项之一的**原子问题**。但即便是最简单的问题形式，也不见得容易解决，因为原子问题的原子性是关于已知计算模型上的算法过程的，而在考虑现实可解性（例如是否可行）上可能蕴含非算法问题，其中包含主观的**价值判断**，而不仅是能通过算法处理的**事实判断**。

　　尽管对理性人，基于确定效用模型的价值判断仍然是算法过程，但现实中这些效用的输入以及对应的偏好往往跟随环境改变。因此，需要锚定一般的原则以减少影响。

　　为了使解尽可能有效和容易重现，应当：

* **避免路径依赖**：选择的策略依赖代价和意义，而不依赖路径。
	* 这包括依赖**当前**的代价和意义，以及**迟早**或**最终**遇到的未来的代价和意义，如果这些是存在（足够收敛能确切感知）的。
	* 例如，不应当认为因为你在幼儿园学习个别初等算术时就接触和熟悉了中缀语法，而认为中缀语法比你之后了解的其它记法普遍更自然。
		* 尽管幼儿园学习初等算术比系统掌握更一般地理论看似更容易（因为知识的总量更少更简单），但只要学习不终止于幼儿园，**迟早**要面对更一般的方法。

## 非对称性

　　考虑在决策问题中，选项 A 和 B 中选取一种的决定性问题的原子问题。

　　忽略偏好变化，实时的简单判定算法很简单：取对选项 A 和 B 效用偏好的序，若相同则随机。

　　若需要考虑偏好，则需要修正。一般的做法是：假想在可预见的未来（决策的影响会生效时），A 和 B 的长期效用的得失。

　　一部分很重要的效用来自（相对）客观的、不容易受到偏好变化影响的外部事实。因此，对事实的理解可以影响决策。一般地，这种影响对 A 和 B 不均等，而可对决策作出非平凡(non-trivial) 的影响。这种影响被认为是“有意义”的而不可被随意忽略。

　　更一般地，至少在统计上，承认以下前提是自然的：

* 承认“有足够意义”的客观世界普遍是*不对称的(asymmetric)* ，并由此使很多人为设计具有独立的不可替代的意义而产生价值。
	* 例如，*封装(encapsulation)* 的一般意义就是来自于一种接口可以和不确定数量的实现方式对应。
		* 接口及其实现的地位不能调换，可以调换的 1:1 设计是平凡的(trivial) ，欠缺封装的意义。

## 领域设计参照

　　关于现成的一个系统的例子参见 [NPL 领域设计原则](https://frankhb.github.io/YSLib-book/Features/NPL.zh-CN.html#%E9%A2%86%E5%9F%9F%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99) 。

　　当然，直接看大概容易会有这样的感觉：

![如何画马](https://imgsa.baidu.com/forum/pic/item/26a8b754564e9258f626578f9d82d158cdbf4ec8.jpg)。

（声明：图像版权不明，此处为合理使用。）

　　这没办法，因为本文档本来就是总结高频出现的观点以备复用，过程作为技术细节，需要被使用的需求并不常见，我就懒得写了。

　　不过有个愿景：作为比较简单的任务，这些步骤应是能被未来的 AGI 填充的。基于炼丹的那些就算了。

# 语言设计上的应用

　　在一些品味(flavor) 的问题上，难免时常难以找到足够客观的判断依据。但明知道不够客观，分开阐述一些逻辑前提，还是更有建设性的——至少比所谓“优雅”“学院派”的 buzzword 清晰地多，因为可以明确*目的*。而因为涉及主观价值判断，这里自然上升为哲学（伦理）问题。

　　这里隐含的出发点即非对称性[非对称性](#非对称性)和应当[避免路径依赖](#决定性问题)。

## 直接应用

　　对语言设计品味的评价的一些结论，可以由上面的前提经过少数几步推理过程得到。

* 例如，在默认蕴含 GC 的语言设计中让用户程序去除 GC 远比不要求 GC 的语言中通过用户程序引入 GC 的工程代价大得多。考虑到没有 GC 能够实现有 GC 时不容易实现的程序性质（如确定性资源管理、低延迟、低内存开销）。作为进一步的推论，一般的通用目的语言应当避免蕴含 GC ，以能适应最大化的情况。
* 又如，计算和逻辑不对等——存在偏可计算函数，不存在偏证明。
	* 计算系统和逻辑系统的目的就不是对称的。
	* 并且，前者可以作为后者的基础，反过来不行。
* 纯 FP 党根据 Curry-Howard 同构到处瞎找对偶(duality) 很大程度是徒劳的，因为不能认为一种结构具有目的，就一定能起到相同的作用。
	* 加入 Lambek 后的范畴论风格两两组队，具有同样的问题。范畴论语言很难在非结构主义数学家的圈外推广，一个方面就是理解上以及使用上的代价（比如罗嗦）跟传统方法都不是对称的。

## 递归和循环

　　作为经常被误解到值得单独一提的例子，递归和循环比较起来就不是对等的。

　　从可编程设施的接口的目的来看，迭代（循环）和尾递归具有同等难以理解的抽象程度和普遍性，初学者真要强行理解那只有具体实现了（例如具体的 `while`），有的派生（如各种 `for` ）语法上还更加麻烦。因为不可能就循环这个概念上强加个全称量词用户确保举一反三，实际上入门的用户根本没法教。真直接抽象的也就是“我要重复几次”之类的声明式描述，其实现对初学者而言还是黑魔法（而且基本也没办法准确实现，比如重复几次是或者不是并行的，副作用每次不同的又不确定相对顺序的，这些实现之间能共用么）。要准确领会抽象的迭代一般是很久之后的事了。反过来，递归在高级语言中基本上只有一种语法，基本不需要关键字。而且直接不需要什么多套娃几层抽象，直接教就是——这还不说尾递归就是加了个特殊条件。

　　有人认为递归比循环麻烦，以至于扣了“学院派”帽子就跑。支持这种观点的依据纯属无中生有。

　　如果是像 JavaScript 这样广泛使用的现有工业语言，为了维护现有代码的兼容性（包括业已鱼龙混杂的现有非专业用户的智商），外加当初的目标本来就不高（不要求那么通用），不去纠正现有设计上显然的技术错误也还情有可原。但抛开具体现有语言不管，这种调调就是扯蛋。

　　理解不了递归比循环自然，对一个实用的通用语言（而不是备胎或者大作业）的设计者来说，简直就像是理解不了 `2 > 1` 。

　　这里顶层功能点复杂度的计数是一目了然的事情：`card({递归}) = 1` 且 `card({递归, 循环}) = 2`；而只有循环没有递归的语言几乎就是低级语言（于是直接不配当通用目的语言），因为：

* 要重入的函数应用时还得 hack 掉活动记录(activation record) 之类本来可以不用出现在简单配置中的细节甚至干脆让用户自己实现一整坨。
	* 如 ISO C 半点没提 the stack 。
	* 搞笑的是因为一般体系结构上的 native stack 不靠谱所以其实已经不少靠谱的语言都自己造活动记录了。
* 而重入函数调用是现实不可避免的。
	* 光是过程式范式就到处都在用了。
	* 并且*在工程上*远远比普及手动造栈这种无聊易错增加实现和验证成本的人肉编译器工作好使得多。

　　注意，在所谓的循环控制结构不配取代递归调用的前提下，这里的 `2 > 1` 就是不对称的实例。所谓的循环用递归实现，而不是反过来，这是另一种自然的不对称。这种实现策略中，循环只是高一层的抽象的惯用法，递归调用是所谓的循环的实现。你说只学循环不学递归行不行？对外行或者 DSL 用户或许有些绕弯路的可行性（因为危害不会即时爆发，对非专业人士可能不影响实际的干活；这也是一个普遍能绥靖的原因），但对专业用户不行——他们迟早会遇上得自己发明正经的递归特性才能自然支持的实际需求——这种需求来源于算法设计（比如树的遍历），根本上来自甲方，不是你个实现人员想干掉就都能干掉的。另一方面，在这个角度上，把递归算法过程分为空间复杂度意义上能等价实现为循环的（尾递归）和不能实现为循环的，又是递归这种实现细节的*抽象泄漏(abstraction leak)* 。反正递归（调用）这种东西在高级语言横竖都要学而且更有力，为什么要逃避？更不说一般的递归根本不只是调用这种隐含的控制操作那么简单，而是普遍得多，根本没所谓的循环能对应的东西——比如文法产生式的自然复用的表达方式，比如 μ 递归函数这样的替代可计算原语。凭什么面对能正经认识这些堪称计算的基础常识的*那么简单*的东西（就是纯粹的认知演绎，又没要求让你保证会构造 Church 数的前驱操作或者做一坨离谱的证明题这种可能需要技巧性天赋的东西；而且几乎肯定比可能某些导论课就引入的那种不大严格的算法分析还简单——毕竟就算没严格形式语言，也有的是有递归的伪代码让你折腾的）的*专业人士*，还要造谣粉饰认知难度会有问题？甚至非得另外夹带私活塞进什么鸟《编译原理》这种设计上基本全是基于蓝星猴子对“计算”认识的历史的支线和野史的弱鸡课里，打包到不起眼的位置上选修？这些操作是在侮辱谁？非得没事让用户走弯路自己发明一遍历史才叫卷是吧？嫌弃**人矿太多了**是吧？

　　真学院派起来，[用迭代建模递归的正经做法也有，需要超纲得多的条件](https://okmij.org/ftp/continuations/undelimited.html#iter-recur)，以至于相当非主流。可见这种对不对称性的无视根本不分什么学不学院派——无非是学院派挖得更深所以无能的东西没那么能力成为主流。像什么[对象演算（相对于 λ 演算）](https://cs.stackexchange.com/questions/18963)也是，基于类之类的记录(record) 本来就是闭包(closure) 和环境(environment) 缝合起来的东西，非得下克上充当 primitives ，当然别扭——说白了除了没事搞煋闻氵 paper（当然，要能推广了证明这是里的路线多欠揍而不可行，在学术上不是坏事），就是脑子不好使，非得往具体的无能欠揍弱鸡设计去对齐，自身目无原始需求，连对执行的上下文(context) 合理分类都做不到（顺带，另一个重要的上下文是续延(continuation) ）。

　　为什么有些看似应当足够内行的用户会连这些基本的现象都没发现呢？我猜：

* 首先是因为没天赋（正常）有能耐直接无中生有这些说多不多、说少也不少的 PL 常识。
	* 特别考虑到，翻车走错路的设计在历史上比比皆是，包括 C/C++ 那种随便炸栈 UB 却还有连让人拿来当入门语言的搞得用户有**不敢推翻的思想包袱**乃至**教条主义和权威主义**，都得不良倾向过滤掉，返工的工作量一下子就上来了，根本不是再多学一个两个流行语言甚至范式能解决的。
	* 更别说有余裕用用之后学到的或者自己凭空领悟到的普遍完善的理论体系重建新的框架，填充上这些次品知识中尚且能用的部分了。
* 同时，**教育质量普遍太烂**，入门的语言就是大杂烩一箩筐姿势不对，根本没基础静心独立分解特性分析清楚，天赋普通又没本事重新把理论从零发明一遍，也没能力搜得到非主流文献做对比。
	* 顺便，以上几点也是我强调对现在有些计科[入门姿势](https://github.com/FrankHB/pl-docs/blob/master/zh-CN/introduction-to-learning-computer-languages.md)乃至[整个理论体系](https://www.bilibili.com/read/cv26671044/)普遍不满的原因。
* 外加个俗人常见的问题：缺乏自知之明。
	* 对历史和现实的力量都几乎一无所知。
	* 对自己的能力也欠缺准确认知，自大，没足够注意这个领域**光说不练的前提很大程度上就是自己有能力把钦点的东西都凭空发明一遍**。
	* 于是，自己想象什么学院派，大概**不知道自己距离真正的学院派有多远**。
	* 也许还死要面子不承认，不知道碰到了普通人没有机缘接触的领域，已经很凭运气了，所以在正经讨论面前连暴论到点上的机会都肆意浪费。
* 还可能基于无知、无能或者态度问题，不但显示不出了解讨论的核心话题，还显示不出了解讨论对方的立场。我就不时被误解得懒得回复，这里顺带集中处理：
	* 比如 Haskell 之类的 PFP 党我就基于一贯相同的价值观照黑不误，不管多有技巧上的美感（其实很多是丑的）。
	* 而 ML 系的我也不大待见。偏偏 Robert Harper 作为黑 monad 急先锋，反而意见跟我一致。
	* 我还直接黑滥用等式逻辑(equational reasoning) ——这是 Haskell 之类惰性求值语言背后的大本营（讽刺的是大多数批评家根本 get 不到这点）。反正这个不会是主流，虽然能跟得上讨论问题的人是什么的基本是非主流了（对这感兴趣的大多是数学家，或者数理逻辑学家）。
	* 把不同的流派合并同类项是很拉仇恨的。觉得学院派就是个不学无数的垃圾筐什么杂碎都能往里面到是不是？更别提指鹿为马了。这两者加起来效果比谭浩强还要拔群。
	* 对没眼力见的外行来说，有点情商比较好。
* 于是，拿学院派偷懒借口缩小提问的外延省心又省事，在摆脱[路径依赖](#决定性问题)、在理解非特定的高级语言设计这个专业用户都应该能实现的普遍目标的路径上，**自我放纵降点难度**，不会修正方向以至于**还在继续走弯路纠结特例**，就这样了。
* 这大概也是所谓“判定递归的终止条件是需要动脑子的，但是判定循环的终止条件不需要动脑子”的真相。因为不是所有语言的思维方式都需要有这种思想钢印。
	* 比如 SICP 2nd Edition 入门的，只要有尾递归的概念，知道怎么把递归限制为尾递归应对这一类根本相同的算法表达需求，典型用法就不应该有什么比循环多出来的心智包袱；反而循环横竖得多记点关键字或者干脆多一层抽象。

　　至于就因为这里列的理由断定 PTC 只是优化手段（异常的问题单独另外说），这完全是没看 [[Cl98]](https://www.researchgate.net/profile/William_Clinger/publication/2728133_Proper_Tail_Recursion_and_Space_Efficiency/links/02e7e53624927461c8000000/Proper-Tail-Recursion-and-Space-Efficiency.pdf) 这类基础文献。PTC 之上的配置分类 Algol（如 C 的抽象机），再上面是 GC ，能说 GC 相对 Algol 只是优化手段？只要语言足够无能、语义管不动，用户吃到语言语义之外的坑（如内存占用爆炸）也活该咯？

　　为什么这里的问题需要澄清？因为不止是个别用户，心智包袱患者也会抱团危害社会。比如 MISRA C 强调禁止函数的递归调用，理由是为了防止栈溢出，全然不顾非递归的嵌套函数调用在典型实现中制造栈溢出也是分分钟的事（虽然说这里更离谱的是合计给你 1 字节的栈也能是 ISO C conforming implementation ）。这种驯化次等从业人员的教条主义就比个别包袱危害性大多了。

　　当然，真有问题的地方还是存在的。比如 PTC 和析构函数（以及 Racket 的 contract check 之类）这种非尾上下文直接有对象语言语义上的冲突而需要非平凡的解决，**至今仍是没有彻底解决的问题**（之前有看见设计类型系统推断的，我觉得不大对路）。但外行么，说到底就是外行。要不是看在公开讨论并不只是会让个别人受益，不过是不是被当作嗟来之食，都纯属多说无益。

## 动态语言

　　类似的方式可以得到：在动态语言上添加限制使之具有静态语言的优点，比静态语言扩展动态特性（如 CLR 语言加 `dynamic` ）普遍更容易实现目的。

　　同时，尽管是不同的性质，静态类型系统在这里算是静态语言在这里类似。进一步推论：凡是静态语言特性这样依赖阶段(phase) 的特性，都不适合出现在一般的通用语言设计中直接支持，而更适合通过扩展添加。特别地，这包含所有静态类型系统。无类型 lambda 演算扩展为有类型 lambda 演算（还不止一个方向，反过来先发明哪个就麻烦了）、JavaScript 扩展到 TypeScript 、Racket 扩展到 Typed Racket 等等，都是实际成功的例子。

　　单独拿出来说，是因为这说明：只讨论 JavaScript 这样的动态语言，在长期目的上（资源充足，不差钱不差人建设生态）就足够在现有一般编程语言中有代表性（尽管通用性上相比上面的描述都有缩水），而不需要再单独讨论静态语言。静态语言在通用目的语言的意义上，只是通用的动态语言一种配置或者优化，不需要单独提出特定的设计。

## 品味的判断

　　上面的推理还隐含着主观价值偏好：基于沉没成本厌恶，良好的品味至少和任何需要**返工**的可能最终是严格冲突的。既然一眼望得到目标，为什么要先走过头再折返呢？（注意这不是典型的最优化问题，因为即便既定目标不变，具体可优化的可操作目标具有随机性和主观性。）

　　在简单的情形下，折返会经过原来已经访问过的部分路径重叠，也就是和中间态相遇。这是很显然的失误决策的标志物。

　　偏离目标的非必要重叠对应的产出，统称**半吊子**解决方案。当然，半吊子也有合理性：因为人不总是有未来视，一眼望得到目标，曲线救国也是可以接受的。在宏观上，静态的通用目的语言（相对于动态语言）和 AOT 编译器（相对 JS 运行时这样的动态语言实现）这类都是典型的例子，虽然最终应当被取代，但在现有投入连目标的中点都遥不可及的情况下，有单独存在的意义。

　　也有更复杂的但习惯上直接认怂的情况，例如 GC 语言的流行不是因为 GC 语言比非 GC 语言早或者晚，也不是 GC 语言在特定情况下能牺牲内存占用和响应发挥吞吐量优势，而几乎全是因为对大部分用户普遍不理解所有权管理应当是静态设计的一部分现实这种情况的妥协。这种情况只能在路线外另行改变。于是，路线外部的交互对保持品味也是重要的——理想的品味应当有自主性和能动性，以避免被后进者拖后腿，不因后进者用户数量和直接产生的价值多少而动摇（要说大部分开发者写垃圾代码没有功劳也有苦劳，能保证比得上被他们吃掉的岗位本来能写的不那么垃圾的代码的人产生的效益么）。

　　这里还有个问题：万一路径上打断了怎么办？最终进展可能还不如半吊子。这的确普遍存在，属于固有风险。但统计上，如果同时试错的实例足够多，打断造成的不可利用的沉没成本期望越低（证明略）。这是为什么看似激进的 flavor 仅仅为了最终遇见的正确性也需要适当扩大生态建设的原因。风险并没减少，实质上是平摊了。但半吊子的无效产出开销会减少（证明略）。

## 例外

　　也有事实上非常普遍重要的性质仅因为使用上的领域特定性而需要单独提出。这里只提一个相关的：**光滑性(smoothness)** 。这种性质允许语言中的设施被**原样**地同时用于元语言和对象语言中。这是先前提出过的**降低语言设计可触及的复杂度下限**的直接技术实现手段。大部分语言完全不具有这项特征。注意这种特征普遍是语义上的，通过元编程可以转换到语法范畴中。而对应在确定语法范畴（目标语言）上的常见典型弱化是**同像性(homoiconicity)**。

　　光滑性猜想(rephrased, Shutt 2010)：光滑性对最大化语言的抽象边界(radius of abstraction) 是必要的。

　　（抽象理论的形式框架略。）

　　考虑到语言设计过程（包含标准化的主要部分）是程序性的活动，对其自身进行元编程的描述再次应用猜想的结论，可以确保实现整个活动复杂性、使之和特定“有意义的”改动的开销接近理论下限目的。

