# [科普]变量、全局变量及其它

Created @ 2013-01-28, v2 rev 2013-01-29, markdown @ 2015-09-14.

对最近各种蛋疼的所谓“全局”“变量”问题的解释做个小结。


## 1.变量(variable) 的一般含义

　　“变量”来源于代数学，是数学中最伟大的发明之一。变量是表示可变数学对象的符号(symbol) 。它具有两重含义，一是指在某个上下文中的符号本身；二是这个符号表示的可变的值(value) 。

## 2.程序设计语言中的变量、变量名(variable name) 和作用域(scope)

　　在程序设计语言中也有类似的概念。不同的是，一般需要更加明确地指出一个变量有效的上下文。这个上下文通常是代码中的一段（连续的）区域(region) ，称为作用域(scope) 。只有在作用域中使用的变量是有效的。

　　由于作用域针对的是一段代码区域，所以对于变量的两重含义的约束是不同的。作用域实际只限定变量作为符号本身，也就是字面形式，即变量名(variable name) 。在语言的语法中，通常标识符(identifier) 这一成分就可以作为变量名。当然变量名也可以是更复杂的表达式。

　　而变量的另一种含义，即变量的内容，各种语言可以提供不同的抽象，例如可以直接指定存储，也可以约定使用其它抽象的语义形式如何体现可变的状态。

　　另外，在明确了名称的概念之后，作用域也不只限于变量，对于任何其它具有名称的抽象也适用。作用域能避免相同的名称的冲突，也就是说允许相同的名称在不同的上下文中指称(denotes) 不同的内容。

## 3.限定名称(qualified name)

　　上面提到过使用作用域的一个原因是防止相同名称的冲突。有时作用域限制不太方便，因为需要在整个程序范围内访问的名称都得放在全局作用域中。所以还需要其它的机制。

　　许多语言提供了全局名称的额外组织机制，一个比较通用的手段是允许对标识符增加表示作用域子集的前缀构成限定名称。限定名称更加明确，而对应未被限定的名称则可以适合更多作用域。

　　这种机制可以有不同的表现形式，如C++和C#的命名空间(namespace) ，Ada 和 Java 的包(pakage) 等（ Java 首先把变量放在类中，然后以类作为包的成员）。

## 4.C语言的变量

　　应该指出， K&R 提到了变量，而 ISO C 中没有正式定义这个概念，甚至 ISO C 正式文本中用到的 variable 一词也都不是变量的意思，而是如 variable length array 等。

　　这里的变量可以按传统意义理解，即包括变量名在内，但对于 C 这样的明确支持不同作用域的语言来说，在整个程序范围内变量的同一性(identity) 就成了问题——“变量名一致的变量就是同一个变量”有违整体上的直觉。而要是撇开造成问题的变量名，即专指变量内容，在 C 语言中使用表示存储的对象(object) 就能很好地解释清楚了，没必要用“变量”这个词来增加理解上的困难。不知是不是这个原因，ISO C 才没定义变量的概念。

　　也就是说，对变量的概念存在两种理解，不见得哪种就是对的，而且都没有必要。这种二义性已经使得一些基本问题的讨论变得没有意义，如“变量是不是表达式”。由于讨论语言问题时“变量”可以被更清晰的术语取代，因此，可以回避这个模糊的说法。

## 5.C++语言的变量

　　C++ 中的变量通过对象或不是作为类的非静态成员的引用的声明引入。变量名指称被声明的引用或对象：

**ISO C++11**

> **3**/6 A variable is introduced by the declaration of a reference other than a non-static data member or of an object. The variable’s name denotes the reference or object.

　　由于引用的存在，这里变量的概念并没有被对象等直接取代。

## 6.全局(global)

　　全局是指整个程序的范围。例如，对于运行时来说，全局状态是程序的各个部分都能访问的状态。

　　全局变量(global variable) 是指程序代码中的各个作用域都能访问的变量。但是，下面会看到，C/C++代码无法真正地达到这点。

