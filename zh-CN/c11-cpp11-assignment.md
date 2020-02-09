# C11 & C++11 的赋值相关表达式求值

Created @ 2013-01-09, markdown @ 2014-11-09.

　　**警告：本文需要有入门级的语言常识和一定的语言理论基础。**

# 1.关于求值(evaluation)

　　C99/C++03 虽然正式地使用到这个词，但没有明确具体的外延。

　　C11/C++11 则把 evaluation 作为局部术语给出了明确了的定义：

**ISO C11(N1570)**

> ### 5.1.2.3
> 2 Accessing a volatile object, modifying an object, modifying a file, or calling a function that does any of those operations are all side effects,12) which are changes in the state of the execution environment. Evaluation of an expression in general includes both value computations and initiation of side effects. Value computation for an lvalue expression includes determining the identity of the designated object.

**ISO C++11**

> # 1.9
> 12 Accessing an object designated by a volatile glvalue (3.10), modifying an object, calling a library I/O function, or calling a function that does any of those operations are all side effects, which are changes in the state of the execution environment. Evaluation of an expression (or a sub-expression) in general includes both value computations (including determining the identity of an object for glvalue evaluation and fetching a value previously assigned to an object for prvalue evaluation) and initiation of side effects. When a call to a library I/O function returns or an access to a volatile object is evaluated the side effect is considered complete, even though some external actions implied by the call (such as the I/O itself) or by the volatile access may not have completed yet.

　　即求值在一般意义上包括值的计算(value computations)和副作用的产生(initiation of side effects)。其中左值（C++中glvalue同之前的左值lvalue）的求值包含确定左值所指示的对象的同一性。

# 2.可观察行为(observable behavior)

　　上面有一点需要特别注意： `volatile` 对象的读取也是副作用。 `volatile` 的根本含义根本体现在对可观察行为的影响，而可观察行为则约定抽象语义和现实实现中的程序语义的联系（这事实上决定了一个符合标准的实现最多能通过修改程序语义优化到的界限）：

**ISO C11(N1570)**

> 1 The semantic descriptions in this International Standard describe the behavior of an abstract machine in which issues of optimization are irrelevant.

> 6 The least requirements on a conforming implementation are:

> — Accesses to volatile objects are evaluated strictly according to the rules of the abstract machine.

> — At program termination, all data written into files shall be identical to the result that execution of the program according to the abstract semantics would have produced.

> — The input and output dynamics of interactive devices shall take place as specified in 7.21.3. The intent of these requirements is that unbuffered or line-buffered output
appear as soon as possible, to ensure that prompting messages actually appear prior to a program waiting for input. This is the observable behavior of the program.

**ISO C++11**

> # 1.9

> 1 The semantic descriptions in this International Standard define a parameterized nondeterministic abstract machine. This International Standard places no requirement on the structure of conforming implementations. In particular, they need not copy or emulate the structure of the abstract machine. Rather, conforming implementations are required to emulate (only) the observable behavior of the abstract machine as explained below.5

> 5) This provision is sometimes called the “as-if” rule, because an implementation is free to disregard any requirement of this International Standard as long as the result is as if the requirement had been obeyed, as far as can be determined from the observable behavior of the program. For instance, an actual implementation need not evaluate part of an expression if it can deduce that its value is not used and that no side effects affecting the observable behavior of the program are produced.

> 8 The least requirements on a conforming implementation are:

> — Access to volatile objects are evaluated strictly according to the rules of the abstract machine.

> — At program termination, all data written into files shall be identical to one of the possible results that execution of the program according to the abstract semantics would have produced.

> — The input and output dynamics of interactive devices shall take place in such a fashion that prompting output is actually delivered before a program waits for input. What constitutes an interactive device is implementation-defined.

> These collectively are referred to as the observable behavior of the program. [ Note: More stringent correspondences between abstract and actual semantics may be defined by each implementation. —end note ]

# 3.求值的顺序

