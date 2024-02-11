# [科普]字符串和字符串的长度

Created @ 2012-07-30 01:11, recovered rev 2015-09-15, markdown @ 2015-09-16.

Markdown 注释：本地阅读器可附加下列内联样式替换源码中的对应内容：

```markdown
NTBS <span style="font-variant: small-caps">ntbs</span>
NTCTS <span style="font-variant: small-caps">ntcts</span>
NTMBS <span style="font-variant: small-caps">ntmbs</span>
```

# 首先明确几个概念

　　字符串：形式语言理论研究的基本对象之一，是字符的有限序列。

　　形式语言理论研究的基本对象之一，是字符的有限序列。

　　以下引用中文喂鸡“[字符串](https://zh.wikipedia.org/zh-cn/字符串)”：

> 设∑是叫做字母表的非空有限集合。∑的元素叫做“符号”或“字符”。在∑上的字符串（或字）是来自∑的任何有限序列。例如，如果∑  = {0, 1}，则*0101*是在∑之上的字符串。\
> 字符串的长度是在字符串中字符的数目（序列的长度），它可以是任何非负整数。“空串”是在∑上的唯一的长度为0的字符串，并被指示为ε或λ。

　　注意，这里的长度的概念是足够清晰的。

　　以下引用中文喂鸡“[字符串->字符串数据类型](https://zh.wikipedia.org/zh-cn/字符串#.E5.AD.97.E7.AC.A6.E4.B8.B2.E6.95.B0.E6.8D.AE.E7.B1.BB.E5.9E.8B)”：

> # 字符串长度
>
> 尽管形式字符串可以有任意（但有限）的长度，实际语言的字符串的长度经常被限制到一个人工极大值。一般的说，有两种类型的字符串数据类型:“定长字符串”，它有固定的极大长度并且不管是否达到了这个极大值都使用同样数量的内存；和“变长字符串”，它的长度不是专断固定的并且依赖于实际的大小使用可变数量的内存。在现代编程语言中的多数字符串是变长字符串。尽管叫这个名字，所有变长字符串还是在长度上有个极限，一般的说这个极限只依赖于可获得的内存的数量。\
> ……

# 表示法

　　一种常用的表示法是使用一个字符代码的数组，每个字符通常占用一个字节（如在 ASCII 代码中）或两个字节（如在 UCS-2 这样的 Unicode 表示中）。它的长度可以使用一个结束符（一般是 [NUL](https://zh.wikipedia.org/zh-cn/空字符) ，ASCII 代码是 0 ，在C编程语言中使用这种方法）。或者在前面加入一个整数值来表示它的长度（在 Pascal 语言中使用这种方法）。

【例略】

　　可见字符串的长度和存储的关系是不唯一的。

　　在 C/C++ 中可以使用多种形式表示和存储的字符串。最常见的基本的字符串表示形式（即C标准库/C++标准库都使用的形式）通称为 C 风格字符串(C-style string) ，在 ISO C++ 的学名是 NTCTS(null terminated character string) 。

**ISO C++11**

> ## 17.3.17 [defns.ntcts]
>
> **NTCTS**\
> a sequence of values that have _character type_ that precede the terminating null character type value `charT()`

　　具体说来，一个典型的场景是：多于一个元素的 `char`/`wchar_t`/`char16_t`/`char32_t`/其它实现允许的扩展字符类型的数组可以放一个 NTCTS 。

　　注意，是 a sequence of values 而不是 characters ，表示抽象的含义。下面会看到 character（但不是 multibyte character ）在 C++ 标准库中的明确受限的意义。

　　顺便，关于 multibyte character 是 C++ 整体通用的基本术语之一，所以独立于 character 之外考虑：

**ISO C++11**

> ## 1.3.13 [defns.multibyte]
>
> **multibyte character**\
> sequence of one or more bytes representing a member of the extended character set of either the source or the execution environment\
> [ _Note:_ The extended character set is a superset of the basic character set (2.3)._—end note_ ]

　　（至于字符、基本执行字符集什么的虽然是必要基础但理解起来很简单，暂且不在此展开。）

**ISO C++11**

> ##### 17.5.2.1.4 Character sequences [character.seq]
>
> 1 The C standard library makes widespread use of characters and character sequences that follow a few uniform conventions:\
> — A _letter_ is any of the 26 lowercase or 26 uppercase letters in the basic execution character set.167\
> — The _decimal-point character_ is the (single-byte) character used by functions that convert between a (single-byte) character sequence and a value of one of the floating-point types. It is used in the character sequence to denote the beginning of a fractional part. It is represented in Clauses 18 through 30 and Annex D by a period, ’.’, which is also its value in the `"C"` locale, but may change during program execution by a call to `setlocale(int, const char*)`,168 or by a change to a locale object, as described in Clauses 22.3 and 27.\
> — A _character sequence_ is an array object (8.3.4) _A_ that can be declared as <tt>_T A [N]_</tt>, where _T_ is any of the types `char`, `unsigned char`, or `signed char` (3.9.1), optionally qualified by any combination of const or volatile. The initial elements of the array have defined contents up to and including an element determined by some predicate. A character sequence can be designated by a pointer value _S_ that points to its first element.\
> 167) Note that this definition differs from the definition in ISO C 7.1.1.\
> 168) declared in `<clocale>` (22.6).

　　据此可以定义更具体的 NTBS(null terminated byte string) ：

**ISO C++11**

> ##### 17.5.2.1.4.1 Byte strings [byte.strings]
>
> 1 A _null-terminated byte string_, or NTBS, is a character sequence whose highest-addressed element with defined content has the value zero (the _terminating null_ character); no other element in the sequence has the value zero.169\
> 2 The _length_ of an NTBS is the number of elements that precede the terminating null character. An empty ntbs has a length of zero.\
> 3 The _value_ of an NTBS is the sequence of values of the elements up to and including the terminating null character.\
> 4 A _static_ NTBS is an NTBS with static storage duration.170\
> 169) Many of the objects manipulated by function signatures declared in \<cstring> (21.7) are character sequences or NTBSs. The size of some of these character sequences is limited by a length value, maintained separately from the character sequence.\
> 170) A string literal, such as "abc", is a static ntbs.

