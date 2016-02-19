# Some comments on refining expression evaluation order of C++

Quoted text are from the referenced material: [P0145R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0145r1.pdf).

## 1 Introduction

> Order of expression evaluation is a recurring discussion topic in the C++ community.

Suspectable. Because ...

> In a nutshell, given an expression such as `f(a, b, c)`, the order in which the 
sub-expressions `f`, `a`, `b`, `c` (which are of arbitrary shapes) are evaluated is left  unspecified by the standard. If any two of these sub-expressions happen to modify the same object without intervening sequence points, the behavior of the program is undefined.
For instance, the expression `f(i++, i)` where `i` is an integer variable leads to undefined behavior, as does `v[i] = i++`.

The current rules are already somewhat clear (compared with the proposed rules) and not hard to recite. It seems that there are few people willing spending time on this topic continuously. Novices usually do not try to change the core language rules, and seasoned users usually do not bother to do too much beyond setting up the convention to follow the standard.

> Even when the behavior is not undefined, the result of evaluating an expression can still be anybody’s guess.  Consider the following program fragment:
```
#include <map>
int main() {
	std::map<int, int> m;
	m[0] = m.size(); // #1
}
```
What should the map object `m` look like after evaluation of the statement marked #1? `{{0, 0}}` or `{{0, 1}}` ?

