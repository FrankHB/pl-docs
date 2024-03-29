﻿# C & C++广义类型系统缺陷

Created @ 2013-04-16, r2 rev 2013-04-16, markdown @ 2015-09-15.

# 类型(type) 和 `const`/`volatile` 限定符(qualifier)

　　类型是程序设计语言中广泛使用的重要特性，被用于抽象数据和程序行为。类型之间遵循语言的若干语义规则，这些规则和类型一起被总称为语言的类型系统(type system) 。

　　C 语言具备较完整的静态类型系统，类型能够通过组合产生新的类型（数组类型、函数类型、指针类型等）。C 的类型对象类型（包括作为不完整对象类型的 `void` ）和函数类型。C++ 在这个层次上没有太大改进，增加了引用类型，而把void类型排除在对象类型之外。

　　`const`/`volatile`限定符通过限定对象类型体现主要语义（但也用于 `void` ），是类型的组成部分。

　　关键字 `const` 在 20 世纪 80 年代引入 C 语言，表示禁止写入对象的只读语义，以便实现利用只读存储器[C++ D&E]。标准化 ANSI C 时使用了类似的语法和语义规则增设了 `volatile` 限定符，语义为读取被限定的对象存储值和写入值时也产生副作用，禁止实现缓存读取的值而减少读操作，能有效提供对I/O寄存器等的映射[C99 Rationale]。

　　而 C++ 中的 `const` 相对有只读以外更激进的语义：对于特定的类型构建（能在编译时确定的）常量表达式。这在某种意义上提升了优化的可能性，但造成了语义的复杂。（题外话，`volatile` 在 Java/C# 有其它不同的含义。）

# 左值性(lvalueness) /值类别(value category)

　　ISO C 以及 ISO C++98 和 ISO C++03 的左值性(lvalueness) 是一种二值系统：C的取值分为左值(lvalue)和非左值；C++中分为左值和右值(rvalue) 。任何表达式的左值性取值都是二者之一[ISO/IEC 14882:1998, ISO/IEC 14882:2003]。

　　ISO C++11 的值类别(value category) 是左值性更复杂的演进，基本类别构成三值系统：纯右值（ prvalue ，相当于 C++03 的右值）、消亡值（ xvalue ，新增的值类别）和左值(lvalue) 。其中纯右值和消亡值合称右值，消亡值和左值合称泛左值(glvalue) 。任何表达式的值类别取值都是三种基本类别之一[ISO/IEC 14882:2011]。该演进主要是因为右值引用的引入而显得必要，详细动机参见[ISO/IEC JTC1/SC22/WG21 N3055]。