　　和 Java 、 C# 等不同，对 C 和 C++ 来说，表达式的求值顺序和字面上（从左到右）没有一致对应关系，也就是说优先级、结合性等纯粹是语法上的现象，只决定了哪些表达式是哪个操作符的操作数，不决定求值顺序的语义。

　　求值的顺序由附加的规则指定。例如， C 和 C++ 的函数调用符合[严格求值](https://zh.wikipedia.org/zh-cn/求值策略#.E4.B8.A5.E6.A0.BC.E6.B1.82.E5.80.BC_.28Strict_evaluation.29)，保证函数实际参数在初始化形式参数之前求值，且不同函数调用之间的不交错(interleave) 执行；其它部分操作符的附加的语义规则，是单独另行定义的。

　　**TODO** 需要澄清 C++ 17 已经通过了更多附加规则指定顺序。一些问题更加复杂，但本文的整体结论不变。

　　原则上若不能决定能得到可预测的结果的，则规定为*未定义行为(undefined behavior)* ，对结果不做出任何保证和要求。这在 C99 和 C++03 都有一定的体现，都用到了*序列点(sequence point)* 的概念：

**ISO C99**

> ### 5.1.2.3

> 2 Accessing a volatile object, modifying an object, modifying a file, or calling a function that does any of those operations are all side effects,11) which are changes in the state of the execution environment. Evaluation of an expression may produce side effects. At certain specified points in the execution sequence called sequence points, all side effects of previous evaluations shall be complete and no side effects of subsequent evaluations shall have taken place. (A summary of the sequence points is given in annex C.)

> # 6.5

> 2 Between the previous and next sequence point an object shall have its stored value modified at most once by the evaluation of an expression. Furthermore, the prior value shall be read only to determine the value to be stored.70)

> 70) This paragraph renders undefined statement expressions such as

`i = ++i + 1;`

`a[i++] = i;`

while allowing

`i = i + 1;`

`a[i] = i;`

**ISO C++03**

> # 1.9

> 7 Accessing an object designated by a volatile lvalue (3.10), modifying an object, calling a library I/O function, or calling a function that does any of those operations are all side effects, which are changes in the state of the execution environment. Evaluation of an expression might produce side effects. At certain specified points in the execution sequence called sequence points, all side effects of previous evaluations shall be complete and no side effects of subsequent evaluations shall have taken place.7)

> 7) Note that some aspects of sequencing in the abstract machine are unspecified; the preceding restriction upon side effects applies to that particular execution sequence in which the actual code is generated. Also note that when a call to a library I/O function returns, the side effect is considered complete, even though some external actions implied by the call (such as the I/O itself) may not have completed yet.

　　而在C++11中，根据提案WG21/N1944，序列点被先序(sequenced before)二元关系等术语取代。WG21/N2052在此基础上（另一个基础是约定内存模型的WG21/N2052）阐明了并发的内存模型——关于适合多线程环境下的特定并发语义，本文从略。
　　尽管ISO C看起来起先并不热心，最后还是接受了这个修正，但在ISO C11（至少是N1570）中保留了序列点的说法。（另外ISO C++11似乎遗漏了sequenced after，尽管从下文可以看出，在赋值的定义中这个词组正式地被使用。）

**WG21/N1944**

> According to my best guesses, there are at least two factors which have inhibited progress in the C committee:

>> WG14 seems collectively to believe that the problem is not urgent or compelling; that most implementers and experts understand the situation well enough to agree on the essentials of the requirements, and that most of the confusion can be treated as an education problem (not within the committee's scope).

>> Most of the proposals to clarify matters are so radical that they are (rightly) met with considerable suspicion, and (accurately) judged to require much deep consideration to ensure that they don't constitute an unacceptable change to the status quo.

**ISO C11(N1570)**

> ### 5.1.2.3

> 3 Sequenced before is an asymmetric, transitive, pair-wise relation between evaluations executed by a single thread, which induces a partial order among those evaluations. Given any two evaluations A and B, if A is sequenced before B, then the execution of A shall precede the execution of B. (Conversely, if A is sequenced before B, then B is sequenced after A.) If A is not sequenced before or after B, then A and B are unsequenced. Evaluations A and B are indeterminately sequenced when A is sequenced either before or after B, but it is unspecified which.13) The presence of a sequence point between the evaluation of expressions A and B implies that every value computation and side effect associated with A is sequenced before every value computation and side effect associated with B. (A summary of the sequence points is given in annex C.)

