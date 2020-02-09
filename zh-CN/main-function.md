# 关于 main 函数的原型和返回值

Created @ 2012-11-07.

# 引言

发现以前说的太零碎，不太好引用……整理一下。

目前我看到的比较靠谱的说法（有正确的引用出处，并指出了实现扩展）：
http://homepage.ntlworld.com/jonathan.deboynepollard/FGA/legality-of-void-main.html

http://tieba.baidu.com/p/626323902

↑而这里的说法是有问题的。

这里再解释一下ISO C/C++中对 `main` 的要求。

# 标准版本说明

基本内容参照[术语和文献列表](terms-and-bibliography.md)。

在本文问题上， ANSI C89 和 C90 、 C99 和 C11 、 C++ 标准各个版本这三组标准之间分别没有实质变化（或根本一模一样），所以只引用最早的标准文本。

# 首先是几个背景知识

本文所讲的实现即语言实现，可以是编译器+链接器等等，可以是解释环境。一般是前者。
关于 implementation-defined 等确切含义可以 Google 。

## ANSI C89支持函数声明省略返回值，隐含为 `int`

也就是说 `main()` 其实是`int main()` ， `foo();` 其实是 `int foo();` 。尤其注意 `main()` **绝不是** `void main()` 。

这在 ISO C99 开始以及 ISO C++ 中是**不允许**的。

## 关于参数列表

C语言的 `(void)` 或函数定义中的 `()` 表示不接受任何参数，相当于 C++ 的 `()` ，也和 C++ 的 `(void)` 等价。

C 语言的 `()` 在函数定义外表示接受任何参数，作用近似 C++ 的 `(...)` 但含义不同（在 C++ 中函数参数列表中的 `...` 对接受参数的类型仍然有限制）。

使用 C 的 `()` 的函数声明主要特殊性在于，此种形式的声明不是原型(prototype) ，若没有其它兼容(compatible) 的原型，则缺少原型声明引入的形式参数和实际参数匹配的检查：

**ISO C11(N1570)**

> **6.5.2.2**/2 If the expression that denotes the called function has a type that includes a prototype, the number of arguments shall agree with the  number of parameters. Each argument shall have a type such that its value may be assigned to an object with the unqualified version of the type of its corresponding parameter.

注意以上检查是属于 **Constraints** 节标题后。这意味着实现在检查失败时给出诊断消息（如警告）：

**ISO C11(N1570)**

> **5.1.1.3**/1 A conforming implementation shall produce at least one diagnostic message (identified in an implementation-defined manner) if a preprocessing translation unit or translation unit contains a violation of any syntax rule or constraint, even if the behavior is also explicitly specified  as  undefined  or  implementation-defined. Diagnostic messages need not be produced in other circumstances.

但是，空参数列表（不是原型中的明确指定参数类型的 `(void)` ）“ `()` ”的使用是过时用法：

**ISO C11(N1570)**

> **6.11.6**/1 The use of function declarators with empty parentheses (not prototype-format parameter type declarators) is an obsolescent feature.

考虑到过时用法容易造成误会，也没有原型声明引入的检查，建议避免使用。

所以在 C 语言中，声明时避免不要省略 `(void)` 中的 `void` ，要是省略就不是预期想要的函数原型了。在定义中可以使用 `()` ，如 `int main(){}` ，同 `int main(void){}` ，但在没有另外的声明中给出原型的情形下仍不建议（尽管把 `()` 看作 `(void)` 在实现上毫无压力，有的实现如 GCC 的确如标准允许地完全无视实际参数不匹配的情形）。若要保证声明和定义通用且/或完全避免过时用法，则应只用 `(void)` 表示函数没有参数。

而 C++ 中，并不存在 C 允许省略原型而不检查函数调用时参数匹配的（过时）特性，不接受任何参数的参数列表写成 `(void)` 是不必要的（虽然也没错，但习惯上的正式写法都没有这种无谓的罗嗦）。

</br>
相关依据：

**ISO C11(N1570)**

> **6.7.6.3**/14 An identifier list declares only the identifiers of the parameters of the function. An empty list in a function declarator that is part of a definition of that function specifies that the function has no parameters. The empty list in a function declarator that is not part of a definition of that function specifies that no information about the number or types of the parameters is supplied.145)

> 145) See "future language directions" (6.11.6).

> # 6.11.6 Function declarators

> 1 The use of function declarators with empty parentheses (not prototype-format parameter type declarators) is an obsolescent feature.

## 实现环境分类

ISO C/C++ 中，根据对环境的要求，分为两类，一类是*独立实现(freestanding implementation)* ，另一类是*宿主实现(hosted implementation)* 。