　　NTBS 的元素通常用 `char` 类型对象或值表示。

　　NTBS 在 NTCTS 和 character sequence 的基础上明确了存储。此外，NTBS 区分于 NTCTS 的定义的重要目的之一是为了明确（允许变长编码的）多字节字符串 NTMBS 的外延——注意，这里的一些“长度”开始体现出显著的区别。

　　先看定义：

**ISO C++11**

> ##### 17.5.2.1.4.2 Multibyte strings [multibyte.strings]
>
> 1 A _null-terminated multibyte string_, or NTMBS, is an NTBS that constitutes a sequence of valid multibyte characters, beginning and ending in the initial shift state.171\
> 2 A _static_ NTMBS is an NTMBS with static storage duration.\
> 171) An NTBS that contains characters only from the basic execution character set is also an NTMBS. Each multibyte character then consists of a single byte.

　　可见 NTMBS 是 NTBS 的子集，它其中可以包含多个（连续）字节组成的字符。

　　按 17.5.2.1.4.1/2 ，NTMBS 即 NTBS 的长度是其中包含的元素数。这里的“元素”概念和NTBS中有区别，即强调作为 NTMBS 时长度是多字节字符数而不是字符（字节）数。显然对于一般的NTMBS，即便去除结尾的空字符，长度和占用的字节数可以不同。

　　但是，ISO C 里面关于“长度”可以有些关键性的不同。简而言之，ISO C标准库使用的string相当于 NTBS ，类似 NTMBS 的概念中长度仍以字节计：

**ISO C99/C11(N1570)**

> 7.1.1/1 A _string_ is a contiguous sequence of characters terminated by and including the first null character. The term _multibyte string_ is sometimes used instead to emphasize special processing given to multibyte characters contained in the string or to avoid confusion with a wide string. A _pointer to a string_ is a pointer to its initial (lowest addressed) character. The _length of a string_ is the number of bytes preceding the null character and the _value of a string_ is the sequence of the values of the contained characters, in order.

　　至于宽字符串，ISO C 单独处理（当然长度也是），某种意义上从 C 角度来说“宽字符串不是字符串”：

**ISO C99/C11(N1570)**