> 13) The executions of unsequenced evaluations can interleave. Indeterminately sequenced evaluations cannot interleave, but can be executed in any order.

**ISO C++11**

> 13 Sequenced before is an asymmetric, transitive, pair-wise relation between evaluations executed by a single thread (1.10), which induces a partial order among those evaluations. Given any two evaluations A and B, if A is sequenced before B, then the execution of A shall precede the execution of B. If A is not sequenced before B and B is not sequenced before A, then A and B are unsequenced. [ Note: The execution of unsequenced evaluations can overlap. —end note ] Evaluations A and B are indeterminately sequenced when either A is sequenced before B or B is sequenced before A, but it is unspecified which. [ Note: Indeterminately sequenced evaluations cannot overlap, but either could be executed first. —end note ]

　　同时，限定求值顺序的条款也有大幅度的修改：

**ISO C11(N1570)**

> # 6.5

> 1 An expression is a sequence of operators and operands that specifies computation of a value, or that designates an object or a function, or that generates side effects, or that performs a combination thereof. The value computations of the operands of an operator are sequenced before the value computation of the result of the operator.

> 2 If a side effect on a scalar object is unsequenced relative to either a different side effect on the same scalar object or a value computation using the value of the same scalar object, the behavior is undefined. If there are multiple allowable orderings of the subexpressions of an expression, the behavior is undefined if such an unsequenced side effect occurs in any of the orderings.84)

> 84) This paragraph renders undefined statement expressions such as

`i = ++i + 1;`

`a[i++] = i;`

while allowing

`i = i + 1;`

`a[i] = i;`

**ISO C++11**

> 14 Every value computation and side effect associated with a full-expression is sequenced before every value computation and side effect associated with the next full-expression to be evaluated.8.

> 15 Except where noted, evaluations of operands of individual operators and of subexpressions of individual expressions are unsequenced. [ Note: In an expression that is evaluated more than once during the execution
of a program, unsequenced and indeterminately sequenced evaluations of its subexpressions need not be performed consistently in different evaluations. —end note ] The value computations of the operands of an operator are sequenced before the value computation of the result of the operator. If a side effect on a scalar
object is unsequenced relative to either anotherside effect on the same scalar object or a value computation using the value of the same scalar object, the behavior is undefined.
[ Example:
```
void f(int, int);
void g(int i, int* v) {
i = v[i++]; // the behavior is undefined
i = 7, i++, i++; // i becomes 9
i = i++ + 1; // the behavior is undefined
i = i + 1; // the value of i is incremented
f(i = -1, i = -1); // the behavior is undefined
}
```
—end example ]

> When calling a function (whether or not the function is inline), every value computation and side effect associated with any argument expression, or with the postfix expression designating the called function, is sequenced before execution of every expression or statement in the body of the called function. [ Note: Value computations and side effects associated with different argument expressions are unsequenced. —end note ] Every evaluation in the calling function (including other function calls) that is not otherwise specifically sequenced before or after the execution of the body of the called function is indeterminately sequenced with respect to the execution of the called function.9 Several contexts in C++ cause evaluation of a function call, even though no corresponding function call syntax appears in the translation unit. [ Example: Evaluation of a new expression invokes one or more allocation and constructor functions; see 5.3.4. For another example, invocation of a conversion function (12.3.2) can arise in contexts in which no function call syntax appears. —end example ] The sequencing constraints on the execution of the called function (as described above) are features of the function calls as evaluated, whatever the syntax of the expression that calls the function might be.

> 9) In other words, function executions do not interleave with each other.

　　注意到 ISO C++11 的篇幅大了很多，是因为它把原先不少 ISO C++03 属于第 5 章的内容提前到第 1 章来讲了。可见这里的普遍性和重要性。