　　左值性来源于 B 语言需要对表达式的左操作数的做出语义限定的需要：只有左值(left-value) 才能作为“=”、“++”和“--”的左边（……以及“&”的右边）——此时 lvalue 显式地作为文法元素；类似地，出现在“=”右边的元素相对称为 rvalue[User's reference to B]。左值的概念后来被沿用至 C 和 C++ 语言。

　　由于左值也同时被作为 `&` 操作符的操作数的语义限定条件，实际上表示可能在内存中保持的对象——存在对用户可知的对应的内存位置(memory location) ，lvalue 也被叫做 locator value ；而C中所谓的右值(rvalue) 一般即为值(value) 的同义词[ISO/IEC 9899]（ C++ 的值则可以是实现定义的其它概念）。

　　注意 C 和 C++ 的左值性在函数类型的表达式上有差异：C 的函数指示符(function designator) 不是左值也不是右值。而 C++ 的函数名是左值。

# 左值变换(lvalue transformation)

　　以上讨论的是原始的左值性/值类别。在 C/C++ 中，通过特定的、仅有少数求值（语法）上下文例外的隐式转换，转换的结果的表达式的左值性/值类别可以改变，同时类型也可以改变。

　　这些转换在 C++ 中为便于重载解析(overloading resolution) 描述而被统称为左值变换，包括三种标准转换(standard conversion) ：左值-右值转换(lvalue-to-rvalue conversion) 、数组-指针转换(array-to-pointer conversion) 和函数-指针转换(function-to-pointer conversion) ，其中值类别由泛左值转换为纯右值（对应 C++98/03 中左值转换为右值），同时后两者有类型变换，分别为数组左值 e→&e[0] 和函数左值 e→&e 。

　　转换被排除的少量上下文是作为（按 C++11 的说法）非求值操作数(unevaluated operand) 时、操作数需要左值时以及初始化左值引用时。

　　在C中也存在类似转换，排除的上下文也类似（当然没有用于初始化引用的情形），但只有第一种被明确命名为左值转换(lvalue conversion) (\[ISO/IEC 9899]) 。

　　左值性/值类别虽然不是 C/C++ 的名义上的类型，但从目的（静态分析区分操作、转换以及检查类型错误）来说，符合静态类型系统的一般特征。因此本文称为广义类型一致处理。

# 左值、限定符和引用类型：冗余和复杂

　　一个主要的问题是左值性和 `const`/`volatile` 限定符（作为类型的组成部分）的不完全正交组合造成的冗余。

　　对于 C 以及 C++ 中排除类类型(class type) 外的非左值表达式，`const`/`volatile` 限定符被自动丢弃。这样，存在5种等价类：

* 非左值：不可写、读不产生副作用；
* 左值：可写、读不产生副作用；
* `const` 左值：不可写、读不产生副作用；
* `volatile` 左值：可写、读产生副作用；
* `const volatile` 左值：不可写、读产生副作用。

　　显然，这里非左值和 `const` 左值在写权限和副作用是重复的，除了作为 `&` 和若干转换的操作数。

　　对于 C 来说，这样的区分尚且是有意义的：`&` 能返回 `const` 左值的指针来间接获得所需的值，代价是需要占用存储；而非左值则只能立即使用值，但不需要被储存。

　　而在 C++ 中，这样的设计就有些匪夷所思了：明明存在能绑定右值的 `const` 左值引用来获取右值对应的值，为什么还需要 `const` 左值？

　　对于 C++ ，另一个冗余是，引入（左值）引用表示的左值，除了存储以及参与不同的重载（但显然能和对应对象类型产生歧义）外和对象左值没有区别，这对于内建表达式基本没有意义，保留不相同的规则造成不一致性。

　　事实上，C++ 中，（左值）引用正在取代传统非引用类型的左值的地位，早在 1992 年标准化开始时的第一篇公开论文[X3J16/92-0053 WG21/N0130]就提议这点，并在之后各种场合改头换面、缺乏直觉规律地出现（如模版左值类型推导、`decltype` ）。但内建操作符表达式左值的类型并没有修改（比如内建赋值和前置返回引用），不和其它 C++ 特性一致而和 C 保持一致。这点很容易被忽视而导致一些误解。

# 可修改(modifiable) 左值的意义

　　在 ISO C/C++ 中，直觉明确的可写权限并没有被定义，而是规定了对左值的可修改(modifiable) 的概念，事实上表示可写左值的子集。

　　这个概念的一个重要应用是禁止数组左值作为赋值的左操作数：ISO C中数组类型的左值就明确不是可修改的。但事实上并没有明确违反语义规则时究竟有没有已经左值转换（转换为右值不能在“=”左边——如果这点成立，可修改左值的规则在这里是冗余了）。这种含糊的归类导致标准在起草措辞(wording) 上的一些问题，例如在 ISO C++ 中遗漏了明确的数组左值的可修改性表述（虽然可以推测合理的情形是和 ISO C 一致），可能是潜在的缺陷。

　　另外，对于位域(bit-field) 类型，可能是因为按字节地址寻址的困难性，尽管仍然能作为可修改的左值，也需要额外明确。可修改在语义上无法帮助减少这里的措辞。

# 为什么 C/C++ 在这里不够理想：解决的困难

　　抛开现有的语言特性，从设计新语言的角度看，相关的基本需求不难总结：

　　作为基本抽象，对象或者值需要可读；

　　不同于所谓“纯”函数式语言，能够保存状态，允许（但不滥用）副作用带来的表达计算的便利是 Algol-like 语言的基本需求之一；

　　作为能够容易操作硬件的语言，需要类似使用 `volatile` 的机制提供映射硬件存储的能力。

　　下面可以证明满足以上需求的设计可以比现有的更简单，而不必要有如此多的冗余和模糊（注意，适合实现或者通过现有语言演进，是另外一回事）。

　　显然，不完全正交的广义类型不如合并为有以下分类的一个类型系统显得更清晰：
值：可写、读不产生副作用；

* `const` 值：不可写、读不产生副作用；
* `volatile` 值：可写、读产生副作用；
* `const volatile` 值：不可写、读产生副作用。

　　这种方案要求需要对所有值一视同仁地提供 `&` 等现在的左值具有的操作，若不考虑彻底放弃或加上使问题更复杂的其它限制的话。（生存期需要另外一些规则，但相比之下不是太大的问题。）表面上看，这阻碍了值“不要求存储”的优化；但是除了 `volatile` 类型外，关于抽象机的可观察行为的等价语义可以很容易挽回这一点，至少对于 C 来说最终只有一点关键的阻碍—— `&` 的结果是什么？

　　基于现在的 ISO C/C++ ，至少理论上并非不可解决：内建&的结果是一个指针类型的非左值，一个重要的语义限制是表示“地址”——但地址相关的可能的可观察语义（指针算术以及值的标准输出）是实现定义的。尽管改变现有的、典型的把地址的取值范围映射至某个地址空间的实现附加的实现复杂性会是非常致命的。

　　但是更致命的是，关于“地址”和“内存位置”等存储实现的（反）抽象：按特定基本单位（字节）连续的存储。这种过于具体的约定换来符合现有体系结构的可实现性（以及关于“布局”的控制，如 `offsetof` 的易实现性），却为类型系统的简化制造了麻烦，同时削弱了用户对象类型的控制：无法抽象出不确定布局但占用存储的类型，也没有能力让用户直接使用标准的手法简洁一致地安排布局。（非常讽刺的是，C++11 在内存模型的定义上更严谨更全面，严格意义上的限制却也更大了。）

　　C++ 的问题更严重些。一个问题是引用类型初始化的非对象复制初始化语义。这点其实也可以通过规定激进的非 as-if （可观察行为）语义来解决（现在ISO C++关于特定的复制/转移构造就这样做，甚至可以无视副作用），虽然这样总体上的语义或许会更模糊，让用户更无所适从。另一点是，在内存模型和布局上一直无法完全由用户控制（如对于有虚函数的类，通过 `offsetof` 计算数据成员地址就无法保证结果可预期），这同时具有贴近现有体系结构实现和便于高层抽象的优点，但另一方面也可以说都是缺点。
　　最后还有一个非常现实的困难：兼容性。假设 C 或者 C++ 确实能通过整体改换设计（幅度显然比当年 C 到 C++ 大得多）解决了这样的问题，新的语言规范如何适应旧的程序？如何使用户能够顺利迁移？从现状（ C/C++ 的用户，以及现有需要维护的项目）来看，这是明显不实际的。

# 结论

　　基于以上讨论可知，要消除类型系统的冗余带来的负面影响涉及过多的核心设计，从现有语言为起点立即改进这些缺陷可以认为是不可行的。对于用户而言，尽量清晰掌握核心概念完成任务是比较实际的。但对于语言自身的维护而言，这个问题导致加入新的语言特性、使不同特性有效协作的难度大大增加，需要长期努力才有望缓解。