独立实现对环境的要求比较低，所以更自由。宿主实现——一般可以看作是有操作系统的实现，提供了比较多的底层接口，约束比较多。

当然 C 和 C++ 之间对两者的要求有所不同。为简化问题，除了 `main` 相关的部分在下文讨论以外，不再提及。

## ISO 标准文档中的情态动词的含义

以下全部节录（供参考，只想看结论的可以跳过）。

表格项分隔使用 | ，同一格内不同项使用 / 。

> # ISO/IEC Directives, Part 3 Annex E(normative)

> # Verbal forms for the expression of provisions

> <tt>NOTE</tt> Only singular forms are shown.

> The verbal forms shown in Table E.1 shall be used to indicate requirements strictly to be followed in order to conform to the standard and from which no deviation is permitted.

> ## Table E.1 — Requirement

> Verbal form Equivalent expressions for use in exceptional cases(see 6.6.1.3)

> shall | is to/is required to/it is required that/has to/only … is permitted/it is necessary

> shall not | is not allowed [permitted] [acceptable] [permissible]/is required to be not/is required that … be not/is not to be

> Do not use “must” as an alternative for “shall”. (This will avoid any confusion between the requirements of a standard and external statutory obligations.)

> Do not use “may not” instead of “shall not” to express a prohibition.
To express a direct instruction, for example referring to steps to be taken in a test method, use the imperative mood in English.

> <tt>EXAMPLE</tt> “Switch on the recorder.”

> The verbal forms shown in Table E.2 shall be used to indicate that among several possibilities one is recommended as particularly suitable, without mentioning or excluding others, or that a certain course of action is preferred but not necessarily required, or that (in the negative form) a certain possibility or course of action is deprecated but not prohibited.

> ## Table E.2 — Recommendation

> Verbal form Equivalent expressions for use in exceptional cases(see 6.6.1.3)

> should | it is recommended that/ought to

> should not | it is not recommended that/ought not to

> In French, do not use “devrait” in this context.

> The verbal forms shown in Table E.3 are used to indicate a course of action permissible within the limits of the standard.

> ## Table E.3 — Permission

> Verbal form Equivalent expressions for use in exceptional cases(see 6.6.1.3)

> may | is permitted/is allowed/is permissible

> need not | it is not required that/no … is required

> Do not use “possible” or “impossible” in this context.

> Do not use “can” instead of “may” in this context.

> <tt>NOTE 1</tt> “May” signifies permission expressed by the standard, whereas “can” refers to
the ability of a user of the standard or to a possibility open to him.

> <tt>NOTE 2</tt> The French verb “pouvoir” can indicate both permission and possibility. For clarity, the use of other expressions is advisable if otherwise there is a risk of misunderstanding.

> The verbal forms shown in Table E.4 are used for statements of possibility and capability, whether material, physical or causal.

> ## Table E.4 — Possibility and capability

> Verbal form Equivalent expressions for use in exceptional cases(see 6.6.1.3)

> can | be able to/there is a possibility of/it is possible to

> cannot | be unable to/there is no possibility of/it is not possible to

> <tt>NOTE</tt> See note 1 to Table E.3.

这里最需要注意的有两点：

- shall 的意义，以及区分 shall 和 should 。

- shall 表示要求， should 表示建议。这点举个例子，在 ISO C++ 关于容器的 `size()` 的复杂度上应该能坑到一些人。

ISO C++98/03 在表格的 note 里要求 “should” O(1) ，因此 libstdc++ 的 `std::list::size()` 可以实现为 O(n) 的，类似 `std::distance(l.begin(), l.end())` ，不违反标准要求。

而 ISO C++11 改成了 “shall” O(1) ，这种实现就不符合标准了。

 can 的含义。
 
应该注意 can 表示可能性，而不是要求。和表示准许的may也应该有清楚的区别。

# 正题

附一个参考链接： http://stackoverflow.com/questions/1765686/correctly-declaring-the-main-function-in-ansi-c

## ANSI C/ISO C 对独立环境的规定

ANSI C89 是这样的：

> ### 2.1.2.1 Freestanding environment

> In a freestanding environment (in which C program execution may take place without any benefit of an operating system), the name and type of the function called at program startup are implementation-defined. There are otherwise no reserved external identifiers. Any library facilities available to a freestanding program are implementation-defined.

> The effect of program termination in a freestanding environment is implementation-defined.

可见独立环境中不要求有 `main` 函数存在作为入口函数，更不限定 `main` 的原型。

ISO C99 是这样的：

> ### 5.1.2.1 Freestanding environment

