# The question

The original question: https://stackoverflow.com/questions/54310982/int-vs-stdsize-t-which-one-should-be-the-default-integer-type-for-c.

The question has once voted on hold to be closed due to "primarily opinion-based", but (surprisingly) it is now removed under moderation. As said in the URL, basically it is about to preferring `std::size_t` or `int` "by default", and the OP's opinion is to always use `std::size_t` as possible, which is the opposite of the view from current committee (WG21) mentioned [here](https://www.reddit.com/r/cpp/comments/7y0o6r/is_it_a_good_practice_to_use_unsigned_values/ductv0f) and the rules presented [here](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-nonnegative).

# The brief answer

This basically is a typing problem: to get a static [manifest type](https://en.wikipedia.org/wiki/Manifest_type) fit for the some contexts need the explicit types. Ideally, this can be solved formally, and *consistently*.

To illustrate the reasoning of decision, some formal properties can be defined here. We can define a binary relationship showing the preference: a *more appropriate* type `T` is superior and preferred than a less appropriate type `U`, iff. we can prove there are some properties strictly more suited for the context preversed by `T` but not by `U`, and other properties are neither preferred or not between `T` and `U`. Besides the domain-specific meaning the type models (e.g. the mathematic value for a numeric value type), the properties of a type are often provided as *contracts* which show users the formal guarantees (*invariants*) it holds for any [inhabitants](https://en.wikipedia.org/wiki/Type_inhabitation) (in C++, the prvalues and any glvalues before decaying of that type, as well as any object having that type).

Using the traditional interpretation of the data types for numeric values, a type can be interpret as a set representing the (mathematical) domain of the numeric values. If you know the numeric values you need are integers and they are never negative, the unsigned integer types are certainly more "appropriate" to signed ones in this sense, because the choice of "unsigned" implies at least one more contract (being nonnegative) has been already encoded in the type system, thus it is more type-safe. (Actually, the "unsigned" way can be reviewed as a constrained [refinement type system](https://en.wikipedia.org/wiki/Refinement_type).) This is exactly the case here for indices: nonnegative contract benefits, and you basically have no choice to the upper bound of the integer unless you don't care the cost of a arbitrary precision integer type (which is ususally an overkill for an index value). And I don't think there are other properties to be taken into account once the upper bound is large enough. So the reasoning terminate with the result that unsigned ones are more appropriate than signed once. As you knows, `size_t` is a portable unsigned type delibrated designed to the context, it is certainly the winner *in the standard way* (and if you want a signed integer, possibly `std::ptrdiff_t` is always preferrable to `int` in the contexts of pointer arithmetics).

But wait... you know you're using C++. The reasoning would be complicated for its strange semantics of types. Dividing integer types to signed and unsigned is not precise. There actually exist representation-dependent cut of the family of the standard integer types: the one which can be overflowed (leading to undefined behavior) and the one always being wrapped in 2<sup>n</sup> modular arithmetics during arithmetic operations. And occasionally, the former is exactly the signed integer types, and the latter is exactly the unsigned integer types. So there are other concerns. That is, using the unsigned integers instead of the signed integers may be harmful to optimization. The decision of using the unsigned types itself is a premature optimization in this sense. Moreover, missing refinement on nonnegative values may [also cause redundant logic in code](https://github.com/devkitPro/libfat/pull/1), even such cases are sometimes more easily found (and eliminated) by both machines and humans.

So where is the problem from? Taking this into account as an extra "other properties" in the reasoning above, you suddenly have one more (unexpected) contract when using unsigned types: the well-defined wrapping (modular arithmetics) behavior. It is cerntainly not needed for this case because the wrapping of the indices is nonsense and it may even hide bugs. In C++ parlance, the condition to guarantee no overflow would occur should better be a [narrow contract](http://www.open-std.org/JTC1/SC22/WG21/docs/papers/2014/n4075.pdf) checked elsewhere (before using as the indices). Sadly, C++ does not provide an obviously more appropriate choice: an unsgined type without wrapping. So you have to bear the fact you have to use a fallback instead (to make it still a portable C++ program).

Moreover, it renders that signed integers and unsigned integers are not directly comparable in the sense of "appropriate" relationship, becuase missing nonnegative guanantee and having unwanted wrapping behavior is not simply comparable. This is probably the only point which is really "[opinion-based](https://stackoverflow.com/help/closed-questions)" in the problem. However, I tend to choose the unsigned type because the nonnegative property sets a deterministic boundary of the values, so it can give more help to prevent misuse of the types in a consistent and reliable way.

## Consistency of the choice

There are other evidences to support the result above. It implies when we need an (mathematical) object of a known set, we don't need to  introduce an entity to model its superset in order to represent the value of the object. To use signed integers for nonnegative numeric values by default sounds consistent to use real numbers for integers and to use complex numbers for real values, which are absurd. And why not use `std::complex`, `std::float_t`, `std::intmax_t` anywhere? (Does the contexts of *constant-expression* count? OK, then you leaks even more details in C++ implementations of some sorts of [partial evaluation](https://en.wikipedia.org/wiki/Partial_evaluation).) So I can hardly found the logic to endorse (fixed-width) integers here when there is a tighter choice built-in the core language as well.

Actually there do have some considerations on implementation details, like bullet 4 in [this answer](https://stackoverflow.com/questions/18795453/why-prefer-signed-over-unsigned-in-c/18796546#18796546). However, relying on the fact just reminds us that we are using some language implementations still not stronger enough to do the transformation between different width of integer automatically (even in the internal use), which is actually allowed in ISO C++ when it can be *proved* (granted by the [as-if rules](http://eel.is/c++draft/intro.abstract#footnote-4)). This is not a satisfying situation. We do pay too much that we don't want to have not for the language, but for the implementations. To make the choice into the language interface seems somewhat more sophisticated premature optimizations.

## Some different concerns on contrete types

### What about `int` vs `int32_t`?

(Well, `int` is always mandatory for current version of ISO C and ISO C++, but `int32_t` is optional. To simplify the discussion, we assume `int32_t` has been provided by the implementation we use.)

In the view of type theory, the latter is a subtype of the former, as it refines the behavior with explicit knowledge of data with and the latter has nothing more significant difference expected from the former, so they conform the relationship of [behavioral subtyping](https://en.wikipedia.org/wiki/behavioral_subtyping). Sadly, such subtying is limited as subclassing in C++ (so they are never available in this case) and you will only have some limited forms of [ad-hoc polymorphism](https://en.wikipedia.org/wiki/Ad_hoc_polymorphism)  (i.e. some type conversions) between them.

Once you have changed one, replacing it to other can make the semantics of the program changed unexpectedly. The consequences can propagate not only to the C++ source programs, but also to the binary programs produced from the C++ code, unless you have some additional assumptions (ABI compatibilities) at the cost of portability of the program.

That said, you still have to choose one if you don't want to duplicate the interface (which is usually a nightmare for maintainers of the code, esp. with different platform configurations) and to distribute the code by generic code (which in reality means you have to almost always provide it in source code). Similar to the analysis for "appropriate" relationship above, we want to find the difference of the contracts implied by the types. As the result, the only concern is that `int32_t` has the one more contract on the data width. I'd argue `int` is always better `int32_t` in such cases unless the concrete data width (`32`) is explicitly needed by the problem domain (to keep the solution meaningful). (And if you do want to rely on the exact width, e.g. in the design of a protocal shared externally, you should *document* the choice of the width explicitly, better with the reason.) This is like the fact a programmer may prefer base classes instead of derived classes in interface design (because the rule - [Liskov substituing principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle) - used here is about subtyping, and subclassing is one form of it). Otherwise there are can be many consequences (I believed) unwanted by most senior programmers: distraction (rather than [separating](https://en.wikipedia.org/wiki/Separation_of_concerns)) of conerns, [leaking](https://en.wikipedia.org/wiki/Leaky_abstraction) the implementation details, undermining the [information hiding properties](https://en.wikipedia.org/wiki/Information_hiding), violating the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege), and so on.

### What about `size_t` vs built-in unsigned types?

Because it is an index, a `size_t` is clear enough. Some other names may have risks of over design, but it depends.

In other cases, using an unsigned type other than `size_t` is approiate. But most differences should still be the details. For example, better do not relying on the differences (including the width) between `unsigned long` and `unsigned`. I will use `unsigned` in such case, because it is short:)

A voilation of this reasoning is that hash requires `size_t` as the result. This looks like an overspecification.

The standard library has some cases to use `unsigned long long` to represent unsigned interger with a largest range. This should be not a good practice following the reasoning (and it should be `std::uintmax_t` instead).

Sometimes a built-in mandatory type is needed for the standard library interface. Such interface is actually designed by workaround.

### What about signed integer (mis)use cases in standard library?

The `std::shared_ptr` has a counter type of `long`. I don't find the reason at all. It may be attribute to `boost::shared_ptr`. An invented unsigned `count_t`, or simply `std::size_t` (following the prior art of the standard library containers in the `std::default_allocator` cases) will be consistent and easier to reason.

The `std::uncaught_exceptions` use `int` for the reasons endorsed by Herb Sutter and Bjarne Stroustrup. However, it is still not sufficient discussed in a formal sense. Actually I have asked [the question in the std-discussion list](https://groups.google.com/a/isocpp.org/forum/#!topic/std-discussion/6UH9pMfh9xk) but there is yet no official (WG21) response and most people do agree this is based on opinions.

## The purpose

It is well-known that Bjarne Stroustrup is eager to "teachablility" of the new C++ dialects in recent years. However, the way to promote `int` over unsigned types does seem suspective for the purpose, because it simply encourages users to consider less without addresing the core problem: how to choose the type consistently? Besides the original problem, this is exactly the purpose of this answer, and it does have some concrete choices with some effort to minimize subjective opinions in the judgement, *under some consistent rules*.

# Appendix

This sections is the responses to some comments in the original question.

> the point is not that you cannot write correct loops by using an unsigend index, the point is that by using a signed index so many typical errors can be avoided 每 user463035818

This is simply wrong. It should even be easily to *prove* some use of the indices when you see the source code sometimes; for example, a simple correct `for` loop with a `size_t` index over an array object. Note this is still not replacable by range-based `for` if you have to do something on the index itself (e.g. print it out).

> its about "difficult to get wrong" not "come on just write correct code then you wont have a problem with unsigned integers" ;) 每 user463035818

Still false. Being not "appropriate" can be actually more error-prone becuase of misconceptions and ignorance on the concrete types the coder has used, particular when answering "why choosing such a type at first" on the failure of modification of the code due to over-tightened type contracts propagated to (polluting) the interface at the call sites, leading to further annoying design defects (ABI compatibility ...).

Note trying to make it "appropriate" can be an instance of *smoothness* (coined by M.D. McIlroy) in the broader context of the programming language theory, but I am not ready to expand this topic soon.

> For one, I would consider the unsigned loop you wrote confusing at first glance. Any programmer reading or writing such loops has to be intimately familiar with the wrapping behavior of unsigned integers to make sense of that loop. Yes, if all the programmers on your team are that deeply acquainted with C++ it can work, but this relies on some very particular aspect of the language and will lead to confusion eventually.

This is also not true. The indices can be already checked by narrow contracts in some configurations.

> The real problem comes in when you start to mix signed and unsigned operations. Math involving an offset to an element (signed since you could be going backwards) and the container size (unsigned because reasons) can lead to unexpected results. You have to think very carefully about what you are doing and that normally is a place where bugs will crop up.

This is another topic. And in reality, it is relative easy to resolve, e.g. with some `-Werror` flags in G++/Clang++ (though such use is not conforming by put more constraints on the code; anyway, it does not cost much to be configured away).

> "INT_MAX might less than vec.size()" That is the problem of mixing convention. Both have their pro and cons. 每 Jarod42

Use `UINT_MAX` (if you think `std::numeric_limit` is too lengthy or you have to use it in the preprocessing phases) at first, then there is no this particular problem.

> @cqwrteur How do you get an offset (or difference) without using a signed integer? I suppose you could use a bool to tell you if you are going forward or backword but now you have more variables and branching. 每 NathanOliver

That depends on what is "use". In C++ terms, you can avoid "odr-used" values without explicitly written forms, e.g. by some instantiations of `std::common_type`. Burying them in generic code is quite easy, if not error-prone.

> The `int` is usually the size of a processor's register with a minimum range. The size_t can be larger. For example, some 32/64-bit machines, there can be a 32-bit int and size_t is 64-bit. 每 Thomas Matthews

It may be good to know the roles they have played historically (and some contemporay implementations are worth a look), but relying on them is rather dangerous without any further guarantees. Once you have relied on such guanatees, the resulted programs would be less portable. Usually, keep away from them unless really necessary.

BTW, also note the initial adoption of the notion "register" in the (C) language is only significant historically for implementations. Today the keyword `register` is even ignored, and [it even may have some other meanings some day](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0577r1.pdf) (*disclaimer: no endorsement or championship is implied here*), as every C++ programmers had better know.

> @Jarod42 Why do you use static_assert when you can solve your problems easily by replacing int without std::int32_t. Longer code without a meaning to write is probably a new bug as well. 每 cqwrteur

Additionally, `static_assert` is also a disaster in some SFINAE contexts :)