　　与全局对立的是局部(local) 。与全局变量对立的是局部变量(local variable) 。但是，C/C++程序中，局部变量往往指块作用域(block scope) 中的变量，并不严格对立。因此下文不使用这些概念。

## 7.C语言的作用域和名称空间

　　C语言有且仅有函数作用域(function scope) 、文件作用域(file scope) 、块作用域和函数原型作用域(function prototype scope) 这些作用域：

ISO C11(N1570)

> ## 6.2.1 Scopes of identifiers

> 3 A label name is the only kind of identifier that has function scope. It can be used (in a goto statement) anywhere in the function in which it appears, and is declared implicitly by its syntactic appearance (followed by a : and a statement).

> 4 Every other identifier has scope determined by the placement of its declaration (in a declarator or type specifier). If the declarator or type specifier that declares the identifier appears outside of any block or list of parameters, the identifier has file scope, which terminates at the end of the translation unit. If the declarator or type specifier that declares the identifier appears inside a block or within the list of parameter declarations in a function definition, the identifier has block scope, which terminates at the end of the associated block. If the declarator or type specifier that declares the identifier appears within the list of parameter declarations in a function prototype (not part of a function definition), the identifier has function prototype scope, which terminates at the end of the function declarator. If an identifier designates two different entities in the same name space, the scopes might overlap. If so, the scope of one entity (the inner scope) will end strictly before the scope of the other entity (the outer scope). Within the inner scope, the identifier designates the entity declared in the inner scope; the entity declared in the outer scope is hidden (and not visible) within the inner scope.

　　顺带纠正两点常见误区：

* a)函数体内中声明的名称所在的作用域是块作用域，而不是函数作用域，后者是标号(label) 专有的（这条也适用于C++）。
* b)没有所谓的全局作用域，通常所谓的全局充其量只是在文件作用域而已。

　　不过在C语言中同一作用域其实允许有限地允许不同实体重名。除了限定名称外，C 语言还有一种消除歧义的方式——名称空间(name space) ：

**ISO C11(N1570)**

> ## 6.2.3 Name spaces of identifiers

> 1 If more than one declaration of a particular identifier is visible at any point in a translation unit, the syntactic context disambiguates uses that refer to different entities. Thus, there are separate name spaces for various categories of identifiers, as follows:

> — label names (disambiguated by the syntax of the label declaration and use);

> — the tags of structures, unions, and enumerations (disambiguated by following any32) of the keywords struct, union, or enum);

> — the members of structures or unions; each structure or union has a separate name space for its members (disambiguated by the type of the expression used to access the member via the . or -> operator);

> — all other identifiers, called ordinary identifiers (declared in ordinary declarators or as enumeration constants).

> 32) There is only one name space for tags even though three are possible.

　　C语言没有限定名称，所有的名称都是标识符。ISO C也没有直接说明“名称”的含义，但这里应该是清楚的。（实际上，C语言语法中的identifier在其前身B语言中的对应物就叫 name 。）

## 8.C++语言的作用域：
　　C++的作用域比较多，以下以标准文本的标题排列：

**ISO C++11**

* 3.3.3 Block scope [basic.scope.local]
* 3.3.4 Function prototype scope [basic.scope.proto]
* 3.3.5 Function scope [basic.funscope]
* 3.3.6 Namespace scope [basic.scope.namespace]
* 3.3.7 Class scope [basic.scope.class]
* 3.3.8 Enumeration scope [basic.scope.enum]
* 3.3.9 Template parameter scope [basic.scope.temp]

　　注意到块作用域一节的交叉引用(cross reference) 的标签(label) 中说的是“local”。这是因为 C++98/03 中用词比较混乱，有时候作 block scope 有时候作 local scope（局部作用域），到 C++11 按 ISO C 统一了，但交叉引用在没有整节作废时还是需要保持兼容性，所以没变。

　　C++ 的名称是指标识符或限定标识符的使用：

**ISO C++11**