　　这里的修改最大的整体差异是对于求值中各个过程的粒度的细化。序列点的描述把值的计算和副作用之间的顺序捆绑在一起，而先序关系可以更清晰地分别描述这两类需要明确顺序的过程之间的逻辑上保持的顺序关系（至于对于整个执行环境来说的顺序，由于要考虑并发，由 *happens before* 二元关系等精确描述，这里从略）。

# 4.ISO C11/ISO C++11的修正

　　接下来需要了解的就是各个具体的表达式的求值顺序，这里从略。

　　重点是 ISO C11/ISO C++11 对 ISO C99/ISO C++03 做了语义上的修订，在明确求值顺序规则的依赖的基础定义（先序关系）的同时，放宽了部分条件，导致原来存在未定义行为的表达式现在可以是正确的。
　　首先的例子是 WG21 Core Defect Report 637 ，关于上面ISO C++11最终使用的例子。

> ## 637. Sequencing rules and example disagree
> ### Section: 1.9  [intro.execution]     Status: CD1     Submitter: Ofer Porat     Date: 2 June 2007

> [Voted into the WP at the September, 2008 meeting.]

> In 1.9 [intro.execution] paragraph 16, the following expression is still listed as an example of undefined behavior:

>     i = ++i + 1;

> However, it appears that the new sequencing rules make this expression well-defined:

>> The assignment side-effect is required to be sequenced after the value computations of both its LHS and RHS (5.17 [expr.ass] paragraph 1).

>> The LHS (i) is an lvalue, so its value computation involves computing the address of i.

>> In order to value-compute the RHS (++i + 1), it is necessary to first value-compute the lvalue expression ++i and then do an lvalue-to-rvalue conversion on the result. This guarantees that the incrementation side-effect is sequenced before the computation of the addition operation, which in turn is sequenced before the assignment side effect. In other words, it yields a well-defined order and final value for this expression.

> It should be noted that a similar expression

>     i = i++ + 1;

> is still not well-defined, since the incrementation side-effect remains unsequenced with respect to the assignment side-effect.

> It's unclear whether making the expression in the example well-defined was intentional or just a coincidental byproduct of the new sequencing rules. In either case either the example should be fixed, or the rules should be changed.

> Clark Nelson: In my opinion, the poster's argument is perfectly correct. The rules adopted reflect the CWG's desired outcome for issue 222. At the Portland meeting, I presented (and still sympathize with) Tom Plum's case that these rules go a little too far in nailing down required behavior; this is a consequence of that.

> One way or another, a change needs to be made, and I think we should seriously consider weakening the resolution of issue 222 to keep this example as having undefined behavior. This could be done fairly simply by having the sequencing requirements for an assignment expression depend on whether it appears in an lvalue context.

> James Widman: How's this for a possible re-wording?

>> In all cases, the side effect of the assignment expression is sequenced after the value computations of the right and left operands. Furthermore, if the assignment expression appears in a context where an lvalue is required, the side effect of the assignment expression is sequenced before its value computation. 

> **Notes from the February, 2008 meeting:**

> There was no real support in the CWG for weakening the resolution of issue 222 and returning the example to having undefined behavior. No one knew of an implementation that doesn't already do the (newly) right thing for such an example, so there was little motivation to go out of our way to increase the domain of undefined behavior. So the proposed resolution is to change the example to one that definitely does have undependable behavior in existing practice, and undefined behavior under the new rules.

> Also, the new formulation of the sequencing rules approved in Oxford contained the wording that by and large resolved issue 222, so with the resolution of this issue, we can also close issue 222.

> **Proposed resolution (March, 2008):**

> Change the example in 1.9 [intro.execution] paragraph 16 as follows:

>     i = v[i++];             // the behavior is undefined
    i = 7, i++, i++;        // i becomes 9
    i = 【去掉++i】 i++ + 1;        // the behavior is undefined
    i = i + 1;              // the value of i is incremented

　　这里很明显地，认为 `i = ++i + 1` 不再存在求值顺序不确定的未定义行为。原因是 `++i` 依赖的 += 的求值顺序有修改。

