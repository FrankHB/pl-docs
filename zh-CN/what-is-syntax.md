# [高级点的技术交流]什么叫语法(syntax)

Created @ 2015-07-01 08:50.

似乎以前发的主题都搜不到几个，所以窃点精。

先声明，我不是撸这专业的；然而大都数吹逼材料在这个问题上的犯傻过于普遍，都不到点上，太不像样了，以至于熟练工还真能一本正经口胡……

原因大概是从头就错惯了，以讹传讹（就像什么“变量”“堆栈”的意思一样）。

# 0.体例

引文文本使用 `>` 前缀（ markdown 语法时可自动转换）。

包括翻译的引文用 `[[ ]]` 标识。

# 1.词义

中文维基被墙所以照搬英文维基解释翻译一下。

词条 [syntax](https://en.wikipedia.org/wiki/Syntax) ：

[[

> In linguistics, syntax is the set of rules, principles, and processes that govern the structure of sentences in a given language. The term syntax is also used to refer to the study of such principles and processes. In linguistics, syntax is the set of rules, principles, and processes that govern the structure of sentences in a given language. The term syntax is also used to refer to the study of such principles and processes. The goal of many syntacticians is to discover the syntactic rules common to all languages.

在语言学中，语法是一个语言中控制句子结构中的规则、原理和过程。术语 syntax 也指对这些原理和过程的研究。（译注： syntax = 语法学。）许多语法学家的目标是发现适用于所有语言的语法规则。

]]

[[

> In mathematics, syntax refers to the rules governing the behavior of mathematical systems, such as formal languages used in logic. (See logical syntax.)

在数学中，语法指控制数学系统行为的规则，例如逻辑学使用的形式语言。（参见逻辑语法。）

]]

[[

> ## Etymology

> From Ancient Greek: σύνταξις "coordination" from σύν syn, "together," and τάξις táxis, "an ordering".

## 语源

古希腊文 σύνταξις “协调”，由词素 σύν(syn) “在一起” 和 τάξις táxis “一种秩序”组成。

]]

语法学的历史非常悠久，公元前 4 世纪古印度语法学家波你尼(Pāṇini) 撰写的梵语语法《八章书》(Aṣṭādhyāyī) 是一个重要的代表……余略……

事实上，抛开自然语言的历史，词条 [syntax (logic)](https://en.wikipedia.org/wiki/Syntax_%28logic%29) 给出了更贴近这里要讨论的上下文的定义：

[[

> In logic, syntax is anything having to do with formal languages or formal systems without regard to any interpretation or meaning given to them. Syntax is concerned with the rules used for constructing, or transforming the symbols and words of a language, as contrasted with the semantics of a language which is concerned with its meaning.

在逻辑学中，语法是任何形式语言或形式系统中不考虑任何解释或含义的部分。语法和用于构造或转换一个语言中的符号和词的规则有关，这和语言的语义——和含义相关——相反。

]]

[[

> The symbols, formulas, systems, theorems, proofs, and interpretations expressed in formal languages are syntactic entities whose properties may be studied without regard to any meaning they may be given, and, in fact, need not be given any.

在形式语言中表达的符号、公式、系统、定理、证明和解释是语法上的实体，它们的属性可以在不考虑解释和含义的情况下被研究——事实上也不需要。

]]

[[

> Syntax is usually associated with the rules (or grammar) governing the composition of texts in a formal language that constitute the well-formed formulas of a formal system.

语法通常和控制形式语言中决定合式公式的文本的构造的规则（或文法）关联。

]]

[[

> In computer science, the term syntax refers to the rules governing the composition of meaningful texts in a formal language, such as a programming language, that is, those texts for which it makes sense to define the semantics or meaning, or otherwise provide an interpretation.

在计算机科学中，术语“语法”指在形式语言中控制构造有意义的文本的规则，例如对一个程序语言，即明确对定义语义或含义有意义或提供一个解释的文本。

]]

关于形式语言的语法学，实际上是现代数学基础最重要的分支之一（数理逻辑）的主要组成部分。对此的延伸话题限于篇幅不作展开，有兴趣的读者可以查找词条内的链接和参考文献进一步了解术语之间的联系。

# 2.文法(grammar)

细心的读者可能会发现，实际上经常被称为语法的词语并不是指 syntax ，而是上面出现过的另一个容易混淆的词： grammar 。

仅涉及自然语言的话题中， grammar 常被翻译成“语法”，而 syntax 则是“句法”（上面的 logical syntax更惯于被翻译成“逻辑句法”），这并没有造成太大问题。不过在涉及更广的范围下，这样会造成一些误会。因为“句”的概念的微妙差异，导致“句法”这个词失去基础。这个情况下，作为泛指，通译 grammar 为“文法”， syntax 为“语法”。

按[维基](https://en.wikipedia.org/wiki/Grammar)给的定义：

[[

> In linguistics, grammar is the set of structural rules governing the composition of clauses, phrases, and words in any given natural language. The term refers also to the study of such rules, and this field includes morphology, syntax, and phonology, often complemented by phonetics, semantics, and pragmatics.

在语言学中，文法是指任意一个自然语言中控制组合分句、词组和词结构性规则的集合。这个术语（译注：语法学）也指对这些规则的研究，这个领域包括（词语）形态学、语法学和音韵学（译注：不要和后面讲的 phonetics 混淆），经常也涉及语音学、语义学和语用学。

]]

可见，至少在自然语言中，文法学的研究范围涵盖了语法学；作为研究对象，语法现象是文法现象的真子集。

对于形式语言来说，没有系统的音韵学或语音学研究的必要。但是，其它重要的部分，尤其是语义和语用这样明确不属于语法学研究范围的内容，仍然是重要的文法现象。

因此，随意混淆“语法”和“文法”是有问题的。在文法的范围内偷换“语法”是典型的缩小外延的逻辑谬误。

# 3.语义(semantics)

上面的讨论实际上还省略了一个重要的问题：什么是“含义”(meaning) ，和“语义”又有哪些差异？

这里一并澄清关键点：前者泛指所要表达的内容即“目的”，而后者是指对含义的一般研究，尤其侧重和被使用的语言的联系。

提这个问题是因为设计和使用人工语言需要注意的一些现象。人工语言的语法并非自然演化而是人为设计的，仅从动机来讲语法背后往往存在非常具体的含义。但是，这些语言的语义和设计这些语言的语法时已经被考虑并集成在语法规则内部的含义并没有关系。

这样的一个明显好处是，语义规则可以完全和语法规则分离讨论（不像自然语言反复折腾上千年也不会有完全的标准形式）；对于用机器实现语言，语法和语义规则的检查（前者又称为分析(parsing) ）也能够容易被分离，便于复用。

因为缺乏设计和实现语言的机会和必要训练，这个和自然语言不同的特性往往造成不少初学者几乎永远都没有机会搞清楚问题，而对解决一些问题（例如学习新的程序设计语言）造成一些阻碍。

这里的设计要点是有普遍实用意义的。形式文法可以同时描述语法规则和语义规则，但相比之下不管是理论还是实践前者远远更加成熟。具有文法表达的形式语义通常是指称语义或公理语义，这对于大多数语言设计者来说要求过高，且现阶段因为各种局限性仍然缺乏实用价值；所以大多数语言规范并不会给出形式文法指定的形式语义。若在设计时不对语义和非语义部分加以区分，则实现语言更加混乱和困难。

与形式语义的困难相反，基本上任何现在的人工语言都会给出形式语法——一般通过 BNF 范式之类的领域特定元语言来描述。这种形式描述的主要优势是能够对处理语法的系统（主要就是语法分析器）提供自动化实现，例如分析器生成器（经典的如 yacc ）或分析器组合器（如 Parsec ）。因为可以节约大量编写和维护分析器的工作，这些自动化工具为调整语言的语法设计带来了极大的便利。（当然，并不是所有语言都方便这样做，下面的例子会提到。）

由于形式语法的高度普及，有时候提到syntax就指得是形式语法，而其它构造规则可能并不称为语法——尽管它们部分或全部地是传统语法学研究范围的内容。

# 4.词法(lexical syntax/lexical grammar)

在语法中，因为有些最基本的和字符序列直接相关的部分内容的含义相对固定，而不需要特别指定不同的规则，习惯上把这部分语法单独提出称为词法。

在实现上，词法分析和其它更一般的语法分析也可以分离并单独配置；同时，也便于学习者理解语言的语法构造。实践证明这是自然的，因此这种惯例普遍存在。

这样，也可以提升词法到和其它一般语法并列，直接使用文法描述。

当然也不是所有语言都必要单独提出词法（除了指定允许什么字符以外），比如brainf*ck或者组合子逻辑以及这样的语法上异常“简单”的语言。

# 5.实例

在 ISO C Clause 6 中，语言的形式语法在标题 **Syntax** 下澄清，而剩余部分澄清非形式的翻译时检查的约束条件 **Constraints** 和一般的语义规则 **Semantics** （对于库还有 **Runtime Constraints** ）。

ISO C 区分 **Constraints** 的要点是——这些规则和 **Syntax** 一样，必须在翻译时被检查和诊断(diagnostics)。

**WG14/N1570**

> # 4 Conformance

> 2 If a ‘‘shall’’ or ‘‘shall not’’ requirement that appears outside of a constraint or runtimeconstraint is violated, the behavior is undefined. Undefined behavior is otherwise
indicated in this International Standard by the words ‘‘undefined behavior’’ or by the
omission of any explicit definition of behavior. There is no difference in emphasis among
these three; they all describe ‘‘behavior that is undefined’’.

这个意义下， **Constraints** 的规则和语法规则具有同等效力，尽管其中有一部分是形式语法说不清楚的(_constant-expression_) 。而剩下的，违反了就当未定义行为了。

在ISO C++中，这里的设计更加明确。

**WG21/N4527**

> ### 1.3.27 [defns.well.formed]

> **well-formed program**

> C++ program constructed according to the syntax rules, diagnosable semantic rules, and the One Definition Rule (3.2).

> ## 1.4 Implementation compliance [intro.compliance]

> 1 The set of diagnosable rules consists of all syntactic and semantic rules in this International Standard except for those rules containing an explicit notation that “no diagnostic is required” or which are described as resulting in “undefined behavior.”

同逻辑学的习惯，“合式”被作为构造性正确的标准，是可以明确决定的——只不过仍然不要求实现必须确定。有别于未定义行为，这里明确区分了诊断要求，表示了更精细的区别。

ODR 本质上来源于构造性语义规则，不过因为单独翻译链接模型和对于不同实体本身的复杂性，这里做了妥协，直接和语法规则、可诊断语义规则并列了。

另外值得一提的是，虽然 ISO C 和 ISO C++ 的 Annex A 都直接给出了类似的东西，但标题却不大一样： C 是 **Language syntax summary** ， C++ 是 **Grammar summary** 。

其实也不难理解： C++ 的语法和语义虽然在原则上多少是想要明确分离的，具体设计上却自己打脸。

C 的语法接近上下文无关语法(context-free grammar) （考虑到抽风的指针语法所以不纯粹），而C++的情况明显糟糕得多。

因为语言要求的一些消歧义非形式文法规则，严格的、单纯的 C++ 语法分析器事实上并不存在。比如说声明中的 `()` 表示一个函数声明符还是初值符的一部分，不判断现有实体的类型是不知道的，而类型需要经过进一步上下文相关的语义分析才能确定。现有的实现基本上用的是有效然而无奈的笨办法：假定每种文法正确都试一遍，最后留下一个没错的就说明没歧义了。

换句话说，要说 C++ 的“语法”烂，正是因为它连“语法”是什么都说不清楚——规则的边界本来就是模糊的。

大概这个槽点， ISO C++ 的 Annex A 也就老实写 grammar 而不是 syntax 了，尽管和C一样，明明两者都没有给出形式语义……

Java 在这里就好得多，直接给出了文法定义，并明确使用上下文无关文法。

（尽管实际上仍然只有形式语法……是 [Guy Steele](https://en.wikipedia.org/wiki/Guy_L._Steele,_Jr.) 参与起草的关系么？）

**[JLS 8](http://docs.oracle.com/javase/specs/jls/se8/jls8.pdf)**

> 2 Grammars 9

> 2.1 Context-Free Grammars 9

> 2.2 The Lexical Grammar 9

> 2.3 The Syntactic Grammar 10

> 2.4 Grammar Notation 10

当然， Java 的语义一锅乱七八糟的这里不展开了。

C# 大体上抄的 Java ，以及C艹的部分糟烂（又是关于<>的疼货）……

**[ECMA-334 3 2005](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-334.pdf)**

> 9.1 Programs

> 9.2 Grammars

> 9.2.1 Lexical grammar

> 9.2.2 Syntactic grammar

> 9.2.3 Grammar ambiguities

ECMAScript 总体上也是一路货（否则干嘛叫 JavaScript ……）

**[ECMA-262 5.1 2011](http://www.ecma-international.org/ecma-262/5.1/ECMA-262.pdf)**

> 5 Notational Conventions

> 5.1 Syntactic and Lexical Grammars

> 5.1.1 Context-Free Grammars

> 5.1.2 The Lexical and RegExp Grammars

> 5.1.3 The Numeric String Grammar

> 5.1.4 The Syntactic Grammar

> 5.1.5 The JSON Grammar

> 5.1.6 Grammar Notation

C++/CLI 是直接照搬 ISO C++ 偷懒了。

[Haskell 2010 Report](https://www.haskell.org/definition/haskell2010.pdf) 则只是提了下 notation ，看起来都没说啥 grammar ……不知道对混乱的设计有多少自知之明呢。

作为补充说明， [Scheme](https://en.wikipedia.org/wiki/Scheme_%28programming_language%29) 的 [R<sup>n</sup>RS](http://www.schemers.org/Documents/Standards/) 是少数给出了文法描述的形式语义的语言规范。正文符号太疼略。

顺便，注意 Scheme 的语法和 [S-expression](https://en.wikipedia.org/wiki/S-expression) 的语法是两回事。

# 6.结语

抛开形式语法，没有一个实用语言的语法是足够简单到直接能一看就全记住的……

那些提啥“只是语法”还有“语法书”的，真拿得出一本出来么。或者替我贴完 ISO C 的 Annex A 科普一下瞧瞧？

（剩下的懒得写了自行脑补。）

附注： C++ 标准中一处和本文不符的提法[已被修正](https://github.com/cplusplus/draft/pull/790)。