> ## 7.1.1 Definitions of terms
>
> 4 A _wide string_ is a contiguous sequence of wide characters terminated by and including the first null wide character. A _pointer to a wide string_ is a pointer to its initial (lowest addressed) wide character. The _length of a wide string_ is the number of wide characters preceding the null wide character and the _value of a wide string_ is the sequence of code values of the contained wide characters, in order.

　　这样对于 `strlen`/`wcslen` 这样的接口来说，所求的长度是非常容易理解的——不包含末尾空字符的字符数；乘上 `sizeof(char)`/`sizeof(wchar_t)` 就是对应不包含末尾空字符的字节数（对于char来说自然可以互换）。

　　对于多字节字符串，`strlen` （ `string::size`/`string::length` 也一样，反正都不知道编码）的含义就不那么直观了。从和形式抽象相容的定义来讲，NTMBS是恰当的；而NTBS的“长度”只是在多字节表示下的字节数而非字符数。

　　大概是因为习惯和历史原因，ISO C/C++ 在这里的一致性上做得不太雅观。不过，在未强调编码/字符集前只考虑空字符前的字节数也是可以理解的。

# 字符串字面量

　　在 C/C++ 中，还有一个非常被错误理解的特性：字符串字面量(string literal) 。

　　重点/易错点择要：

## 1.词法形式

　　`"xxx"` 、`L"xxx"` 、`u"xxx"` 等。另外 raw string literal/user defined literal 以及后者造成的 C++03/C++11 关于一个string literal是否是一个 _token_ 的不兼容性等等，这里关系不大，先略过。

## 2.具有静态存储期

## 3.除了 `u` 等起始的 Unicode 字面量，使用的字符集由实现定义（通常由源文件的编码决定）

　　因此事实上除了仅包含基本执行字符集中的元素的字符串字面量，相同的输入并不一定导致相同的结果，长度也是由实现定义的。

## 4.末尾隐含空字符

## 5.类型

　　对于 C ，类型是对应字符类型的数组类型；对于 C++ ，类型是对应字符类型的 `const` 数组类型。

## 6.修改引起未定义行为

## 7.**不等同于字符串**

　　一个字符串字面量不一定表示一个字符串。

　　关于最后一点在 ISO C 里有特别指出：

**ISO C11(N1570)**

> ## 6.4.5 String literals
>
> 6 In translation phase 7, a byte or code of value zero is appended to each multibyte character sequence that results from a string literal or literals.78) ...\
> 78) A string literal need not be a string (see 7.1.1), because a null character may be embedded in it by a `\0` escape sequence.

　　例如，`"123\0\0123"` 不是一个字符串，而是两个连续存储的字符串（注意后面的 `\0` 不是一整个字符而是和后面的字符继续构成八进制转义序列）。

　　除了C风格字符串，还有其它形式。ISO C++ 标准库提供了类模版 `std::basic_string` 。对于 C++03 ，它不一定以 NTCTS 的形式存储；而对于 C++11 ，因为接口的限制，它的实例对象在可观察行为上表现为内部存储一个NTCTS。

　　非标准库的形式也有很多。比如说 M$ 的 `_bstr_t` ；或者自己实现一个类 PASCAL 字符串，等等。

　　对于这类非标准字符串，如果涉及到多字节字符且不明确实现（通常来说对于用户不应该明确实现）的话，通常应当约定“长度”指的是字符数还是字节数。

　　至于数组长度，完全不上道的玩意儿。

　　也就是字符串字面量用来初始化数组约定隐式数组的长，和C风格字符串都没什么直接关系。

# 新手指引

Appended @ 2012-7-30 01:32.

* 明确一般意义形式上的字符串和 C 风格字符串的差异；
* 明确 C 风格字符串和字符串字面量的差异；
* 明确空字符不是字符串的内容；
* 明确一般意义上的字符串长度和字符集/编码相关；
* 明确 C 风格字符串的长度通常只讨论不包括末尾空字符的字节数。

　　剩下基本问题不大了。

# 非标准库字符串进阶参考

Appended @ 2012-7-30 01:39.

* [C++1y 候选的 `string_ref`](http://code.google.com/p/cxx1y-array-string-ref/source) ；
* [LLVM 中的各种字符串表示形式的实现](http://llvm.org/docs/ProgrammersManual.html#string_apis、llvm.org/docs/ProgrammersManual.html#ds_string) 。

