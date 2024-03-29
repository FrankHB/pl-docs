﻿# 组合子语用批判

# 引言

　　近年来在讨论编程语言及其教学的议题中有少数不同观点。

　　这其中有一股值得注意的历史逆流。

# 样品

[知乎回答](https://www.zhihu.com/question/30467199/answer/490183958) by bhuztez 。

　　表达个人观点，整理和保存资料并非具有冒犯性，在普遍意义上是对公众有益的。但是，为了个体好恶而无视一般人具有的更深层次的利用计算的方式，鼓吹个别风格，则是无益的。

# 问题分析

> 未来是属于以APL为首的组合子邪教的，括号家族的好日子，我看也快看到头了。

　　括号是否需要到头暂且不论，但基于对正常认知的尊重，必须旗帜鲜明地指出，滥用组合子的风格，本身就是一种邪教。

## 概要

　　组合子在此的最重要的性质是**排除一般意义上的用户定义的符号指称**，具体点说，它禁止任何允许用户引入和上下文相关的变量。

　　与组合子风格对立的，并不是所谓的 LISP ，而是所有**支持带作用域规则的变量**的语言设计。

　　这种设计最早被 A. Church 在 λ 演算中较完整地形式化，所以对立的源头倒是没错。但正因如此，反映了该文作者在相关领域欠缺历史常识：λ 演算的流行并不是因为语言设计的倾向性，而是**任何实用到需要允许用户引入变量的语言（不论其设计者是否意识到），都会在操作语义上落入 λ 演算及其变体**。

## 可用性

　　带有作用域规则的变量在 λ 演算中以 λ 抽象(lambda abstraction) 的形式提供，对应于一般语言设计中的用户自定义的过程和函数。λ 抽象就是这种实体的构造器。与此相对，组合子本身并不允许这种构造器，而只允许排除依赖上下文的符号的输入。这给人类用户使用编程语言带来普遍的困扰。

　　应该注意，在**表达计算**上，λ 抽象不是必要的，如纯粹的[组合子逻辑](https://zh.wikipedia.org/zh-cn/%E7%BB%84%E5%90%88%E5%AD%90%E9%80%BB%E8%BE%91)可以具有同等的可计算意义上的表达能力。

　　但是，在**抽象能力**上，两者并不等同。缺乏抽象支持的纯粹组合子系统显著缺乏一些抽象能力。技术上，不需要考虑“抽象”的精确定义，这已体现在：

* 组合子逻辑的语言相对并不容易表达一般的逻辑，因为这依赖在已知组合子中判断选取是否能符合程序目的的子集。
	* 人类用户在这个意义上普遍是弱势的。
	* 依赖寻找恰当的组合子进行编程，和在语法分析树中暴力搜索一个程序的难度近似。
	* 对人有意义的、不限制长度的一般组合子程序往往通过对应的 λ 演算的程序基础上消除(eliminate) 抽象得到。一个例子是 [unlambda](http://www.madore.org/~david/programs/unlambda/#lambda_elim) 。
* 相反，使用类似 λ 抽象的自由变量这样带有上下文的变量，重写系统规则允许人类编码不影响程序语义但可能具有附加意义的等效编码。
	* 具体地，λ 演算允许对自由变量 α-重命名等价。这对应一般编程语言中，变量命名不需要影响程序的语义（尽管可以附加人为的其它限制来约定不同的命名风格允许出现的上下文）。
	* 这种性质允许代码的作者选取具有对人类读者特定含义的普遍命名，而不需要把这种含义在程序中以符合语言的语义规则的构造完全地实现。
	* 这种分离实现构造和接口意图的做法，允许用户引入抽象表达意图。
	* 不需要使语言的语义依赖程序的完整构造使语言能够具有非平凡的等式理论(non-trivial equational theory) ，使程序的局部片段能够被单独翻译而不影响这些片段组合之后的语义，这种性质允许了一类最一般意义上的程序优化。
* 纯粹的组合子的表达并不蕴含允许具有不同符号但附加不同意义的程序。除去上述计算上的反人类设计的问题无法简易解决，缺少的抽象以组合子的语用方式也基本无法挽回，而且消极影响会更加严重。
	* 如果要克服缺少抽象带来的作用，需要约定程序构造来作为缺乏命名接口的变通。
	* 若只使用组合子自身的语法——应用组合子的并置连接(juxtaposition) ，这会直接限制组合子之间出现的顺序。这样，同样意图的组合子为应对“重载”需求需要附加编码和领域逻辑无关的片段，进一步混淆了接口和必要的实现。

　　这些理由导致缺少抽象的语言不适合人类用户普遍使用。

　　注意缺少抽象的缺陷根本是语义规则的问题。至于括号，只有线性语法消歧义时才需要。如果抽象语法树直接表示为可视化编辑器中的区域，或者使用允许代码的视觉呈现带有区分片段的颜色等方案，那么不需要括号就能消除歧义。

　　比较语言的抽象能力被作为正式的课题研究，如[抽象理论](https://web.cs.wpi.edu/~jshutt/abstraction-theory.html)。

# 认知偏差

　　该文的作者在人对计算认知的历史和相关专业知识上也存在几乎算是全外行的偏差。

## 历史问题

　　作者认为，Turing 和命令式编程没有直接关系，有关系的是 von Neumann 。这是普遍的误解。

　　实际上，这两人跟编程本身的范式都不大。反倒是 Church 的工作的确和函数式编程有些关系。

　　Church 和 Turing 两人为了解决关于可计算性上的问题分别提出关于计算的模型。一个重要的区别是，Church 提供的 λ 演算作为一个演绎系统(deduction system) ，自身可以作为语言来用，并且和普通的代数方式衔接得较好。事实上日后 John McCarthy 设计 LISP 时，直接拿来了 Church 的部分设计，[是因为计算表达的简易，并没有考虑很深刻](http://jmc.stanford.edu/articles/lisp/lisp.pdf)。所以这个意义上 Church 的确影响了编程语言的设计和使用范式，尽管这不是他的本意。而 Turing 提供的模型并没有被作为语言使用。

　　至于 von Neumann ，主要贡献是和当时一些其他人一起整理和提出了程序和数据统一执行的 Princeton 架构，这是计算机的模型，比计算模型离抽象的语言还更远。

　　命令式编程和程序存储被计算机解释的指令的计算机实现相关，但这并非 von Neumann 的发明（Zuse 的 [Z1](https://en.wikipedia.org/wiki/Z1_%28computer%29) 就使用可编程指令）。而早年流行的串行解释指令在之后的发展中也被证明不必要，现代的命令式编程也不那么强调按字面顺序默认求值。

　　一个关键问题在于，按 von Neumman 的设计，代码和数据并不需要具有体系结构上的原则差异，那么表示代码的指令自然是数据的特例，而不是相反（除非不允许程序存储数据，此时所有的数据都必须编码在指令中）。硬说的话，LISP 的代码即数据反倒是体现了 Princeton 架构，而 C/C++ 之类更多 ALGOL-like 语言则原生地更接近程序代码和数据分离的 Havard 架构。

### 范式

　　同理，程序是表达声明还是命令，普遍是用户的决定。一个充分成熟的设计上并不需要被特别关心，特别是一个通用语言，几乎总是该容忍用户对不同范式的混用。

　　**而通常被认为命令式风格的典型设计，包括语句和一些控制操作，都是更普遍的过程式语言的特例。**

　　注意，虽然历史上计算机实现的“过程”晚于指令出现，但逻辑上整个程序生存期可以被视为活动的过程，只不过不需要考虑程序内部的调用者。所谓的指令或者更抽象语句，说白了只是附加了控制操作符这种抽象过程的程序片段的组合罢了。

　　具体来说，过程式语言的一般构造是可具有递归构造的表达式，计算以表达式的求值表示。表达式的求值可能蕴含子表达式的求值。求值可具有计算作用，包括确定计算结果和副作用。

　　像 ALGOL-like 语言所谓的语句，是保证表达式求值顺序符合语法定义的单一顺序的控制操作的语法糖；把语句视为蕴含子表达式的表达式，这种操作是作用在子表达式上一种控制操作符。事实上在 Scheme 中，这种操作符以特殊形式 `begin` 体现，本身并不需要特别的语法设计。可见“语句”的概念是特殊（多余）的。

　　其实 Scheme 在减小设计冗余上也不咋地，R<sup>n</sup>RS 用的实现是 `lambda` 里写死的（然而原始的 lambda 并没有这个意思，毕竟纯函数式嘛都不在乎子表达式求值顺序），而更具有扩展性的设计（如 [Kernel](https://web.cs.wpi.edu/~jshutt/kernel.html) ）允许把类似的操作分解为其它实体。

　　另外，强调操作语义，任何语言首先都是“命令式”的；传统上“声明式”的设计，总是需要一个类似事件循环的外壳套着驱动解释来表现程序行为，就像处理器加载程序解释指令一样。单说声明式的部分，那还不如直接当作一种源代码输入格式来讲比较贴切。

### 方法

　　为什么要采取这种视角？因为这是所有编程语言的**操作语义**事实上的统一构造。**其它的形式都会有更多的冗余。**

　　这种方法本质上复用了描述系统规则的元语言和被描述的系统中对象语言的规则。其它的形式语义方法，没法直接统一计算机模型的实现，同时作为计算模型和计算机模型，而需要多出定义计算到计算机的映射。

　　λ 演算作为计算模型和编程语言的模型就是这种方法的例子之一，尽管这不是设计者的本义，因为不支持副作用实用性也受限。LISP 则是更实用的例子。相对地，组合子演算可以作为计算模型，但距离计算机自然地更远，仅凭这个理由就不可能有 LISP 的历史地位。顺便，[Forth](https://en.wikipedia.org/wiki/Forth) 这样的语言可能算是组合子逻辑在接近计算机而远离通用计算模型上的对偶。至于更常见的 ALGOL-like 语言，那只能说大杂烩，不说什么光写个随便什么方法的形式语义都够呛。

## Ultimate

> 这就涉及到第一行提出的一个问题，λ是不是the ultimate？

　　历史常识问题。[Lambda the Ultimate](https://softwareengineering.stackexchange.com/questions/107687/what-is-the-origin-and-meaning-of-the-phrase-lambda-the-ultimate) 原本鼓吹的还只是表达问题而不是抽象能力问题。“没必要使用其它计算模型”（怼的是[当年的 actor model](https://en.wikipedia.org/wiki/History_of_the_Actor_model#Scheme) ）还是隐藏在另外的设计（也就是 first-class continuation ）中的。（讽刺的是，OOP 党照搬 actor model 话术的时候把里面的“消息”之类的意思弄拧了。）

　　要是考虑到抽象，那更加没组合子之类什么事了，毕竟组合子就是“排除抽象”出来的。

　　有意见，还可以考虑把 [LtU](http://lambda-the-ultimate.org/) 掀了。

## FP

> 在这些非LISP系的FP里面，比较典型的有两个，一个是prolog，另一个是APL。关于这两个，感兴趣的大家可以自行科普，也可以评论留言。

　　原本的 LISP 设计只是为了强调简单地表达数学意义上函数的计算；之后的实现更接近一般意义上的过程，因此算不上很 FP ——上面的范式也是建立在这一点上。

　　之后自称 functional 的往往是所谓纯 FP 也就是排除支持副作用，这里最明显的派阀是 ML 。纯 FP 里 ML 是 eager 的，Haskell 是对面 lazy 的代表。这哪轮得到 Prolog 和 APL 了？

　　硬说的话 Prolog 反而可以单独划分出 LP(logical programming) 但明显更加 DSL 。APL 是资格老但反而是这里提到过的语言里最没存在感的，不知道是不是用户体验的问题（至少需要特殊键盘输入这点大部分用户似乎都觉得糟心）。

　　至于现代 Lisp 方言是普遍不排除副作用。Scheme 相比更不鼓励副作用，Common Lisp 之类更接近命令式语言的习惯。

　　说到底 FP 很大程度是生造的概念。虽然 John Backus 当年是有搞过叫 [FP 语言](https://zh.wikipedia.org/zh-cn/FP_%28%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80%29)的，但除了强行一个参数和现在流行的少部分所谓 FP 语言类似外，其它设计差得很远，甚至背道而驰（比如强调所有函数都具有一种类型）。至于 LISP 要比这个早得多，不知如何讹传上的。Lisp 方言方面，Scheme 自称过程式，Common Lisp 等自称多范式，本来也算不上多 FP 。因为随便一个过程式语言都能支持数学意义上的函数，FP 就变得愈发不明了。或许 FP 的剩余价值在强调函数作为一等对象(first-class object) 这种正常语言本该具有的特性；不支持一等函数的语言就是非 FP 的。

## FP 和 OOP

　　FP 和 OOP 的同源就是都属于结构化的过程式的范式。其它的，只能说 OOP 套用的概念更加不明所以，比 FP 的内涵和外延不清晰更加具有杀伤力。

　　另外，[OOP 有专用的操作语义模型](https://stackoverflow.com/questions/3113368)，但实际上都不怎么样，现在学术界都过气了。像所谓 object calculus 的 sigma 抄的 lambda ，并没有所宣称的“更简单”，又不符合大多数实际使用的 OOP 语言，还不如用 lambda 和 record 来实现 OOP 。

　　究其根本，流行的 OOP 特性都是些事实上更高层的东西，而要直接表达计算的核心本来就用的是过程式那套，也就是 λ 演算的变体。只有不明所以的一开始就把 OOP 想成新鲜基础的外行才会徒劳地妄图逃脱 λ 抽象的统治范围。就算山寨成功，撑死了也就是一个 λ 演算计算体验接近的方言；作为逻辑模型缺乏对应的传统逻辑的基础，也没逻辑学家会鸟；更别指望扩展出 [λ 立方体](https://zh.wikipedia.org/zh-cn/Lambda%E7%AB%8B%E6%96%B9%E4%BD%93)之类。

　　的确存在和传统风格不太一样的其它扩展 λ 演算的[不同方法](https://www.wpi.edu/Pubs/ETD/Available/etd-090110-124904/)，但跟区区 OOP 没什么关系。

## LISP 和 OOP

　　LISP 和 OOP 没什么直接关系，但 Common Lisp 的 [CLOS](https://en.wikipedia.org/wiki/Common_Lisp_Object_System) 是早期到现在的一个重要 OOP 语言支持实现的代表。CLOS 比大多数 OOP 语言内建的功能更强，比如支持方法多分派。

　　记得《C++ 语言的设计和演化》里 Bjarne Stroustrup 还提过参考了一些 CLOS 的设计。

# 实用性

　　到这里，可以考虑一下组合子真要作为严肃的编程语言构造，发展如何。

　　组合子被作为“不带有自由变量”的 λ 抽象的副产品被发现具有普遍计算表达能力的构造是在 λ 演算发明之后，但显然早于 APL 。如 SKI 这样的组合子演算系统，只提供了少量“标准”组合子。

　　那么照理说，作为实用语言的 APL ，应该派生更多的组合子（以现有的组合子实现这些组合子）来体现设计在表达计算上的有效性。但是，实际上，APL 提供了大量的和程序派生无关的组合子，并没有把它们作为能构造其它计算的基础。这种用法只是常规函数上的限制，在证明可用性（尤其是可扩展性）上远远不具有早期 LISP 的公理化操作符具有说服力。

　　由于组合子之间的派生关系并没有被 APL 这样的语言明确，如何选取组合子就成了用户自己的负担。

　　需要注意的是，自然语言的学习经历普遍不能克服这个困难（且不提自然语言在避免歧义为前提的抽象上相对大部分允许自由变量的人工语言来讲体验极差）。因为组合子之间的可派生性往往非常抽象（且经常缺乏变量名作为暗示），这不像是自然语言中的许多词汇可以由并行累积学习后融会贯通然后潜意识地消解构词上的重复，因此用户需要直接记忆映射符号和具体含义的“词汇表”。不凭借外部实现的知识，这种词汇表在任何时候都难以被压缩。除了具体含义（目的）的存在性，这种词汇表的内部组织形式脱离 APL 这样的具体语言规则后难以复用，空间效用远远不如自然语言，堪称**垃圾知识**。这种用户精力和注意力的浪费和剥削又进一步限制了用户关心如何使用组合子有效派生出程序的可编程意图上。

　　上述种种问题技术上几乎不可能由当前可预知形态的组合子语言解决。因此，组合子以及所谓的[无点(point-free)](https://zh.wikipedia.org/zh-cn/%E9%9A%90%E5%BC%8F%E7%BC%96%E7%A8%8B) 风格在主流编程语言（不管是否通用目的）普遍只会作为“不带有自由变量”的特例，而不是唯一选项。（题外话，“无点”并不容易理解，但叫做“隐式编程”容易带来更大的混淆——隐去的只是指作为函数的变量名，远远不能涵盖其它风格。）

　　适当强调组合子替代无谓的抽象构造可能有助于程序的简化，但并不能作为基本的操作。例如，C++ 的 `std::bind` 不能代替 lambda 表达式，而只能相反（虽然后者也不被无条件地鼓励，因为使用组合子的方式在特定上下文仍是有益的，例如有利于建立表达不关心自由变量这种实现细节的更高层次的抽象）。

　　组合子风格作为计算模型的基础有历史上的合理性，但依赖这种基本特性构建编程语言，鼓吹其普适性却无视无法解决的固有缺陷，不论是否自欺欺人，都是对人类用户的普遍挑衅。

## 技术理由

　　当然，这种企图并非个例。大多数动机是出于“更容易使用”，如 John Backus (1977) 鼓吹的[函数级编程](https://zh.wikipedia.org/zh-cn/%E5%87%BD%E6%95%B0%E7%BA%A7%E7%BC%96%E7%A8%8B) ：

> 编程语言似乎遇到了麻烦。每种后续语言，虽做了一点清理，都合并了它的前任们的所有特征，并加上了更多的特征。[...] 每种新语言都宣称了新的和时尚的特征... 但是明显的事实却是，很少有语言使编程者能足够廉价或更加可靠的去验证生产和学习使用它们的代价是值得的。

　　这种观点的问题在于：

* 强调纯粹的函数级编程，即无点风格，相比其它普遍流行的高级语言，要求在语言中取消（或添加上下文限制）引入的变量名指称实体的能力。这和在语言中取消（或仅限个别上下文）引入副作用的能力是类似的，导致**需要解决本不需要存在和解决的问题**。
	* 这是放弃既有高级语言普遍有效的设计，而改用抽象上更低级的语言来实现表达计算的意图的实例。这种路线的价值在方法论上极其可疑。
	* 相比既有的语言，这样的“纯粹”设计显然地不实用。因为变量名和副作用这样的设施在解决通用领域问题中的普遍性，排除这些设计通常就是把如何引入这种特性的问题丢给了语言的用户。然而经验表明，在缺乏直接支持这些特性的语言中，用其它特性组合出可用的设施实际上并不容易——至少并不容易做到易于无冲突地表达意图。
	* 如先前指出的，程序语言理论的发展将允许精确地刻画描述抽象能力。即便如此，不太可能推翻以上经验性的结论：既有的语言在这些最基本的普遍设施的设计上，相当接近理论可能允许的平衡设计简单性和普遍可用性的需求之间的最优解。因此，不论使用何种具体设计，只要实现从既有的设计中去除这些设计的企图，极可能总是会自然地损害整体上的实用性。
* 而增加一个不能完全取代适用旧有场景新风格不会使语言容易被验证和学习，反而容易增加学习和选型成本，特别是两种风格不总是容易互相迁移的情况下。
	* 同时支持不同风格的语言中，容易存在互操作性问题。这还容易引入本不相关的翻译阶段(phase) 依赖而引起新的心智包袱。
		* 注意语言实现不是强人工智能，不可能彻底理解使用语言实现的完整意图，原则上无法代替人类用户实现决定如何命名的策略，因此不可能完全地（在语义上无损地）自动变换迁移不同的接口设计和实现风格。
		* 一个另外的典型的例子是 [Boost.Hana](https://www.boost.org/doc/libs/release/libs/hana/) 试图解决 C++ 中“类型级”不如原有的更一般的“值级”计算在表达上更自然的问题。而它仍无法解决的“类型级”弱于“值级”计算在抽象能力（特别是表达副作用能力）上的缺陷。
* 实际上一般意义上的无点风格在调用中传递和返回函数隐含一等函数的支持，所以[被当作传统函数式风格的一部分](https://dl.acm.org/doi/10.1145/72551.72554)也并不奇怪。反而是刻意区分不同的风格来划分语言子集的做法缺乏显著必要性。
	* 计算模型上也不必要，详见以下相关讨论。
* 函数式以外的场合中，滥用具体风格可能放弃语言更普遍的良好定义的性质。
	* 惯用无点风格除需要表示操作的原语(primitive) 外，可能更依赖运行环境的结构而泄漏抽象，而损害整体的抽象性。
		* 这样的设计通常难以免除用户想象作为细节的具体资源分配（而不能直接把资源以对象代替），例如不得不关心一个栈在计算的什么阶段取得了什么参数。相对地，类似 λ 演算的语言直接使用纯粹的重写系统，在计算结果外只要考虑副作用即可。
		* 一个例子是 Forth ——和 LISP 类似地同样具有特色鲜明的求值规则，但和 LISP 不同，抽象更为薄弱。
	* 片面地鼓励的上下文相关的构造，为了配合参数的使用顺序，可能强加任意的文法复杂性。
		* 例如，为了[避免调整特定操作数的顺序而不惜引入消歧义](https://www.zhihu.com/question/55490625/answer/145033672)，使分析依赖语义而取消语法的独立性。
* 忽略了鼓励构造无点风格的函数的实质——滥用消除参数后的调用形式的等价性（在 λ 演算中的 η-等价）——而具有限制和代价。
	* 这直接限制参数的可能附加的计算作用，而影响抽象的可用性。
		* 例如，RAII 资源不能直接被消除，因此和无点风格不兼容。
	* 保持等价的变换是非局部的。不论需要向无点或常规风格调整，调用者和被调函数都可能需要同步修改。
	* 滥用无点形式的代码可能难以被理解。
		* 当存在逻辑上需要同时处理的多个参数时，隐去参数会造成理解的困难而破坏可读性。
		* 特别地，在没有类型签名辅助推导的情况下，理解代码的合理手段就是转换为带有参数变量名的通常形式。
	* 存在不少可以被理解但可用性存疑的情况，难以明确必要性。
		* 参数变量名明确是语法噪音时，无点风格才明确更好。这种情况在多数领域中较少出现。
		* 无点风格可减少不必要的参数命名。但是不使用无点风格，对参数进行命名也不应有困难。
		* 无点风格可能使程序更简短。但对同步修改多个函数的情况，这种判断可能难以简便和正确地实现。
* 更多时候，这种风格直接造成实现困难，因此需要限制使用。
	* 无变量(variable-free) 风格根本不能表示需要被复用的同一计算结果，除非这些实体的引入和消除没有计算作用而总是视为同一；但后者的冗余变量本身就因为分散关注点而有碍可读性，几乎总是不被鼓励出现。
	* 函数的构造可以明显更困难。
		* 一般的情况下，逻辑上函数可能需要多个参数作为输入，不易直接构造无点风格的写法。
		* 对鼓励这种风格的设计，语言需要限制函数参数，而另外调度其它参数。这种限制这在 John Backus 最初的 FP 语言和一些现代函数式风格的语言中都有体现。
		* 若使用 Currying 调度参数，调用的形式很容易不必要地复杂。
		* 受到控制操作符影响，有条件使用的参数不容易构造为无点形式。用户可能需要人工进行控制流分析。
	* 大多数语言中不限制无点风格的程序，但用户并不普遍使用这种风格，也说明应用场景有限。
* 考虑计算模型的性质，只使用这样的设计的语言在抽象能力上和实现的有效性上都是普遍较为薄弱的。
	* 无点函数风格对应的计算模型上普遍具有劣势。
		* 无点函数风格不使用 λ 抽象，自然也不使用 λ 抽象的应用以（可能依赖环境的）替换作为形式参数的变量来定义函数。这更接近 [μ-递归函数](https://zh.wikipedia.org/zh-cn/%E9%80%92%E5%BD%92%E5%87%BD%E6%95%B0)。
		* 相对 λ 抽象定义的 λ-递归函数，它具有取消变量和以及与传统的逻辑系统关联性的劣势，也缺乏可实现性和抽象能力上的明确优势，因此除了基于自动机理论讨论复杂度类外实际上很少用到。这样的模型来指导实用语言设计的必要性上非常可疑。
		* 此外，类似的模型自身严重缺乏如 λ 演算的扩展性：通过修改局部规则构造出新的实用模型的能力。
			* 除去类型系统添加进一步限制的有类型 λ 演算，在 λ 抽象上直接补充细化实现替换的结构和需要满足的附加约束，以及补充状态和控制作用都可以得到保持一定重要可用性（如 Church-Rosser 性质）的模型（如[先前提到的扩展](https://www.wpi.edu/Pubs/ETD/Available/etd-090110-124904/)的例子）并描述实用编程语言。
			* 在缺乏抽象构造器作为中间层的其它演绎系统中，没有如此多的成功应用于编程语言中的例子，甚至能保持多少有用性质都显得可疑（就先无视相对于 λ 演算少了抽象的问题好了）。
		* 区分函数级程序和值级程序在元语言中并非必要。
			* 即便无变量风格的构造器使用完全不同的规则，同为函数构造器，它仍然能被嵌入到以 λ 演算作为基础的系统中，这样的系统描述程序级和值级共存的语言，但这个意义上它就是一般意义上的支持一等函数的现代函数式语言——即便写到元语言里也不是静态的，用对象语言的通过使用 λ 抽象实现的机制替代也可以无法分辨。
			* 而更加纯粹的、限制更大的、拒绝嵌入具有 λ 抽象的纯函数级语言，才需要原始的（更弱的）不包含 λ 抽象的系统描述。
	* 就语言设计来讲，即便其函数定义排除了变量替换，为表达求值规则，一样需要在元语言中引入替换的规则，起草语言规范时整体并没有显著的简化，反而无法在对象语言中可用。（元语言的不必要的单独复杂性这也是 Lisp 方言外绝大多数语言的通病。）
	* 对实现来讲，缺乏能在函数内局部使用的环境和名称解析的支持使之自举更为困难，实现 meta-cirtualar evaluator 这样的自解释机制更加困难，使动态程序翻译需要有更多的冗余，而更难彻底复用现有的运行时实现。

# 语法

　　另外，[Notation as a Tool of Thought](https://www.jsoftware.com/papers/tot.htm) 的观点，在不被曲解的情况下，不能支持类似 APL 这样的语言的普遍适用。

　　这篇文章指出了记法作为语法设计的基本构成的重要性，旨在批判传统数学在符号系统上的随意设计缺乏关联（和一致性）导致的一些后果。因此，APL 这样的语言相对具有先进性。

　　超出这种范围的应用遇到的问题，不是这篇文章所要表达的内容。这篇文章中的内容也无法解决：

* 文章提到的应用是对固定规则的应用的举例表示，没有一个元语言性质上体现可编程性的例子。
	* 实现上，这些问题自然不需要使用 meta-cirtualar evaluator 这样的抽象机制。一般的编程语言足够应付。
* 文章提供的可供用户编程的表示都是实现意义上的，使用场景有限，并没有解决更一般的抽象缺失。
	* 这就像集合论引入的自然数构造的方法不足以取代用户使用数码对自然数进行符号化的表示的需求一样。
		* 即便是 APL ，在自然数上也没有要求分解为其它组合子的组合，而是直接提供内建的符号。
	* 根本上，这只是具体的实现，而想要表达接口，则需要另行编码。
	* 讽刺的是，要原地引入解释器，可能需要和 meta-cirtualar evaluator 同等重量级的计算设施。
* 作者了解在区分上下文的认知，但只是基于已有的应用举例给出了“模式”这样的粗粒度方案，并没有充分讨论其优缺点。
	* 实际使用时能发现这种上下文的构造非常薄弱。
	* 当然，大多数语言未必擅长解决这样的问题，但允许不依赖现有符号的组合而是直接通过过程定义替换抽象，实用上显然更加有效。

　　关于语法问题，另见[这里](about-syntax.md)。