　　这个修改的理由同样由 Core Defect Report 提出：

> ## 222. Sequence points and lvalue-returning operators
> ### Section: 5  [expr]     Status: CD1     Submitter: Andrew Koenig     Date: 20 Dec 1999

> [Voted into the WP at the September, 2008 meeting.]

> I believe that the committee has neglected to take into account one of the differences between C and C++ when defining sequence points. As an example, consider

>     (a += b) += c;

> where `a`, `b`, and `c` all have type `int`. I believe that this expression has undefined behavior, even though it is well-formed. It is not well-formed in C, because `+=` returns an rvalue there. The reason for the undefined behavior is that it modifies the value of `a' twice between sequence points.

> Expressions such as this one are sometimes genuinely useful. Of course, we could write this particular example as

>     a += b; a += c;

> but what about

>     void scale(double* p, int n, double x, double y) {
        for (int i = 0; i < n; ++i) {
            (p[i] *= x) += y;
        }
    }

> All of the potential rewrites involve multiply-evaluating p[i] or unobvious circumlocations like creating references to the array element.

> One way to deal with this issue would be to include built-in operators in the rule that puts a sequence point between evaluating a function's arguments and evaluating the function itself. However, that might be overkill: I see no reason to require that in

>     x[i++] = y;

> the contents of \``i`' must be incremented before the assignment.

> A less stringent alternative might be to say that when a built-in operator yields an lvalue, the implementation shall not subsequently change the value of that object as a consequence of that operator.

> I find it hard to imagine an implementation that does not do this already. Am I wrong? Is there any implementation out there that does not `do the right thing' already for (a += b) += c?

> 5.17 [expr.ass] paragraph 1 says,

>>    The result of the assignment operation is the value stored in the left operand after the assignment has taken place; the result is an lvalue. 

> What is the normative effect of the words "after the assignment has taken place"? I think that phrase ought to mean that in addition to whatever constraints the rules about sequence points might impose on the implementation, assignment operators on built-in types have the additional constraint that they must store the left-hand side's new value before returning a reference to that object as their result.

> One could argue that as the C++ standard currently stands, the effect of `x = y = 0;` is undefined. The reason is that it both fetches and stores the value of `y`, and does not fetch the value of y in order to compute its new value.

> I'm suggesting that the phrase "after the assignment has taken place" should be read as constraining the implementation to set `y` to `0` before yielding the value of `y` as the result of the subexpression `y = 0`.

> Note that this suggestion is different from asking that there be a sequence point after evaluation of an assignment. In particular, I am not suggesting that an order constraint be imposed on any side effects other than the assignment itself.

> Francis Glassborow:

> My understanding is that for a single variable:

> 　　Multiple read accesses without a write are OK

> 　　A single read access followed by a single write (of a value dependant on the read, so that the read MUST happen first) is OK

> 　　A write followed by an actual read is undefined behaviour

> 　　Multiple writes have undefined behaviour

> It is the 3) that is often ignored because in practice the compiler hardly ever codes for the read because it already has that value but in complicated evaluations with a shortage of registers, that is not always the case. Without getting too close to the hardware, I think we both know that a read too close to a write can be problematical on some hardware.

> So, in `x = y = 0;`, the implementation must NOT fetch a value from `y`, instead it has to "know" what that value will be (easy because it has just computed that in order to know what it must, at some time, store in `y`). From this I deduce that computing the lvalue (to know where to store) and the rvalue to know what is stored are two entirely independent actions that can occur in any order commensurate with the overall requirements that both operands for an operator be evaluated before the operator is.

> Erwin Unruh:

> C distinguishes between the resulting value of an assignment and putting the value in store. So in C a compiler might implement the statement `x=y=0;` either as `x=0;y=0;` or as `y=0;x=0;` In C the statement `(x += 5) += 7;` is not allowed because the first += yields an rvalue which is not allowed as left operand to `+=`. So in C an assignment is not a sequence of write/read because the result is not really "read".