> 1 In a freestanding environment (in which C program execution may take place without any benefit of an operating system), the name and type of the function called at program startup are implementation-defined. Any library facilities available to a freestanding program, other than the minimal set required by clause 4, are implementation-defined.

> 2 The effect of program termination in a freestanding environment is implementation-defined.

没什么变化。

ISO C++98 是这样的：
 
> ## 3.6.1 Main function [basic.start.main]

> 1 A program shall contain a global function called main, which is the designated start of the program. It is
implementation-defined
whether a program in a freestanding environment is required to define a main
function. [Note: in a freestanding environment, startup
and termination is implementation-defined;
startup
contains the execution of constructors for objects of namespace scope with static storage duration; termination
contains the execution of destructors for objects with static storage duration. ]

## 2.2 ANSI C89 对宿主环境的规定

> ### 2.1.2.2 Hosted environment

> A hosted environment need not be provided, but shall conform to the following specifications if present.

> "Program startup"

> The function called at program startup is named main . The implementation declares no prototype for this function. It can be defined with no parameters:

> `int main(void) { /*...*/ }`

> or with two parameters (referred to here as argc and argv , though any names may be used, as they are local to the function in which they are declared):

> `int main(int argc, char *argv[]) { /*...*/ }`


> If they are defined, the parameters to the main function shall obey the following constraints:

> * The value of argc shall be nonnegative.

> * `argv[argc]` shall be a null pointer.

> * If the value of argc is greater than zero, the array members argv[0] through `argv[argc-1]` inclusive shall contain pointers to strings, which are given implementation-defined values by the host environment prior to program startup. The intent is to supply to the program information determined prior to program startup from elsewhere in the hosted environment. If the host environment is not capable of supplying strings with letters in both upper-case and lower-case, the implementation shall ensure that the strings are received in lower-case.

> * If the value of argc is greater than zero, the string pointed to by argv[0] represents the program name ;`argv[0][0]` shall be the null character if the program name is not available from the host environment. If the value of `argc` is greater than one, the strings pointed to by `argv[1]` through `argv[argc-1]` represent the program parameters .

> * The parameters `argc` and `argv` and the strings pointed to by the argv array shall be modifiable by the program, and retain their last-stored values between program startup and program termination.

> ...

对宿主环境可能有的(can) ：有 `main` ，并且 `main` 的原型是这里提到的其中之一。
若使用第二种原型，参数有一定限制(shall) 。 `argc` 必须非负， `argv[argc]` 是空指针。 `argv[0]` ... `argv[argc-1]` 表示程序参数。一般实现中支持使用命令行传递这些参数。

由于这里是 can ，实际上也允许其它原型。

但这种说法显然太隐晦了。

## ISO C99 对宿主环境的规定

> ### 5.1.2.2 Hosted environment

> 1 A hosted environment need not be provided, but shall conform to the following specifications if present.

> #### 5.1.2.2.1 Program startup

> 1 The function called at program startup is named `main`. The implementation declares no prototype for this function. It shall be defined with a return type of int and with no parameters:

> `int main(void) { /* ... */ }`

> or with two parameters (referred to here as argc and argv, though any names may be used, as they are local to the function in which they are declared):

> `int main(int argc, char *argv[]) { /* ... */ }`

> or equivalent;9) or in some other implementation-defined manner.

> 2 If they are declared, the parameters to the main function shall obey the following constraints:

> — The value of `argc` shall be nonnegative.

> — `argv[argc]` shall be a null pointer.

> — If the value of `argc` is greater than zero, the array members `argv[0]` through `argv[argc-1]` inclusive shall contain pointers to strings, which are given implementation-defined values by the host environment prior to program startup. The intent is to supply to the program information determined prior to program startup from elsewhere in the hosted environment. If the host environment is not capable of supplying strings with letters in both uppercase and lowercase, the implementation shall ensure that the strings are received in lowercase.

> — If the value of argc is greater than zero, the string pointed to by `argv[0]` represents the program name; `argv[0][0]` shall be the null character if the
program name is not available from the host environment. If the value of `argc` is greater than one, the strings pointed to by `argv[1]` through `argv[argc-1]` represent the program parameters.

> — The parameters `argc` and `argv` and the strings pointed to by the argv array shall be modifiable by the program, and retain their last-stored values between program startup and program termination.

这里实质的变化是明确要求 ANSI C 中的两种原型必须被宿主实现接受。而 or in some other implementation-defined manner 的妥协可以看成是对 can 的兼容。

## ISO C++98 的规定

> # 3.6 Start and termination [basic.start]

> ## 3.6.1 Main function [basic.start.main]