It should be clear without guess because every time the reader meet `=`, he should trace the dependencies (including evaluation order) trivially in a few milliseconds. Since [CWG222](http://wg21.cmeerw.net/cwg/issue222) is resolved, it should be safe to merge the parsing and tracing passes in one's brain for both overloaded and builtin `=` as long as the left side operand has no insane subexpressions screwing up the value dependencies, which should be easy to keep (away from abuse of macros and "properties", etc). Here the effect of `=` is up to the values of operands uniquely. Note the fragment is *not* the case like `m[m[0] = m.size()] = m.size();`. On the other hand, will it be a problem in practice? Just *do not write obviouosly stupid unreadable code*.

If it is still indeed annoying for someone, then C-like languages are likely not suitable, please try Lisp-like dialects without such overhead of dealing with infix expressions instead.

## 2 A Corroding Problem

> The traps aren’t just for novices or the careless programmer. 

Not exactly. If one does not know why these fundamental rules are important and why ignorance of them is dangerous, he *is* a novice. If one has no idea to avoid it, he *is* a novice. If one cannot force *himself* to obey the convention to prevent such trap, he *is* careless. The remained problem is how to enforce *others* following the convention, but that is another story.

> Even if you would like to blame the “excessive” chaining, remember that expressions of the form `std::cout << f() << g() << h()` usually result in chaining,  after the overloaded  operators have been resolved into function calls.

It is questionable that how "excessive" comes. There is no clear boundary to classify what is "excessive" chaining based only on syntactic use without specific knowledge of call-side scenes. However, if one have no idea that what would occur in `f()`, `g()` and `h()`, then `std::cout << f() << g() << h()` should be already "excessive" to him.

> The solution isn’t to avoid chaining.

Why? This key reason is not given by the document as it should be.

Avoiding chaining *anywhere* is not intended, but avoiding harmful chaining conventionally *is* feasible in practice, since one can *eventually* avoid them anywhere *when in doubt* (but does not have to). This is similar to expressions that can often with (excessive) parentheses being added around.

> Rather, it is to fix the problem at the source: refinement of the language rules.

The reason is still missing. Why the unspecified evaluation order is "the problem at the source"? The real problem is *the existence of improper use* and *damage caused by such use*. Unless one can prove the "source" is strictly implying these problems, the statement is logically unsound. This cannot be achieved because there exist some practical ways (not depending on this proposal) to avoid such problems.

## 3 Why now?

> However, a living and evolving programming language cannot just hold onto inertia.

True, but again the reason for suggested conclusion is missing. Why unspecified (not "underspecified") evaluation order in general is just "inertia"?

It should be easy to understand that disallowing evaluation with unspecified order and not providing replacement in general is weakening the expressiveness (about indeterminism) of the language. However the document ignores it totally.

> The language should support contemporary idioms.

Questionable. What support can the language provide? How? There are many "contemporary idioms", are all of them deserved to be supported by modifying the language rules? If not, which subset of idioms should be treated like this?

A more apporiate statement can be: the language should *allow* contemporary idioms, and *encourage* them when there are no obvious defects and superior replacements.

> For example, using `<<` as insertion operator into a stream is now an elementary idiom.  

False. Using `<<` as insertion operator into a stream is only idiomatic for *formatted insertion* of *standard stream classes* and similarly designed classes, and actually not so *elementary* because it is somewhat domain-specific and less dependent by widely spreaded higher-level usage.

A proper (indisputable) example of "elementary" idiom can be "using `<` to express strictly partial ordering" (which has been further supported by `std::less` and being depended by widely used associative containers, etc). Another can be using unqualified `swap` to swap contents of two objects, which has been supported in Clause 17 and many other clauses of the standard, rather than than only in a few parts of Clause 27 and Clause 30.

> So is chaining member function calls.

Nor are chaining member function calls. The method chaining style and related so-called "fluent interface" are strongly OOP-flavored, and even argubly not a good OOP practice in general in other "OOP" language communities. Since C++ is a multiparadigm language rather than "OOP" one, it is questionable to standardize such a disputable style overall into the language specification (either the language core or the library-wide descriptions). That is, it should even *not be encouraged in general*. BTW, there are other easy-to-use idiomatic(able) way to achieve some main goals of method chaining in FP flavor, with some better properties.

> The language rules should guarantee that such idioms aren’t programming hazards.

Since such idioms are not unique, the guarantee is not needed urgently.

> We have library facilities (e.g. `std::future<T>`) designed to be used idiomatically with chaining.

Still usable without chaining.

> Without the guarantee that the obvious order of evaluation for function call and member  selection is obeyed, these facilities become traps, source of obscure, hard to track bugs
, facile opportunities for vulnerabilities.

Then forbid such use by fixing the interface, if teaching the correct idiom is too cumbersome.

> The language should support our programming.

Only with a clear design and a clean clue in user's mind to reason why (or not) he should take the idiomatic way.

> The changes suggested below are conservative, pragmatic, with one overriding guiding principle: **effective support for idiomatic C++**. 

Actually they are not conservative because they changed the expression semantics so broadly (though the meaning and the behavior of the programs may keep the same); also not so pragmatic because of the limitation of the idioms they supported.

The "guiding principle" is not so principle as "guiding" because it is lack of:

* the guideline to determine which idioms are "idiomatic" enough that even the core language rules can be sacrificed;
* the method of measuring the effectiveness.

> In particular, when choosing between several alternatives, we look for what will provide better support for existing idioms, what will nurture and sustain new programming techniques.

This is also methodologically questionable. It seems that "to support of member function calls chaining" has been the ultimate premise even before the principle itself. For example, what about to abandon and to replace it by other idioms? The document said nothing about alternatives on such level. And what kinds of new techniques are expected to be nurtured?

> The primary focus is on what the programmer reads and writes, in particular in generic codes, not what the compiler internally does according to fairly arcane rules.

This is probably not feasible, because as different compliers do, different human users may also *always* read and write differently, e.g. to assume and depend on the left-to-right order as possible vs. to assume indeterminate evluation and to depend on explicit sequenced ordering (while there should only be one concise set of rules of the language, of course). The worse thing is, there is no way to refine programers (as conformance to compilers) being away from TIMTOWTDI. So in this case, change of rules can only reflect some of the existed practice, thus the result may be less idiomatic than the status quo, which largely defeats the original purpose.

> Rather, the driver seat should be given to idioms. 

Which idioms?

## 4 A Solution

> A simple solution would be to require that every expression has a well defined evaluation order.

This can hardly be simple in C++. The proposed changes of rules also shows it.

> In summary, the following expressions are evaluated in the order `a`, then `b`, then `c`, then `d`:

>	1.	`a.b`
>	2. 	`a->b`
>	3.	`a->*b`
>	4.	`a(b1, b2, b3)`
>	5.	`b @= a`
>	6.	`a[b]`
>	7.	`a << b`
>	8.	`a >> b`

"b1, b2, b3" seems to be a typo.

And what about other binary operators? Another trap?

## 5 Postfix Increment and Decrement

> The side effects of unary expressions shall be committed before the next expression (if any) is evaluated if it is part of a binary expression or a function call.

So a careless writer is still easy to make it a mess. In order to figure out what the expression means, readers may be forced to find which is the "next" expression to be evaluated in these contexts, or they will probably be trapped again. This is even worse than before.

## 6 Formal Wording

It seems that "the value computation and the associates side effects of E1 are sequenced before those of E2" are repeated too many times.

## 7 Implmentation Experience and report

> **It is worth noting that these results are for the worst case scenario where the op timizers have not yet been updated to be aware of, and take advantage of the new evaluation rules and they are blindly forced to evaluate function calls from left to right.**

How do the optimizer take the advantage of stricter limitation on imlementation?

> Based on these experiments, we feel confident recommending the left-to-right evaluation rules for syntactic function calls and in the functional cast notation involving more than  one arguments in the argument list. 

This chapter is mostly about experience on specific implementation, concerned with some QoI issues. These experiments do not provide some further information to answer:

* How to estimate the worst cost to modify a current implementation in general?
* May it affect adding non "traditional" reordering optimization in future as per as-if rules?
* Will it leads to more ABI problems?

Note that an implementation may even need to modify nothing to be conforming.

## 8. Alternate Evaluation Order for Function Calls

> We do not believe that such a non determinism brings any substantial added optimization 
benefit, but it does perpetuate the confusion and hazards around order of evaluations in function calls.

Please prove your belief.