> In C++ we decided to make the result of assignment an lvalue. In this case we do not have the option to specify the "value" of the result. That is just the variable itself (or its address in a different view). So in C++, strictly speaking, the statement `x=y=0;` must be implemented as `y=0;x=y;` which makes a big difference if `y` is declared volatile.

> Furthermore, I think undefined behaviour should not be the result of a single mentioning of a variable within an expression. So the statement `(x +=5) += 7;` should NOT have undefined behaviour.

> In my view the semantics could be:

> 　　if the result of an assignment is used as an rvalue, its value is that of the variable after assignment. The actual store takes place before the next sequence point, but may be before the value is used. This is consistent with C usage.

> 　　if the result of an assignment is used as an lvalue to store another value, then the new value will be stored in the variable before the next sequence point. It is unspecified whether the first assigned value is stored intermediately.

> 　　if the result of an assignment is used as an lvalue to take an address, that address is given (it doesn't change). The actual store of the new value takes place before the next sequence point.

> Jerry Schwarz:

> My recollection is different from Erwin's. I am confident that the intention when we decided to make assignments lvalues was not to change the semantics of evaluation of assignments. The semantics was supposed to remain the same as C's.

> Ervin seems to assume that because assignments are lvalues, an assignment's value must be determined by a read of the location. But that was definitely not our intention. As he notes this has a significant impact on the semantics of assignment to a volatile variable. If Erwin's interpretation were correct we would have no way to write a volatile variable without also reading it.

> Lawrence Crowl:

> For `x=y=0`, lvalue semantics implies an lvalue to rvalue conversion on the result of y=0, which in turn implies a read. If `y` is volatile, lvalue semantics implies both a read and a write on `y`.

> The standard apparently doesn't state whether there is a value dependence of the lvalue result on the completion of the assignment. Such a statement in the standard would solve the non-volatile C compatibility issue, and would be consistent with a user-implemented `operator=`.

> Another possible approach is to state that primitive assignment operators have two results, an lvalue and a corresponding "after-store" rvalue. The rvalue result would be used when an rvalue is required, while the lvalue result would be used when an lvalue is required. However, this semantics is unsupportable for user-defined assignment operators, or at least inconsistent with all implementations that I know of. I would not enjoy trying to write such two-faced semantics.

> Erwin Unruh:

> The intent was for assignments to behave the same as in C. Unfortunately the change of the result to lvalue did not keep that. An "lvalue of type int" has no "`int`" value! So there is a difference between intent and the standard's wording.

> So we have one of several choices:

> 　　live with the incompatibility (and the problems it has for volatile variables)

> 　　make the result of assignment an rvalue (only builtin-assignment, maybe only for builtin types), which makes some presently valid programs invalid

> 　　introduce "two-face semantics" for builtin assignments, and clarify the sequence problematics

> 　　make a special rule for assignment to a volatile lvalue of builtin type

> I think the last one has the least impact on existing programs, but it is an ugly solution.

> Andrew Koenig:

> Whatever we may have intended, I do not think that there is any clean way of making

>     volatile int v;
    int i;

>     i = v = 42;

> have the same semantics in C++ as it does in C. Like it or not, the subexpression `v = 42` has the type ``reference to volatile int,'' so if this statement has any meaning at all, the meaning must be to store 42 in v and then fetch the value of v to assign it to i.

> Indeed, if v is volatile, I cannot imagine a conscientious programmer writing a statement such as this one. Instead, I would expect to see

>    v = 42;
    i = v;

> if the intent is to store 42 in `v` and then fetch the (possibly changed) value of `v`, or

>     v = 42;
    i = 42;

> if the intent is to store `42` in both `v` and `i`.

> What I do want is to ensure that expressions such as ```i = v = 42`'' have well-defined semantics, as well as expressions such as `(i = v) = 42` or, more realistically, `(i += v) += 42` .

> I wonder if the following resolution is sufficient:

> Append to 5.17 [expr.ass] paragraph 1:

>> There is a sequence point between assigning the new value to the left operand and yielding the result of the assignment expression. 

> I believe that this proposal achieves my desired effect of not constraining when j is incremented in x[j++] = y, because I don't think there is a constraint on the relative order of incrementing j and executing the assignment. However, I do think it allows expressions such as (i += v) += 42, although with different semantics from C if v is volatile.

> **Notes on 10/01 meeting:**

> There was agreement that adding a sequence point is probably the right solution.

> **Notes from the 4/02 meeting:**

> The working group reaffirmed the sequence-point solution, but we will look for any counter-examples where efficiency would be harmed.

> For drafting, we note that `++x` is defined in 5.3.2 [expr.pre.incr] as equivalent to `x+=1` and is therefore affected by this change. `x++` is not affected. Also, we should update any list of all sequence points.

> **Notes from October 2004 meeting:**

> Discussion centered around whether a sequence point “between assigning the new value to the left operand and yielding the result of the expression” would require completion of all side effects of the operand expressions before the value of the assignment expression was used in another expression. The consensus opinion was that it would, that this is the definition of a sequence point. Jason Merrill pointed out that adding a sequence point after the assignment is essentially the same as rewriting

>     b += a

> as

>     b += a, b

> Clark Nelson expressed a desire for something like a “weak” sequence point that would force the assignment to occur but that would leave the side effects of the operands unconstrained. In support of this position, he cited the following expression:

>     j = (i = j++)

> With the proposed addition of a full sequence point after the assignment to i, the net effect is no change to j. However, both g++ and MSVC++ behave differently: if the previous value of j is 5, the value of the expression is 5 but j gets the value 6.

> Clark Nelson will investigate alternative approaches and report back to the working group.

> **Proposed resolution (March, 2008):**

> See issue 637.

　　最大的问题出在赋值上。

# 5.赋值中的求值顺序

　　对于赋值的修订的要义可以直接通过对照简单赋值的标准条款的差异看出来。复合赋值的等价展开形式和求值一次的规则并没有变化。

**ISO C99**

> ## 6.5.16

> 3 An assignment operator stores a value in the object designated by the left operand. An assignment expression has the value of the left operand after the assignment, but is not an lvalue. The type of an assignment expression is the type of the left operand unless the left operand has qualified type, in which case it is the unqualified version of the type of the left operand. The side effect of updating the stored value of the left operand shall occur between the previous and the next sequence point.

> 4 The order of evaluation of the operands is unspecified. If an attempt is made to modify the result of an assignment operator or to access it after the next sequence point, the behavior is undefined.

**ISO C++03**

> # 5.17

> 1 There are several assignment operators, all of which group right-to-left. All require a modifiable lvalue as their left operand, and the type of an assignment expression is that of its left operand. The result of the assignment operation is the value stored in the left operand after the assignment has taken place; the result is an lvalue.

**ISO C11(N1570)**

> ## 6.5.16

> 3 An assignment operator stores a value in the object designated by the left operand. An assignment expression has the value of the left operand after the assignment,111) but is not an lvalue. The type of an assignment expression is the type the left operand would have after lvalue conversion. The side effect of updating the stored value of the left operand is sequenced after the value computations of the left and right operands. The evaluations of the operands are unsequenced.

