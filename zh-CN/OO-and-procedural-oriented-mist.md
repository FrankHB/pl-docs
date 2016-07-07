# 面向对象和所谓的“面向过程”

## 摘要

　　本文综述面向对象，尤其是面向对象编程的基本概念和一些其它编程范型的比较，并指出了现有初学者的一些常见误区。

## I.面向对象(Object-Oriented, OO)综述

　　公认的面向对象是一种“思想”，更精确地说是一种*方法学(methodology)* 。*面向对象编程(OOP)* 、*面向对象分析(OOA)* 、*面向对象设计(OOD)* 等范畴是对此的衍生。容易理解， OOP 指使用 OO 的方法进行编程； OOA 和 OOD 分别指使用 OO 的方法进行系统分析与设计（可以合称 OOAD ），是 OO 方法学在软件工程上的应用。对于使用 OO 方法学的软件开发， OOP 是基础也是具有更强的普遍性（即便使用 OO 方法，也并非所有的软件都有必要使用系统化的 OOA 和 OOD 进行开发），通过 OOP 可以反映 OO 在编程实践中的重要应用，因此本文着重论述 OOP 的有关内容。关于 OOAD ，读者可以在了解OOP的基础上自行学习。

## II.编程范型(programing paradigm)

　　是计算机编程中的一种基本方式[[en.wiki:programming paradigm]](http://en.wikipedia.org/wiki/Programming_paradigm) 。 OOP 和*命令式编程(imperative programing)* 、*函数式编程(functional programing)* 、 *逻辑编程(logical programing)* 并列，是当前主流的编程范型[[Kurt Nørmarks]](http://people.cs.aau.dk/~normark/prog3-03/html/notes/paradigms_themes-paradigm-overview-section.html)。此外还有*结构化(structured)* 、*声明式(deriective)* 、*面向方面(aspect-oriented)* 、*数据驱动(data-driven)* 、*泛型(generic)* 、*并行(parallel)* 、*元编程(metaprograming)* 等各种范型。

　　应该注意的是，这些范型并不都是同一层次上的风格，且由于分类方法的不同，不都是互斥的。当然也有些范型是对立的：结构化与*非结构化(non-structured)* ，但这是少数。因此通常在同一段程序中使用了一种以上的编程范型，只强调其中的一部分。

## III.结构化编程(structued programing) 、命令式编程(imperative programing) 和过程式编程(procedural programing)

　　早期的程序没有强调任何范型，是非结构化的。结构化程序由子程序的（顺序）执行、选择、迭代构成，无需跳转。

　　命令式编程是一种重要的编程范型。它和声明式编程相对，强调特定路径执行的步骤（控制流）使程序的状态向预期改变，而非和执行路径无关的计算逻辑的表达。大部分硬件实现的体系结构都直接支持命令式范型。可执行的语句作为语言特性，是支持命令式编程的重要特征。

　　过程式编程有时被看作是命令式编程的同义词，也可以表示一种基于结构化编程和过程调用(procedual call) 的编程范型[[en.wiki:Procedural programming]](http://en.wikipedia.org/wiki/Procedural_programming)。过程（或例程、子例程、方法、函数——注意不要和函数式编程混淆）在这里是可以通过调用这一手段被重复执行的程序片段，作为语言特性，是支持过程式编程的语言的重要特征。

　　下文中提及的过程式编程都指第二种含义，而第一种直接称为命令式编程。

　　无论是命令式还是过程式的编程范型都被许多编程语言广泛支持。对于 Pascal 、 C 、 C++ 、 Java 等语言，两者都是最基本的（使用时几乎无法避免的）范型，而“结构化”通过这两个范型的上层被体现。

## IV.作为编程范型(programing paradigm)的OOP

　　OO 程序可以看成一系列对象的交互，而不是如传统的过程式编程那样执行一系列任务（*过程* ）。

　　如 OO 字面所说，*对象(object)* 是其中的一个要素。

　　OOP 中每个对象都有能力接收、处理和向其它对象发送*消息(message)* ，可以被看作具有不同角色或职责的独立单元。对象的动作（或“*方法(method)* ”）与之紧密相关。例如， OOP 数据结构倾向于携带自身的操作（或至少从类似的对象或类“*继承* ”）。

　　对于过程式编程，程序可以被分解为若干过程。 OOP 的做法不同——它分解程序为若干对象和对象的交互（尽管其中的“方法”仍然可以扮演过程式编程中的过程的角色）。

　　传统的过程式编程中过程对数据的访问是不加外部限制的，而OOP的做法不同——它使用访问权限控制等特性，强调封装，鼓励程序员使某一部分的数据仅可被特定的程序片段（过程）访问，以减少可能发生的错误。

　　对象和消息是 OOP 的核心。 OOP 和对类型系统的抽象导致*类(class)* 概念的出现。使用类作为核心特性的语言实现 OOP 的*基于类的风格的(class-based style)* OO ，这是支持OOP的语言中的主流，如 C++ 、 Java 。与之不同的是基于*原型的风格的(prototype-based style)* OO，如ECMAScript。

　　当前流行的 C++ 、 Java 等“主流面向对象语言”（事实上把 C++ 称为面向对象语言并太不恰当，见下文），其实并不能算最符合 OOP 的基本要义。原始的 OOP 的观点，见面向对象之父 Alan Kay 的阐述[[Alan Kay]](http://userpage.fu-berlin.de/~ram/pub/pub_jf47ht81Ht/doc_kay_oop_en) 。

　　OOP 常具有如下几个特性：数据抽象、封装、消息、组件化、多态和继承。注意，这些特性并非同时提出的，因此并非都是 OOP 的必要组成部分；也并非 OOP 的专利。

　　应该指出的是，即便不使用直接的 OOP 特性，也可以进行 OOP 。例如 C 可以使用结构体模拟类。

## V.语言相关的实例：Java和C++的OOP和其它范型的支持策略的比较

　　有些语言为特定的范型设计，鼓励用户使用特定的范型。如 Java 鼓吹的“简单”的“纯” OOP 。但是通过上文可以知道， Java 支持的面向对象只是一种基于类的风格的 OOP ，并且血统远非纯净。另外， Java 不支持多重（实现）继承而只支持接口继承，即便仅从基于类风格的 OOP 上来说也有缺陷（由于这点，加上 Java 缺乏其它一些有用的特性，这给*混入(mixin)* 等带来麻烦，也容易造成代码冗长）。语言构造也决定 Java 无法摆脱命令式和过程式编程，在这个意义上并不“纯”。（尽管 Java 支持泛型，但功能过于薄弱，在此忽略。）使用好 Java 需要用户对 OOP 的较清晰全面的理解，事实上一点都不“简单”。

　　另外一些语言不强调（甚至弱化）特定的范型，鼓励用户选择合适的范型，如 C++ 。

　　关于 OOP 两者还有一些整体上重要的、原则性的不同。 Java 的 OOP 特性支持的设计试图减少 OOP 和 OOAD 的差距，使 OOAD 的结果尽量能直接 OOP 对应。初衷可以理解，但效果不见得好（取决于用户设计的合理性），反而几乎肯定增大了语义上的复杂性——这点的一个例子是“继承”不考虑 private 成员。而 C++ 的设计事实上无视这一点。 C++ 的 class type 并不见得就是 OOAD 意义上的 class ，它可以是类似 Java 里的 interface 或者和 OOP 无关的东西——如元编程用到的 traits 。这点可能导致学习上的困难，但增大了灵活性。

## VI.结论：一些需要避免的误区

　　首先，所谓“面向过程”并不是公认的编程范型的名称，无法和过程式、 OOP 等相提并论。提出这个生造的术语可能只是效仿“面向对象”的构词，却忽略了其中的内涵。通常使用这个术语通常似乎是想表达“过程式编程”。在已有更精确和广泛接受的术语的情形下，不应该使用这种模糊的称谓。

　　其次，关于语言和范型。尽管语言可以直接通过语言特性支持范型，但范型实质上是跟语言无关的。尤其对于 C++ 而言，强调“支持面向对象”作为和其它语言的主要区别，是没有根据的。

## 参考文献

[en.wiki:*] 英文喂鸡，喂度娘，略。

[[Kurt Nørmarks]](http://people.cs.aau.dk/~normark/prog3-03/html/notes/paradigms_themes-paradigm-overview-section.html)

[[Alan Kay]Dr. Alan Kay  on the Meaning of “Object-Oriented Programming”](http://userpage.fu-berlin.de/~ram/pub/pub_jf47ht81Ht/doc_kay_oop_en)

