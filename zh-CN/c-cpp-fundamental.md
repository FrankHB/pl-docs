# ISO C/C++ 基础总结

本文档意为避免 C 和 C++ 语言中的一些陷阱的通用指引及撰写相关 [FAQ](http://en.wikipedia.org/wiki/FAQ) 的权威参考的要点注解。

可能会包含程序语言理论或其它语言的具体例子作为参照。

因为涉及内容过多，本文档不保证内容的完整覆盖，且可能会多次修订。

本文档避免原创研究，尽量保证参考文献可查证但不保证所有文献的权威性。

未完成部分以 TBD(to be determined) 标记。正在修订部分以 WIP(work in progress) 标记。

# 0 体例说明

## 0.1 引用文献

仅在此标注权威文献。

正式标准 ISO/IEC 14882:2011 和 ISO/IEC 9899:2011 分别标记为 [C++11] 和 [C11] 。

相关工作组文献，以标准草案为例：[ISO/IEC JTC1 WG21 N3936](https://github.com/cplusplus/draft/blob/master/papers/N3936.pdf?raw=true) 和 [ISO/IEC JTC1 WG14 N1570](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf) ）分别标记为 [WG21/N3936] 和 [WG14/N1570] 。

标记中可包括章节号或章节引用。

引用 ISO C 和 ISO C++ 对应段落的顺序不确定，视文档内容需要而定。

涉及到 ISO C 和 ISO C++ 的差异会特别标示。

引文可能有部分省略。

若无特别说明，保证引用的草案和正式标准中的实质含义一致。

不额外特别标识其它引用。

## 0.2 记法

和 ISO C 以及 ISO C++ 类似，斜体用于表示首次引入的需要读者注意的局部概念 [C++11 1.3/3] 或*语法记法(syntax notation)* [C++11 1.6] 。这些概念或语法标注在引用的参考文献或本文档内可以找到释义。

粗体用于强调。

## 0.3 本文的结构

一级目录基本和 ISO C++ 保持一致。

考虑系统性和内容涵盖，在此以 ISO C++ 而不是 ISO C 作为范本。

# 1 概论

## 1.1 语法和语义

什么称为语法或语义不是 ISO C 或 ISO C++ 的内容。但是，尽管没有明确定义，ISO C和 ISO C++ 都明确地用到了这个概念。作为背景知识，在这里有必要澄清通常的含义。

所谓*语法(syntax)* ，在一般高级语言中指一种描述合规性的“字面上的”构造。其它所有的含义可被归类为语义范畴。

**语法和语义不存在绝对的界限。**区分语法和语义主要来自于人为的设计。这种差异能够使设计和实现一个语言更简单。从设计的角度来讲，明确语言规则的过程可以首先从提取固定的语法规则开始。语法规则的检查——语法分析具有相对成熟的理论和实现，在此分离这能够让设计者集中于后续的目的性更加明确的语义设计。从实现的角度看，首先检查语法指定的规则，如果不符合则可以直接拒绝接受，避免复杂通用的语义检查造成的实现困难和低效；同时也有机会复用*分析器(parser)* 的实现。

对于没有经过设计的语言比如自然语言来说，语法和语义的界限通常并不明确。和其它形式语言一样，它们也可以用*文法(grammar)* 来描述。

对于显式区分语法和语义的语言来说，语法和语义也可以通过指定*形式文法(formal grammar)* 研究。形式文法被具体表述为*记法(notation)* 时本身构成一种*元语言(metalanguage)* ，具有自身的*[元语法(meta syntax)](http://en.wikipedia.org/wiki/Metasyntax)* 。元语法的一个经典的例子是 [BNF](http://en.wikipedia.org/wiki/Backus%E2%80%93Naur_Form) 。

ISO C 和 ISO C++ 使用相似的文法标记。关于体例说明，分别参考 [C11 Clause 6] 和 [C++11 1.6] ；此外，通过 [C11 Annex A] 和 [C++11 Annex A] 总览语法。

此外，ISO C 还对语言规则中使用 Constraints 和 Semantics 为标题进行了分类。

对于实用来说，这里语法和语义的区别重要性，在于**它指定了什么程序是应该正确被接受的**。

## 1.2 程序正确性

ISO C++ 定义了*合式的(well-formed)* 程序以及一些其它重要的术语定义：

**[WG21/N3936]**

> # 1.3 Terms and definitions
>
> ## 1.3.10 [defns.impl.defined]
>
> **implementation-defined behavior**\
> behavior, for a well-formed program construct and correct data, that depends on the implementation and that each implementation documents
>
> ## 1.3.12 [defns.locale.specific]
>
> **locale-specific behavior**\
> behavior that depends on local conventions of nationality, culture, and language that each implementation documents
>
> ## 1.3.24 [defns.undefined]
>
> **undefined behavior**\
> behavior for which this International Standard imposes no requirements\
> [ Note: Undefined behavior may be expected when this International Standard omits any explicit definition of behavior or when a program uses an erroneous construct or erroneous data. Permissible undefined behavior ranges from ignoring the situation completely with unpredictable results, to behaving during translation or program execution in a documented manner characteristic of the environment (with or without the issuance of a diagnostic message), to terminating a translation or execution (with the issuance of a diagnostic message). Many erroneous program constructs do not engender undefined behavior; they are required to be diagnosed. —end note ]
>
> ## 1.3.25 [defns.unspecified]
>
> **unspecified behavior**\
> behavior, for a well-formed program construct and correct data, that depends on the implementation\
> [ Note: The implementation is not required to document which behavior occurs. The range of possible behaviors is usually delineated by this International Standard. —end note ]
>
> ## 1.3.26 [defns.well.formed]
>
> **well-formed program**\
> C++ program constructed according to the syntax rules, diagnosable semantic rules, and the One Definition Rule (3.2).

其中：

* *未定义行为(undefined behavior)* 在 ISO C++11 的意义上理解为“错误的”或“不可移植的”，不保证行为可预测。
* *未指定(unspecified)* 不保证结果唯一，可以是正确的。
* *实现定义(implementation-defined)* 类似未指定，但实现有义务在文档上标注唯一确定的选项。
* *可诊断(diagnosable)* [C++11 Clause 1.4] 详见下文。
* *ODR(One Definition Rule)* [C++11 Clause 3] 独立于一般的语法和语义规则之外，详见下文。

相对地，ISO C 也有类似的术语定义：

**[WG14/N1570]**

> # 3.  Terms, definitions, and symbols
>
> # 3.4
>
> 1 **behavior**\
> external appearance or action
>
> ## 3.4.1
>
> 1 **implementation-defined behavior**\
> unspecified behavior where each implementation documents how the choice is made\
> 2 <tt>EXAMPLE</tt>  An example of implementation-defined behavior is the propagation of the high-order bit when a signed integer is shifted right.
>
> ## 3.4.2
>
> 1 **locale-specific behavior**\
> behavior that depends on local conventions of nationality, culture, and language that each implementation documents\
> 2 <tt>EXAMPLE</tt>  An example of locale-specific behavior is whether theislowerfunction returns true for characters other than the 26 lowercase Latin letters.
>
> ## 3.4.3
>
> 1 **undefined behavior**\
> behavior, upon use of a nonportable or erroneous program construct or of erroneous data, for which this International Standard imposes no requirements\
> 2 <tt>NOTE</tt>  Possible undefined behavior ranges from ignoring the situation completely with unpredictable results, to behaving during translation or program execution in a documented manner characteristic of the environment (with or without the issuance of a diagnostic message), to terminating a translation or execution (with the issuance of a diagnostic message).\
> 3 <tt>EXAMPLE</tt>  An example of undefined behavior is the behavior on integer overflow.
>
> ## 3.4.4
>
> 1 **unspecified behavior**\
> use of an unspecified value, or other behavior where this International Standard provides two or more possibilities and imposes no further requirements on which is chosen in any instance\
> 2 <tt>EXAMPLE</tt>  An example of unspecified behavior is the order in which the arguments to a function are evaluated.

不同主要在于 ISO C++ 的未指定和实现定义是分离的，ISO C 中的实现定义是未指定的特例。

ISO C 没有规定“合式”，而使用以下机制：

> # 4. Conformance
>
> 1 In this International Standard, “shall” is to be interpreted as a requirement on an implementation or on a program; conversely, “shall not” is to be interpreted as a prohibition.\
> 2 If a “shall” or “shall not” requirement that appears outside of a constraint or runtimeconstraint is violated, the behavior is undefined. Undefined behavior is otherwise indicated in this International Standard by the words “undefined behavior” or by the omission of any explicit definition of behavior. There is no difference in emphasis among these three; they all describe “behavior that is undefined”.\
> 3 A program that is correct in all other aspects, operating on correct data, containing unspecified behavior shall be a correct program and act in accordance with 5.1.2.3.\
> 4 The implementation shall not successfully translate a preprocessing translation unit containing a#errorpreprocessing directive unless it is part of a group skipped by conditional inclusion.

## 1.3 诊断消息

最典型的诊断消息是编译错误和警告。

一个实现应当正确的程序。对于错误的程序，为了避免实现复杂，ISO C 和 ISO C++ 允许实现不完全进行语义检查。这里略有差异：ISO C 把这种情况统一为未定义行为，而 ISO C++ 特地增加了 no diagnostics required 条款。因此对于 C++ ，**编译通过的程序，即便排除未定义行为，在语言规则下仍然可能是错的**。

## 1.4 存储模型

## 1.5 对象模型

## 1.5.1 “对象”的正式定义

**[WG14/N1570]**

> # 3.15
>
> 1 **object**\
> region of data storage in the execution environment, the contents of which can represent values\
> 2 <tt>NOTE</tt>  When referenced, an object may be interpreted as having a particular type; see 6.3.2.1.

这 ISO C++ 中基本一致：

**[WG21/N3936]**

> # 1.8 The C++ object model [intro.object]
>
> 1 The constructs in a C++ program create, destroy, refer to, access, and manipulate objects. An *object* is a region of storage. [ *Note:* A function is not an object, regardless of whether or not it occupies storage in the way that objects do. *—end note* ] An object is created by a *definition* (3.1), by a *new-expression* (5.3.4) or by the implementation (12.2) when needed. The properties of an object are determined when the object is created. An object can have a *name* (Clause 3). An object has a *storage duration* (3.7) which influences its *lifetime* (3.8). An object has a *type* (3.9). The term *object type* refers to the type with which the object is created. Some objects are *polymorphic* (10.3); the implementation generates information associated with each such object that makes it possible to determine that object’s type during program execution. For other objects, the interpretation of the values found therein is determined by the type of the *expressions* (Clause 5) used to access them.

在 Java[JLS8] 以及传统的*基于类风格的面向对象(class-based style object-orientation)* 的上下文中，对象则是指“类的实例”，和 ISO C 以及 ISO C++ 都不同。（另外，“实例”的意义在 ISO C++ 中也不一样。）

**TBD**

## 1.6 程序执行

## 1.7 多线程环境

# 2 词法转换

**TBD**

# 3 基本概念

以下“基本概念”指 [C++11 Clause 3] 中讨论的内容。

## 3.1 名称和实体

ISO C++ 引入 *ODR(One Definition Rule)* [C++11 Clause 3] 而特别指定了*实体(entity)* 的概念。

与此相对，*名称(name)* 用于*指称(denote)* 实体，通过*声明(declaration)* 引入。

**推论：名称不是实体。**

*变量(variable)* 的含义也在此基础上被明确。

**[WG21/N3936]**

> # 3 Basic concepts [basic]
>
> 3 An *entity* is a value, object, reference, function, enumerator, type, class member, template, template specialization, namespace, parameter pack, or `this`.\
> 4 A name is a use of an *identifier* (2.11), *operator-function-id* (13.5), *literal-operator-id* (13.5.8), *conversion-function-id* (12.3.2), or *template-id* (14.2) that denotes an entity or *label* (6.6.4, 6.1).\
> 5 Every name that denotes an entity is introduced by a *declaration*. Every name that denotes a label is introduced either by a `goto` statement (6.6.4) or a *labeled-statement* (6.1).\
> 6 A *variable* is introduced by the declaration of a reference other than a non-static data member or of an object. The variable’s name, if any, denotes the reference or object.

**但是，ISO C 没有这些概念。** 在 ISO C 中：

* variable 一词并没有被正式使用为名词表示“变量”的含义。在 *VLA(variable length array)* 中出现的是形容词，意为“变量的”，指不在翻译时确定大小，和这里的概念无关。
* entity 一词被不经正式定义地使用，也没有和 ISO C 类似的含义（考虑 **ISO C 没有 ODR** ）。似乎作者认为是不言自明的。
* name 一词被不经正式定义地使用。ISO C 没有 ISO C++ 那样带记号 :: 的 *qualified-id* [C++11 5.1.1] 和 *operator-function-id* [C++11 13.5] 等语法构造，“标识符”和“名称”基本一致。

## 3.2 声明和定义

首先，预处理 *阶段(phase)* 的 *宏定义(macro definition)* 不属于本章讨论的“定义”的外延。

在 ISO C++ 中，**定义总是声明，反过来不成立**：

**[WG21/N3936]**

> # 3.1 Declarations and definitions [basic.def]
>
> 2 A declaration is a *definition* unless it declares a function without specifying the function’s body (8.4), it contains the `extern` specifier (7.1.1) or a *linkage-specification* 25 (7.5) and neither an *initializer* nor a *function-body* , it declares a static data member in a class definition (9.2, 9.4), it is a class name declaration (9.1), it is an *opaque-enum-declaration* (7.2), it is a *template-parameter* (14.1), it is a *parameter-declaration* (8.3.5) in a function declarator that is not the declarator of a *function-definition* , or it is a `typedef` declaration (7.1.3),\
an *alias-declaration* (7.1.3), a using-declaration (7.3.3), a *static_assert-declaration* (Clause 7), an *attribute-declaration* (Clause 7), an *empty-declaration* (Clause 7), or a *using-directive* (7.3.4).
> 25) Appearing inside the braced-enclosed *declaration-seq* in a *linkage-specification* does not affect whether a declaration is a definition.

ISO C 也有类似的结论，但略有不同：

**[WG14/N1570]**

> # 6.7 Declarations
>
> ## Semantics
>
> 5 A declaration specifies the interpretation and attributes of a set of identifiers. A *definition* of an identifier is a declaration for that identifier that:\
> — for an object, causes storage to be reserved for that object;\
> — for a function, includes the function body;<sup>119)</sup>\
> — for an enumeration constant, is the (only) declaration of the identifier;\
> — for a `typedef` name, is the first (or only) declaration of the identifier.\
> 119) Function definitions have a different syntax, described in 6.9.1.