111) 暂略。

**ISO C++11**

> # 5.17

> 1 The assignment operator (`=`) and the compound assignment operators all group right-to-left. All require a modifiable lvalue as their left operand and return an lvalue referring to the left operand. The result in all cases is a bit-field if the left operand is a bit-field. In all cases, the assignment is sequenced after the value computation of the right and left operands, and before the value computation of the assignment expression. With respect to an indeterminately-sequenced function call, the operation of a compound assignment is a single evaluation. [ Note: Therefore, a function call shall not intervene between the lvalue-to-rvalue conversion and the side effect associated with any single compound assignment operator. —end note ]

　　概括起来，明显的变化是现在可以确定对操作数的值的计算、更新左操作数和整个赋值表达式的值的计算依序进行。当操作数的求值是赋值（以及语义同 `+= 1` 的内建前缀 `++` 时），这里的副作用和更新包含它的赋值表达式的左操作数这个副作用之间的顺序就被确定了——这是之前没有的。

　　因此，对于内建非 `volatile` 类型，`i = ++i + 1` 这个表达式不再具有未定义行为。

　　这个修改的可行性在于是放宽而不是加强限制，且现有的实现基本无需做出修正就可以直接符合新的标准要求。

# 5. C 和 C++ 在内建赋值上的差异