> **3**/4 A name is a use of an identifier (2.11), operator-function-id (13.5), literal-operator-id (13.5.8), conversion-function-id (12.3.2), or template-id (14.2) that denotes an entity or label (6.6.4, 6.1).

　　关于“全局”，这里需要提一点：

**ISO C++11**

> **3.3.6**/3 The outermost declarative region of a translation unit is also a namespace, called the global namespace. A name declared in the global namespace has global namespace scope (also called global scope). The potential scope of such a name begins at its point of declaration (3.3.2) and ends at the end of the translation unit that is its declarative region. Names with global namespace scope are said to be global name.

　　也就是说 C++ 有“全局（命名空间）作用域”，是命名空间作用域的特例，其中名称为全局名称。可以看出它和C的文件作用域是对应的。

　　（为什么 C 没有真正的“全局”而 C++ 可以有了呢——并不是这个单独决定的。）

## 9.C/C++程序与链接(linkage)

　　一个C/C++程序由一个或多个翻译单元(translation unit) 组成。翻译单元作为源代码可以各自独立地被翻译为目标代码然后链接(linking) 成完整的程序。语法上，（预处理后的、正确的）翻译单元由名称的声明(declaration) 构成。声明引入标识符/名称并确定它们的指称。某个翻译单元中的声明仅在这个翻译单元内有效。合法的程序中使用任意名称之前都需要这个名称在所在翻译单元内（而不是“全局”）的（用户提供的，或者实现预定义的）声明。

　　翻译单元的地位是等价的；不存在“全局”的翻译单元。因此，一般地，C/C++ 程序不存在源码层次上的“全局”的概念：“任意名称”自然也包括变量名，只能依赖于具体翻译单元。这样，不管是 C 还是 C++ ，都无所谓真正意义上的“全局变量”。（考虑到 C90 允许隐式声明，倒是有些“全局”的风格；但这种手法不被当前的 C/C++ 接受。）

　　但语言允许在不同翻译单元中共享程序的全局状态——可以直接通过共享不同翻译单元的实体的指称实现。只要指定不同翻译单元中文件/全局作用域中的某些名称指称相同的对象就可以达到“全局变量”的目的。相应地，也存在和外部无关，只是指称翻译单元内部存在的实体名称。这样的属性称为链接(linkage) 。注意链接是针对名称的，而不是对象等具体实体。

　　很自然地，能以相同名称共享外部实体的名称具有外部链接(external linkage) ，只在翻译单元内部共享指称实体的具有内部链接(interal linkage) ，不共享指称的无链接(no linkeage) 。

　　所谓外部变量(external variable) 就是指变量名具有外部链接的变量。这和作用域没有直接的关系。但由于对象声明默认具有的链接，容易造成混淆。这里仅举两例，不详细展开： a) `extern` 的确切含义； b)`const`对象在 C 和 C++ 中具有不同的链接。

　　这样，可以确定，外部变量和“全局变量”不是一回事。这方面的误解看来还是不少，不知道拜谁所赐了。

## 10.结论：C/C++中“全局变量”的确切含义

　　可见，无论是“全局”还是“变量”，在 C 和 C++ 之间都有一些差距。

　　所谓“全局”，无论在 C/C++ 中，都没有传统的意义。 C 能实现在效果上和“全局变量”类似的只是名称具有外部链接的对象，但硬说“全局”就名不副实了。 C++ 多了全局命名空间这种在源码层次上强制的约定，但要真正能保证实现“全局”，还是得靠链接。

　　C 语言中讨论“全局变量”可以有各种没明确的意义，所以这个混乱的概念还是不用为妙。只有C++中是可以明确的：指全局命名空间作用域中的变量。“文件作用域对象”才是C语言中对应的比较明确的提法。

　　C 的这种混乱的根源除了 ISO C 没有明确一些和传统认知有微妙差异的关键概念外，主要是由于缺乏对通过链接共享实体指称的多翻译单元的语言设计的理解所致。不过说到底，既然是坑，没能力填上就绕过去吧。

