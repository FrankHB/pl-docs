# 《与C++相关的一些术语的翻译和问题》评注

Created @ 2012-12-28, markdown @ 2014-11-07.

引用原文： http://www.is.pku.edu.cn/~qzy/books/cppl/words.htm 。

引用著作：

裘宗燕译，《C++程序设计语言》，机械工业出版社， 2002.7 。

　　原文中讨论若干术语的翻译的观点，包括一些较不流行的译法，可能增加对读者C++理解的困难。遂于此说明，并附个人观点供讨论。

# 1.关于“侵入式”（intrusive）和“非侵入式”设计 

　　目前来看已经可以算是主流术语了，尤其是对于原文讨论的引用计数和容器等。

　　其它可选翻译如boost-zh-doc的“介入式”。

## 2.1 `public` / `private`

　　有理。所有权(ownership) 目前也是资源管理惯用法中比较明确的概念，而不应对访问权限使用“用”与之混淆。

　　不过适当保留关键字不译更符合语言习惯。

## 2.2 `friend`

　　“友元”目前已经是通用译法。

## 2.3 constructor

　　可以接受，但并非关键问题。最大的问题是，所谓“构造函数”其实是 constructor 的子集—— constructor 还包括 constructor template （构造模版）。因此像某些 Java 专著那样翻译成“构造器”可能更好点。

PS： default/copy/move constructor 这些特殊成员倒真是函数。ISO C++ 明确强调了 non-template ，第 12 章的标题就是“special member function”。

PS2： Java 里面 constructor 不算 method 。为了不跑题过远， constructor 的语义的其它差异暂时略过。

## 2.4 `inline`

　　inline 翻译成“在线”看来并不是什么好主意，会被容易误解为“online 的错译”——有这种实例。此外，这里表达的意义并不充分， `inline` 除了可能改变代码生成外，更重要的、决定性的一点是关于 One Definition Rule 的特殊性，跟非 `inline` 修饰的函数不同。

　　基于这点，适当保留关键字不译更合理。

## 2.5 garbage collection

　　“废料收集”更合理。

## 2.6 smart pointer

　　合理，但需要改变习惯。

## 3.1 bind

　　“约束”通常作为 constraint 的翻译，因此不是很合适。

## 3.2 override

　　“覆盖”比“重写”更好。后者实质上更强调工程行为，描述的是使用语言的操作而不是任何层次上的语义。