　　无关上面的修正，有一个根本差异都没有变化： C 的赋值表达式不是左值， C++ 的赋值表达式是左值。

　　这在判断涉及 `volatile` 左值的赋值时应该尤为注意。

　　对于 C 来说，赋值得到一个值而不是 `volatile` 修饰的左值，作为左操作数时不涉及必然读 `volatile` 对应的存储：

**ISO C11(N1570)**

> 111) The implementation is permitted to read the object to determine the value but is not required to, even when the object has volatile-qualified type.

　　这里若需要读取，只能理解为赋值自身的副作用。

　　事实上因为赋值表达式不是左值，也不可能允许 `(x = 1) = 2` 这样的表达式。所以从属是确定的，不存在顺序不明确的问题。

　　但对于C++来说，情况就不一样了。作为右操作数的 `volatile` 左值需要通过 lvalue-to-rvalue conversion 转换为右值，这是对 `volatile` 的读取的额外副作用。左操作数的左值若需要保持左值则不需要。
　　这里还有个陷阱：若赋值表达式是完全表达式，那么整个表达式作为左值是否应该转换成右值（对 `volatile` 类型来说导致一次额外副作用）？
　　这个问题在C++03中的答案是右值，也就是说对 `volatile` 左值赋值后需要无条件从被赋值中左值取得存储的值来作为右值。这在实现中可以被轻易优化掉，但在语言规则中存在一定问题，如：

```
void f()
{
    volatile int x;
    x; // Undefined behavior?
}
```

　　若坚持这一点，上面的代码就存在未定义行为，因为读取未初始化的对象——即便读出来的值实际并没有被用到。
　　这看起来不太合理。 Core Defect Report 举出了类似的例子（虽然没涉及未定义行为，从中也可以看出 C 和 C++ 在这个问题上的差异）：

> ## 1054. Lvalue-to-rvalue conversions in expression statements

> ### Section: 6.2  [stmt.expr]     Status: FDIS     Submitter: Hans Boehm     Date: 2010-03-16

> [Voted into the WP at the March, 2011 meeting.]

> C and C++ differ in the treatment of an expression statement, in particular with regard to whether a volatile lvalue is fetched. For example,

>     volatile int x;
    void f() {
        x;    // Fetches x in C, not in C++
    }

> The reason C++ is different in this regard is principally due to the fact that an assignment expression is an lvalue in C++ but not in C. If the lvalue-to-rvalue conversion were applied to expression statements, a statement like

>     x = 5;

> would write to x and then immediately read it.

> It is not clear that the current approach to dealing with the difference in assignment expressions is the only or best approach; it might be possible to avoid the unwanted fetch on the result of an assignment statement without giving up the fetch for a variable appearing by itself in an expression statement.

> **Proposed resolution (January, 2011):**

略，见下文。

　　C++11 最终采纳了这个建议，增设一段，明确保证不转换：

**ISO C++11**

> # 5

> 10 In some contexts, an expression only appears for its side effects. Such an expression is called a discarded-value expression. The expression is evaluated and its value is discarded. The array-to-pointer (4.2) and functionto- pointer (4.3) standard conversions are not applied. The lvalue-to-rvalue conversion (4.1) is applied only if the expression is an lvalue of volatile-qualified type and it has one of the following forms:

> — id-expression (5.1.1),

> — subscripting (5.2.1),

> — class member access (5.2.5),

> — indirection (5.3.1),

> — pointer-to-member operation (5.5),

> — conditional expression (5.16) where both the second and the third operands are one of the above, or

> — comma expression (5.18) where the right operand is one of the above.

　　这个概念同时也用于简化其它一些规则的描述。