排除 ISO C++ 不兼容 ISO C 的部分，存在重要的差异：

* **ISO C 的声明和定义都针对标识符。ISO C++ 的声明引入名称（包括声明标识符的情形），而定义针对实体。**
* **ISO C 中的一个声明是否是定义，可能和出现的位置相关。** （例如 *tentative definition* 。）
* `typedef` 声明是可以定义而不总只是声明。

## 3.3 ODR

## 3.4 关于变量

上文已经提及 ISO C++ 中的“变量”的定义，且明确了对应术语在 ISO C 中不存在。

ISO C 避免使用“变量”的原因可能在于含义的不够明确。

**TBD**

与此类似的是，术语“对象”在不同程序语言的规则下也有不同的含义。但是**在 ISO C 和 ISO C++ 中*对象(object)* 的内涵和外延都是清楚的**。

### 3.4.1 “变量”的一般意义

**TBD**

一般地，变量的“可变”指不同的名称可以绑定不同的实体。

但是，近年来的程序语言设计习惯于曲解为变量可以存储不同的“值”。这是片面的说法。事实上，存在“变量”的值不可变的语言，如 Haskell [Haskell]。

**TBD**

不管使用什么定义，可以确定的是，变量总有*变量名(variable's name)* 。并且和数学的传统不同，在程序设计语言中，一般不能把两者任意互相替代。

### 3.4.2 变量和常量

和直觉上不同，在 ISO C++ 的上下文中，变量和常量并不相对：

* ISO C++ 明确定义了什么是变量，但没有定义什么是常量。
* ISO C++中明确涉及常量的术语只有若干*常量表达式(constant-expression)*

常量表达式在实用的意义上指有可能在翻译时即可确定值的表达式，在一般意义上和变量不完全相对。

而在 ISO C 中，却明确有*常量(constant)* 的术语。但是，这只是语法上的分类，相当于 ISO C++ 所说的一部分*字面量(literal)* 。

此外，应该了解，“常量”和下文的 const 限定符是完全两回事，尽管后者在 C++ 中具有常量的目的。

**WIP**

## 3.5 作用域

## 3.6 名称查找

## 3.7 程序和连接

## 3.8 启动与终止

## 3.9 存储期

## 3.10 对象生存期

## 3.11 类型

### 3.11.1 基本类型

ISO C 的基本类型和 ISO C++ 的基本类型不同。

Java 中类似的基本类型称为*原始类型(primitive type)* 。

**TBD**

### 3.11.2 组合类型

### 3.11.3 数组和指针

**WIP**

数组和指针都是组合类型的分类。**数组不是指针，指针不是数组。**

不导致歧义时，数组的值或者数组类型的对象可能称为数组。指针类似。同样，在这个上下文中**数组不是指针，指针不是数组。**

无论何种上下文上面讨论的都是实体。根据名称和实体的推论，**数组名不是数组，指针名不是指针。**

## 3.11.4 函数类型

## 3.11.5 类类型

## 3.11.6 限定符

## 3.12 值类别

B 语言指定了*左值(l-value)* 作为语法上的分类。相对地，非左值为*右值(r-value)* 。

C 语言中，左值指示可能访问的存储。右值即为“表达式的值”（参见 [C11 6.3.2.1] 的注释）。除了在特定的一些上下文中，左值会转换为值。

**WIP**

C++03 规定，表达式不是左值就是右值。

C++11 在引入右值引用类型后引入了新的分类，称为*值类别(value category)* 。左值被扩充为泛左值(glvalue) ，包括原来的左值和新的消亡值(xvalue) ；右值的概念被扩充为包括消亡值和原来的右值，后者称为纯右值(prvalue) 。消亡值同时是泛左值和右值。左值和右值的集合不相交仍然没有改变。此外，一个表达式或者是泛左值，或者是纯右值。

引入消亡值的动机是为了解决涉及右值引用的场合下的表达式的分类。因为这种情况下表达式兼有左值的性质，不适合使用传统的右值，却也不能作为传统的左值。问题具体体现在以下方面([WG21/N3055]) ：

* 右值表示对象本身，而右值引用对应存储中的对象。
* 右值的类型必须是完整的，同时静态类型和动态类型总是一致；右值引用需要有多态性，允许静态类型和动态类型不一致。
* 非类类型的右值不被 cv-qualifier 限定。绑定到 `const` 或 `volatile` 限定对象的右值引用必须保留限定符。
* 右值引用和左值引用一样能绑定到函数。若把返回右值引用的函数调用作为右值，则需要引入新的函数右值，需要较多的改动。

**WIP**

# 4 标准转换

## 4.1 左值到右值转换

## 4.2 数组到指针转换

数组类型的左值可以转换为指针类型的右值。

## 4.3 函数到指针转换

## 4.4 限定符转换

## 4.5 浮点转换

## 4.6 整数提升

## 4.7 通常算术转换

## 4.8 bool 相关转换

## 4.9 转换的阶

# 5 表达式

# 6 语句

# 7 声明

# 8 声明符

# 9 类

# 14 模板