> 1 A program shall contain a global function called main, which is the designated start of the program. It is implementation-defined whether a program in a freestanding environment is required to define a main function. [Note: in a freestanding environment, startup and termination is implementation-defined; startup contains the execution of constructors for objects of namespace scope with static storage duration; termination contains the execution of destructors for objects with static storage duration. ]

> 2 An implementation shall not predefine the main function. This function shall not be overloaded. It shall have a return type of type int, but otherwise its type is implementation-defined. All implementations shall allow both of the following definitions of main:

> `int main() { /* ... */ }`

> and

> `int main(int argc, char* argv[]) { /* ... */ }`

> In the latter form argc shall be the number of arguments passed to the program from the environment in which the program is run. If argc is nonzero these arguments shall be supplied in `argv[0]` through `argv[argc1]` as pointers to the initial characters of nullterminated multibyte strings (NTMBSs)(17.3.2.1.3.2) and `argv[0]` shall be the pointer to the initial character of a NTMBS that represents the name used to invoke the program or `""`. The value of argc shall be nonnegative. The value of `argv[argc]` shall be 0. [Note: it is recommended that any further (optional) parameters be added after `argv`. ]

> 3 The function main shall not be used (3.2) within a program. The linkage (3.5) of main is implementation-defined. A program that declares main to be inline or static is illformed.
The name main is not otherwise reserved. [Example: member functions, classes, and enumerations can be called main, as can entities in other namespaces. ]

ISO C++ 的规定和 ISO C 类似，但有几点重要的不同：
- a)程序必须包含全局 `main` 。但在独立实现中不一定用到 `main` 作为程序启动的入口，事实上程序启动过程是由实现定义的。
- b)实现不能预定义 `main` （以免用户定义的 `main` 冲突违反 one definition rule 导致错误）。注意这不和上文矛盾。例如，链接器可以在生成可执行二进制映像时决定是否应该添加 `main` 的定义。
- c)main必须返回 `int` 。
- d)全局 `main` 禁止被使用。因此不像 C ， C++ 中 `main` 无法递归调用。 `&::main` 也是错误的。

# 结论

## `main` 的兼容性

别盲目认为哪个是对的哪个是错的，标准没这么简单。

`void main()` 在 C 仍然可以是*符合标准(conforming)* 的扩展，只要有文档——看我一开始给的链接。只不过依赖 implementation-defined 不是 strictly conforming 罢了，可移植性较弱。

在 C++ 中不返回 `int` 的 `main` 直接不符合标准。

## 基于保证可移植性的入口函数使用的建议策略

以下不适用于自己实现语言或者写操作系统之类。

- a)确定实现。如果是独立实现，自己翻实现文档，否则
- b)如无特殊必要，尽量使用标准明文规定的两种形式；
- c)使用其它形式应能找到文档，并且确保当前需求能容忍由此导致的可移植性缺陷。

# 附录

## 标准返回值

ISO C99 起，及 ISO C++98 起，全局 `main` 若没有 `return` ，相当于末尾隐含 `return 0;` 。对于一般实现，返回 0 表示程序执行成功。 C/C++ 标准库宏 `EXIT_SUCCESS` 表示由实现定义的成功返回状态。 `EXIT_SUCCESS` 可用于 `exit` 函数，而 `main` 终止和 `exit` 语义上等价，所以也可以 `return EXIT_SUCCESS;` 代替。

## 关于 Bjarne Stroupstrup 的说法

在个人主页中 Bjarne Stroustrup 明确解答了[关于 `void main` 的问题](http://www.stroustrup.com/bs_faq2.html#void-main) ，表示不赞同使用 `void main` 。其中提到， `void main`

> ... is not and never has been C++, nor has it even been C.

注意下文：

> See the ISO C++ standard 3.6.1[2] or the ISO C standard 5.1.2.2.1.

显然这里的依据是 ISO C 和 ISO C++ 。

不过之后的

> A conforming implementation may provide more versions of main(), but they must all have return type int.

实际上指的仅仅是 ISO C++ ，而不适用于 ISO C 。而之后的

> The int returned by main() is a way for a program to return a value to "the system" that invokes it. On systems that doesn't provide such a facility the return value is ignored, but that doesn't make "void main()" legal C++ or legal C. Even if your compiler accepts "void main()" avoid it, or risk being considered ignorant by C and C++ programmers.

则明确存在一些允许 `void main` 的实现。使用这类实现的 `void main` 的程序是不可移植的，一定不是符合 ISO C 意义下的严格一致(strictly conforming) 的程序，但并不像 ISO C++ 被禁止而使语言实现直接违反一致性(conformance) 。上文中 never has been 也应在这个基础上理解。

