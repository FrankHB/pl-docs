# Introduction

This document lists features wanted but not found (in a satisfied flavor) in existing contemporary programming languages (with some discussions and external resources presented together). It is also used as a collection of descriptions about the reasons (and the reasoning) for a new **general-purposed** language. Moreover, it implies the contents in a [requirement document](https://en.wikipedia.org/wiki/Product_requirements_document) of the language.

## Disclaimers on neutrality

This document tries to describe objective views on the aspects being interested by arbitrary but unspecified stakeholders of the language. Most significant aspects including generality and simplicity. However, the judgement of interests otherwise can be largely subjective. It may also be authored (and authorized) with some subjective concrete points (e.g. on the set of features) occasionally. Such points are not defects iff. there is sufficient reasoning specifically.

That said, any biased points without sufficient reasoning are not intended, albeit the judgement of "sufficient reasoning" is also somewhat subjective.

### Judgement on the general-purposed language

A significant judgement to generality and simplicity is based on the how much ability the language has to solve problems in various [problem domains](https://en.wikipedia.org/wiki/Problem_domain). When the features of the language expose enough of these properties, it is considered a general-purposed language. Otherwise, it is only enough to serve to some specific problem domains well, hence a [DSL (domain-specific language)](https://en.wikipedia.org/wiki/Domain-specific_language).

By leaving any specific domains out at first, the language needed here has to be general; it is also simple to avoid any unexpected dependencies meaningful only for a few specific domains. Such approach can still serve to the resolution for different domains by *derivation* (see the Outlines clause below for details) instead of a set of various DSLs, which is not considered simple here. Again, the boundary of being general-purpose is somewhat but not wholly subjective. It should be quite restrictive because many existing language features are eventually excluded as they are considered only serving well enough to specific domains. Such justification of subjective reasoning as well as the feasibility (the point of "well enough") is covered by the extents of the disclaimer here.

In particular, the *smoothness conjecture* (in the section 1.1.2 of [the dissertation introducing the vau calculi](https://web.wpi.edu/Pubs/ETD/Available/etd-090110-124904/unrestricted/jshutt.pdf)) is interested in the design in this document, because a language can only become enough general-purposed by providing sufficient ability of adaption in different problem domains with effective and suited ways of abstraction in both the language to be derived and the languages being derived.

### Conservative adoption on proposed features

This document tries to follow the first sentence of the INTRODUCTION chapter in [R<sup>n</sup>RS](http://community.schemewiki.org/?RnRS) specifications and takes it more seriously (despite that the rule is not strictly obeyed by versions of R<sup>n</sup>RS themselves as this document):

> Programming languages should be designed not by piling feature on top of feature, but by removing the weaknesses and restrictions that make additional features appear necessary.

The judgement of "necessary" however largely falls in the extents of the disclaimer here, except for the points explicitly specified in this document.

## Styles and layouts

Some features can be quite "large" in the sense of specification, so they are listed alone by subclauses.

Approved and proposed design choices are organized as the following "Outline" clause. Main highlights (pros) of a design choice as short phrases may be listed in lists, following the subclause title or the introduction paragraph.

For the purpose of feasibility, concrete languages may be illustrated. They are also used as proof-of-concept resources about technical difficulties and reasonability of the design, which would not be exhaustive here.

The features are about the design of languages which. Nothing beyond specifications are assumed here except illustrations to prove implementation feasibility.

# Outlines

For generality, only the general-purposed properties are emphasized here, as a DSL can be *derived* from a properly designed general-purposed language. (A DSL may not require most of the features, though.) Some listed features are difficult to be derived, so they deserved to be *primitive*.

## Specified

The language shall have a *normative* specification.

Otherwise, the design is incomplete *on purpose*. This is often true for a language being evoluted.

An example is [the Rust programming language](https://www.rust-lang.org/en-US/). As of writing, it does not have an official specification, although it seems the specification is to be done in near future.

Users can rely on documentation other than specifications, but judgement of the language design from user documentation may be improper.

## First-class falsity

* Allowing distinction of *undefined behavior* (but not *unspecified* one)

The term "falsity" means the ability to allow [logical false](https://en.wikipedia.org/wiki/False_%28logic%29) (often expressed as symbol ¡Í)  formally occurs in [well-formed formulae](https://en.wikipedia.org/wiki/Well-formed_formula) in the model of the language, representing the cases can be assumed never happening. Any programs reduce to ¡Í in the model has unpredictable (or at least, non-portable) behavior due to missing guarantees of any predictable behavior by design. Such design provides a model-level axiom to reduce program semantics in an easier way because it (again by design) does not require any uninterested information logically as the premise to be expressed elsewhere, and it provides larger equivalent classes on program behavior compared to those provided by unspecified behavior. Thus, it effectively makes some equational reasoning easier, which may be beneficial for [program transformation](https://en.wikipedia.org/wiki/Program_transformation) (notably, some [program optimization](https://en.wikipedia.org/wiki/Program_optimization)).

This is also significant to allow *derived* languages from a base specification to adapt to new domains which require more refined constraints as the viral property, while keeping the base specification essentially (formally) simple without unnecessary overhead. By specifying the more concrete behavioral requirements complementing the undefined behavior, a derived language (sometimes called as a *dialect*) can have more strictly constrained requirements to provide more guarantees specifically to the target domain. Such methodology of drafting specifications of a *family* of languages (or dialects) would only cost a few resources compared to drafting them separately, due to its nature of reusing of rules in the base specification by default.

This requires embodies a philosophy requirement deeply: do abstract and express, rather than do reason instead. By burying the ease of the implementation under the requirement, it prioritize specific properties of interface at first. As the extent is indeed quite broad, some counterexamples are given to illustrate the intent:

* Someone may conclude [a type represents falsity indicates the "crashed" state of the abstract machine](http://okmij.org/ftp/continuations/undelimited.html#introduction).
	* This is not conforming because it has introduced an implicit equivalence between "crashed state" and the [negation](https://en.wikipedia.org/wiki/Negation) of predictable states (*well-behaveness*), which is an implementation detail rather than the interface property.
	* See also the discussion on mandatory typechecking in subclauses below for details.
* Someone may conclude [closures imply predictable access of captures](http://lambda-the-ultimate.org/node/5007#comment-81721).
	* The [declared notion to define the requirements](http://lambda-the-ultimate.org/node/5007#comment-81724) are based on existed language designs and even well-defined formal (denotational) semantic models, but all of them are specific to a few concrete language designs and neither of them has actually abstracted properly the need of distinction between a closure and "an object that happens to have some mutually recursive methods".
	* This is not [historically](https://en.wikipedia.org/wiki/Closure_%28computer_programming%29#History_and_etymology) significant out of the concrete contexts above. For the original case and other simplest cases like [STLC](https://en.wikipedia.org/wiki/Simply_typed_lambda_calculus) mentioned here, the distinction is irrelevant because terms are not able to be recursively owned.
	* Later closures are proposed to resolve the [funarg problem](https://en.wikipedia.org/wiki/Funarg_problem). However, the solution actually introduces an implicit premise relying on well-behaveness of accesses to captures, making the distinction significant. Although the problem itself is widespread (for any languages has the similar "function" notion), the solution is not adopted by all designs, so the meaning of the term "closure" is not naturally strengthened with more restrictions of well-behaveness.
	* Because nothing is otherwise required to restrict the extended meanings of "lexical" closures (used other than the simplest cases above) being well-behaved in the general sense in other directions, the contextual meaning ultimately depends on the language rules which define the term "closure". As of in general, the distinction should be still insignificant. (This approach of general terminology is also consistent to the spirit of "variables do not necessarily imply mutable states" whether the context is the so-called "functional languages" or not.)
	* It is actually insignificant in some contemporarily designs. At least ISO C++ uses the terms [*closure object*](http://eel.is/c++draft/expr.prim.lambda#2) and [*closure type*](http://eel.is/c++draft/expr.prim.lambda#closure-1) satisfying both notions here.

## Examples

[ISO C](http://www.open-std.org/jtc1/sc22/wg14/) and [ISO C++](http://www.open-std.org/jtc1/sc22/wg21/) fall in this category. Most others (like [Java](https://docs.oracle.com/javase/specs/jls/se11/html/index.html)) are not. Some languages only have undefined behavior in contexts which need to interact with the languages having this feature, e.g. `unsafe` in [C#](https://en.wikipedia.org/wiki/C_Sharp_%28programming_language%29) (specified by [ECMA-334](https://www.ecma-international.org/publications/standards/Ecma-334.htm)).

## Calculi based

* No need of extra semantic model
* Provable of basic semantic rules in a manner without ambiguity

This falls in the sort of the so-called [formalism](https://en.wikipedia.org/wiki/Formalism) flavor. Most industrial languages do not meet this point.

As a minor concern, lack of designated models may introduce controversial views between existing languages and the models, which seriously undermines the value of any other candidates in practice. For example, the case about so-called [OO languages](http://www.atalon.cz/om/what-is-a-metaclass) reveals that almost all industrial OO languages are not well-formalized for years. Introducing a model specifically for the design of a language can avoid such embarrassment for that language, although there is still room of divergence on the topic of common properties among different languages.

A notable exception is [Standard ML](http://sml-family.org/). A recent version of its specification is [here](http://sml-family.org/sml97-defn.pdf).

A secondary kind of such category is an informal model (usually expressed in natural language) in the specification, which can be formalized later. This is considered compromised to the purpose of formal specification. Nevertheless, a few languages can have the work together in their specifications. For example, the Scheme language have more than one models in various R<sup>n</sup>RS specifications in their appendix, but they are not normative. For Scheme, there are also standalone detailed documents like [this](http://users.eecs.northwestern.edu/~robby/pubs/papers/jfp2008-mf.pdf).

Some non-specifications for existing language dialects also fall in this category, like [[Muller92]](http://www.cs.bc.edu/~muller/research/postscript/toplas92.ps) and [[Ellison12]](http://fsl.cs.illinois.edu/pubs/ellison-2012-thesis.pdf).

There exist works on formal models to improve current language specifications, like [this](http://fsl.cs.illinois.edu/pubs/ellison-2012-thesis.pdf). Similar work can identify the issue on some existed specification that is normative (but not based on formal models), like [this](https://github.com/cplusplus/draft/issues/2541).

A more specific requirement is the model should be *computational*, as a general language is always implemented with [*computational effects*](https://www.sciencedirect.com/science/article/pii/S1571066104050893/pdf), rather than by *(equational) reasoning*. The latter is centric in the models for some DSL (e.g. languages used in proof assistants) and some implementation methodologies (e.g. program transformation), but it is not necessarily in the designs of general-purposed languages. This is the main reason why "calculi" rather than more generic (arbitrary) "models" are focused here.

Although historically there are languages designed based on [*formal logical systems*](https://en.wikipedia.org/wiki/Formal_system#Logical_system) or targeting the use of proofs (notably, [ML](https://en.wikipedia.org/wiki/ML_%28programming_language%29), and they may have better designs (in the sense of formalism) than other ordinary languages in nature, they are not considered "really" general-purposed here, because the models are heavily tuned for specific use by *some specific type systems*. This is not acceptable because computational system does not imply type systems. Although the neutrality of the methodology used in formalism does not rule out such designs directly, relying on type systems (even in the meta language) is now allowed by other policies. See discussions related to type systems below.

Calculi are refinement of [*rewrite systems*](https://en.wikipedia.org/wiki/Rewriting). They may share other properties (e.g. [*Church-Rosser properties*](https://en.wikipedia.org/wiki/Rewriting#The_Church¨CRosser_property_and_confluence)) with a broader sort of models ([*deductive systems*](https://en.wikipedia.org/wiki/Formal_system#Deductive_inference)). However, they are also required to be with computational meanings. For simplification, in an object language the rewrites are modeled of *evaluations* on expressions, while in the meta language they are modeled of *reductions* on *terms*. The meta level transitions may hide some *administrative* information away from the object language when they are not the same (by evaluating corresponding object-language level *redexes* - reducible expressions).

(Note the view of formalization in [the article above](http://www.atalon.cz/om/what-is-a-metaclass) dispenses the model of calculi, because it does not focus on programming but a more high-level view on OO as the foundation of OOP. This is not fit for the purpose to design a specific language, so it is not adopted here.)

### Named calculi based

Like some traditional languages based on [lambda calculi](https://en.wikipedia.org/wiki/Lambda_calculi), with the ability to introduce names.

Such a calculus provides a form of *abstraction* allowing expressing *substitution* with explicitly named *variables* directly, whose results embody the notion of *reusable* building blocks of computation. The most famous example of such device of abstraction is the [lambda abstraction](https://en.wikipedia.org/wiki/Lambda_calculus). As the part of one of the earliest formal models aiming to describe computation in general, it can be the prototype of many other similar facilities. It is also the *de facto* formal prototype of the so-called "function" (rather than [most ones in mathematics](https://en.wikipedia.org/wiki/History_of_the_function_concept)) in most contemporary programming language, even in languages whose designers do not have knowledge of formal models (but just derive the language from the *prior arts*). With the implied *name bindings*, Such device is handy to convey the *meaning* of components of programs in practice, as it directly provides formalized *explicit* (named) ways to describe the *input* (via zero or more named *parameters* in the abstraction) and the defined *output* (via the subterm as the *body* of the abstraction) independently, so users can establish *fine-grained* or the *minimal* local dependencies among some subsets of such program components on demand. One more benefit is that such program components can have computational effects both pure and non-pure without changes to the syntax, which effectively [separate the concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) of the objects being modeled and specifically interested side effects tied on them, in a flexible way. Further, such separation enables a way to implement additional computational effects through some extensions of the deductive rules on existing calculi, so it also separates the concerns to language authors in the way of tailoring the feature set of the languages in some degrees, and the specializations of such a family of languages can be separately developed (by iteration) in a more engineering-friendly way. 

On the contrast, more low-level systems like [combinatory logic](https://en.wikipedia.org/wiki/Combinatory_logic) avoid the abstraction facilities, so users have to manually get it to work via mappings with an *implicit* (e.g. *[point-free](https://en.wikipedia.org/wiki/Tacit_programming)*) style (like functions in traditional mathematics, e.g. combinators in combinatory logic), albeit such mappings themselves are often considered central in a more high-level (more abstractive) kind of coding manner (by the guarantee of leaking less implementation details). Such use may benefit in succinct programs (and prevent headache on naming in nature). However, enforcing it uniformly in a general-purposed programming language is essentially unnecessarily *restrictive* to the choices of users on how to assign meaning to things being consumed by the devices of abstraction. It is also *essentially difficult to program* in general cases. Thus, it is never capable enough in practice except for serving to some *very* specific problem domains. In fact, except quite a few exceptions like the [identity function](https://en.wikipedia.org/wiki/Identity_function) (as the `I` combinator in the [SKI combinator calculus](https://en.wikipedia.org/wiki/SKI_combinator_calculus)), most implicit fundamental mapping (without specific pre-defined meanings in the problem domain) are so sophisticated to reason about, that devices of abstraction in explicit style are often used instead in various ways:

* A convenient mean to express the semantic definitions of such mappings is to use lambda abstractions instead. This is even *canonical* [in the most foundational level in such a system](https://en.wikipedia.org/wiki/SKI_combinator_calculus#Recursive_parameter_passing_and_quoting).
* During programming using such systems, users have to introduce devices of abstraction in explicit style to figure the mappings out (see examples below).

A notable example NOT in the category is [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck), whose "parent" model is [P''](https://en.wikipedia.org/wiki/P¡ä¡ä). Another is [the Unlambda programming language](http://www.madore.org/~david/programs/unlambda/), which implements a dialect of combinatory logic. Different to name-based calculi, the tokens or characters in such languages does not represent *names*. Thus, variable bindings cannot be introduced directly. As a result (and the example of the major difficulty to use them in practice), to figure out how to encode program logic as native mappings (in implicit style, like "functions" in Unlambda) usually requires users first to know the counterpart of the devices of the abstraction in explicit style, like [in lambda abstractions](http://www.madore.org/~david/programs/unlambda/#howto).

The (untyped) [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus) is the typical baseline model for its historical significance.

* Allowing anonymous procedures
* No special declaration grammar needed
* Allowing implementing reflection features quite easily
	* Not needed in essential, see below

Strictly speaking, the lambda calculus has binding construct (the lambda notation) in its built-in syntax. However, this can be easily replaced by an identifier when the derivation support names. This is the traditional way in [Lisp](https://en.wikipedia.org/wiki/Lisp) dialects.

See subclauses below for other details.

#### Impure lambda calculi based

* Allowing side effects, namely, "impure"
* Allowing native (mutable) states natively
	* No need to "compile" manually to get the states transformed

The formal model of impure variant of lambda calculi are relatively recent compared to the practical languages with the feature. See [[Plo75]](http://homepages.inf.ed.ac.uk/gdp/publications/cbn_cbv_lambda.pdf) and [[Fe91]](https://www2.ccs.neu.edu/racket/pubs/scp91-felleisen.ps.gz).

##### Lexical scoped impure lambda calculi based

* Allowing scope-based encapsulation
* Only requiring one kind of scope

[ALGOL-like](https://en.wikipedia.org/wiki/ALGOL) languages, albeit usually without formal specifications, do NOT following in the category because they specified the block scopes as a special built-in language feature. Block scopes (i.e. local scope in modern variants like ISO C and ISO C++) enables information hiding from outer regions of code and avoids name pollution from outer (enclosing) regions. However, the feature itself does not need to be special. The rules of special scoping rules work because of [lexical scope](https://en.wikipedia.org/wiki/Scope_%28computer_science%29#Lexical_scope_vs._dynamic_scope), rather than special properties in specific regions of code. With the lexical scoping core rules, block scopes can be implemented in primitives. This is actually what the Scheme language's `let` family does (see also [this Wikipedia page](https://en.wikipedia.org/wiki/ALGOL#History)). More complicated scoping rules can be simulated similarly once the core rules expose sufficient expressiveness. Keeping different scoping rules out of the core language makes the design simple and succinct.

##### Lexical scoped impure vau calculi based

See [the vau calculi](https://web.wpi.edu/Pubs/ETD/Available/etd-090110-124904/unrestricted/jshutt.pdf) for the base theory.

* No need to specify phases in the language specification
* Reflection and macros now redundant in the design
* Allowing writing compilers directly based on an existed interpreter
* Ensuring most features have no effect on core language features
	* Allowing them to be implemented by libraries provided by users

[The Kernel programming language](https://web.cs.wpi.edu/~jshutt/kernel.html) is designed following this model.

See also [fexpr](https://en.wikipedia.org/wiki/Fexpr) for historical insights and examples of similar featured languages.

## First-class entities

* Ensuring basic consistency and general availability of expressive language features

The dissertation introducing the vau calculi also talks about first-class objects. An instance of such objects is considered a *first-class entity* (or shorten as *entity*) here, as it does not enforce its identity (see below).

First-class entities are also important for abstraction, because abstraction is mainly embodied by using of first-class entities as one of the fundamental methods.

### First-class objects

* Allowing distinction of object identities

An *object* is an entity which preserve its identity. Unless otherwise specified, any two objects can be differentiated by the identity. This definition has more restriction of identity compared to the one in the dissertation introducing the vau calculi (which is called as a first-class entity, see above).

The identity is needed for [ontology](https://en.wikipedia.org/wiki/Ontology) purpose. Although when not built-in, it can still be expressed by derived languages from the base specification, the essence of generality makes this approach unnecessarily *indirect* in the whole design. This is the very same reason compared to the reason about keeping expressiveness of [*side effects*](https://en.wikipedia.org/wiki/Side_effect) in the base specification (rather than providing them through the derived languages), as well as the reason of allowing the programs being *stateful* (see also first-class states below): if a language is lacking of these essential features by design, it is more suited to be a target language [being transformed to](https://en.wikipedia.org/wiki/Program_transformation) (e.g. by a [compiler](https://en.wikipedia.org/wiki/Compiler) during translation of a source program, or within a [proof assistant](https://en.wikipedia.org/wiki/Proof_assistant) during verifying some properties of a source program), but not a language used by human users directly.

### First-class states

* Allowing a normative way of distinction of different side effects
* Enabling possibilities to specific kinds of customized effects (e.g. ISO C style `volatile`)

By specifying identity on objects, side effects can be bound on objects with restricted number of instances. The restriction ensures the effects on the object are not duplicated or eliminated unexpectedly.

### First-class continuations

* Allowing programmable control effects
* Minimal control primitives
	* No need to provide and stick on various special control syntaxes

The reified continuation is first known as the result of [`call/cc` operator](https://en.wikipedia.org/wiki/Call-with-current-continuation) in the Scheme programming language.

#### Composable first-class continuations

* Allowing more reasonably implementable features on continuations
* Simplification on user programs using first-class continuations

Scheme's continuations [do not compose](http://okmij.org/ftp/continuations/undelimited.html#introduction). This limit its practical use. It can be resolved by introducing control delimiters. The resulted continuations are *delimited continuations*. It is also considered [superior to undelimited one [in any reasonable practical case](http://okmij.org/ftp/continuations/against-callcc.html).

There are various of control operators for building delimited continuation. About their expressiveness, see [here](http://okmij.org/ftp/continuations/#impromptu-shift).

### Case studies

C and C++ lack first-class objects as the objects can have decayable types like arrays. Despite that, they have also other non-first-class entities: functions, and references in C++. It is particularly interested that what will happen when they are turned into first-class entities. For entities other than references, the main reason is compatibility of decaying - several kinds of implicit conversions (in C++, `LvalueTransformation` category of several kinds of standard conversion) on specific non-first-class entities, and C++ has provided replacements based on class types (e.g. `std::array`, *closure types* of *lambda-expression*). Except for `void` (treated as an object type in C, and another special kind in C++, while the essence of the type is exactly same), the remaining kind is references in C++ - it is treated specially by the core language rules. Without mandatory of rules on types (see also the discussions about type systems in related subclauses below), there is room to make it also first-class, without interfering the conceptional differences between "objects" and "entities" (see above).

## Proper tail calls

* Providing mandatory guarantee of space complexity boundary of most language constructs
	* No need to abstract loops specifically
* Allowing users to extend the mechanism when needed, with minimal compact to existing syntax

This is an important property exposed by the language design with mandatory rules. See [[Clinger98]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.83.8567&rep=rep1&type=pdf) for formalization on a dialect of the Scheme language. This property is generally language-neutral, and it can be adopted in new language designs.

Note *PTC (proper tail call)* is NOT same to *TCO (tail call optimization)*. See [here](https://groups.google.com/d/msg/comp.lang.lisp/AezzhxTliME/2Zsq7HUn_ssJ) for clarification.

### Implicit tail call notations

* The default style of proper tail calls

This is what mandated in most "truly" PTC languages. Besides, some alternatives of "conditional" forms of PTC are also proposed by introducing syntactically explicit forms to designate the places of PTC needed, like [Syntactic Tail Calls proposed for ECMAScript](https://github.com/tc39/proposal-ptc-syntax/). Such approach is considered compromised, because:

* The opt-in nature directly voids the space complexity guarantee of PTC.
* The syntactic nature limits the contexts to a strict subset of the syntactic contexts in the formal model.
* The explicit nature is against the simplicity of the code "just work".

All the consequences mean users will have strictly less chances to leverage this feature in practice, thus it will essentially undermine the original purpose.

The preference to the implicit style *by default* does not mean that explicit style is always bad. It is still true that users may need to designate the place of contexts explicitly, but this should be usually rare. Namely, such explicit syntactic notation should be used to *opt-out* PTC. Such explicit notation is specifically for the cases when PTC is inconsistent in high-level of the code logic, i.e. to express some non-ignorable but (syntactically) implicit side effects *at the end of activation record frames deterministically*. (Such cases in existing languages include non-trivial destructor calls in C++ and some contract checks in Racket.) Alternatives of implicit PTC in such contexts will at least result in better diagnostics to users, providing better chances to avoid the inconsistency.

Nevertheless, syntactic forms at the call sites are still far from correct, because such cases are exposed by the essential properties of *activation record frames* or *activated instances of the calls* (perhaps in some context-sensitively ways, e.g. effected by the states of previous "parent" frames), but not the expressions forming the entries of the calls.

### Evlis tail calls

* Known most efficient way in space being compatible to pass-by-value semantics

See [[Clinger98]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.83.8567&rep=rep1&type=pdf) and [this page](https://www.akalin.com/evlis-tail-recursion) for introduction.

### Reasonable performance hit

* For availability in practice

PTC may have performance penalty. The cost is generally unavoidable when there is no "native" PTC (provided by the underlying implementation language) available, so more effort should be taken for maintenance work of the additional data structure serving to bookkeeping. Only in limited cases when it can be proved that no such work is needed, the cost can be eliminated, but (depending on the feature set of the language) the proof itself can also be costly. The performance penalty should still be reasonably insignificant at least with the configurations for end-users in practice, so the cost does not defeat the general availability of the feature.

There exist misnomers that PTC or TCO will be harmful to diagnostics and/or availability of debugging information. This is red herring in general. It is true that by implementation experience, PTC implemented by languages without native capability of PTC (which usually requires TCO) needs more complicated mechanism to save the history of the activation record frames avoided by PTC in the call trace compared to naive implementations of ALGOL-like languages based on the "native" ([ISA](https://en.wikipedia.org/wiki/Instruction_set_architecture)-level) [call stack](https://en.wikipedia.org/wiki/Call_stack), but the extra effort (e.g. a [CHICKEN](https://en.wikipedia.org/wiki/CHICKEN_%28Scheme_implementation%29)-style [shadow stack](https://en.wikipedia.org/wiki/Shadow_stack) specifically for debug implementations like [in JSC of Webkit](https://bugs.webkit.org/show_bug.cgi?id=155598)) is not unreasonably. 

## Free store'd activation records

* No need of [the control stack](https://en.wikipedia.org/wiki/Call_stack), which may be easily overspecified
* No need to worry unportable behavior on activation records exhaustion
* No need to expose internal data struct of activation records
* Allowing nested calls are bound by single species of system resource
* Allowing resource usage being configured uniformly through interface of the [free store](https://en.wikipedia.org/wiki/Memory_management#Dynamic_memory_allocation)

This approach does not use "native" control stack provided by traditional [ISA (instruction set architecture)](https://en.wikipedia.org/wiki/Instruction_set_architecture). Losing direct support from hardware seems inefficient, but not much. Practical implementations can use such strategy as well. For example, [SML/NJ](https://www.smlnj.org/) uses heap-allocated stack frames.

## Deterministic deallocation

* Allowing programmable boundary of effects
* Allowing reusing of effects as first-class objects

This is somewhat conflict to PTC because side-effectful deallocation may break the assumptions of tail contexts, but users can still have chances to specify which one is needed when they are both supported.

C++ is an example supporting such feature by destructors. Another similar example is [D programming language](https://dlang.org/).

[RAII](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) is enabled by deterministic deallocation on first-class objects. This is an idiom significantly eases resource management in real programs.

[C#](https://en.wikipedia.org/wiki/C_Sharp_%28programming_language%29) and [Visual Basic .NET](https://en.wikipedia.org/wiki/Visual_Basic_.NET) support deterministic deallocation via the idiom based on [`IDisposable` interface](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=netframework-4.7.2), but this does not support RAII in general because it does not support the effects of deallocation to be propagated with first-class objects. They can support a weaker variant locally: `using` statement.

[Clang and GCC "cleanup" extension for C](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization#Clang_and_GCC_"cleanup"_extension_for_C) is also a form of deterministic deallocation limited for local variables.

### Necessity

A language lacking deterministic deallocation may require [GC (garbage collection)](https://en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29), if it does allow allocation of resources (so it can be a general-purposed language) and it does not avoid collection at all (to avoid leaking the allocated resources totally). GC can be deterministic with reference counting, but it cannot handle cyclic references properly. Tracing GC is not deterministic. This makes it in general not fit for resources other than memory and there have to be different mechanism to handle different kind of resources, like finalizers, which may lead to complex problems like [object resurrection](https://en.wikipedia.org/wiki/Object_resurrection). Even for memory, it will leak if not called in time. Most garbage collectors have STW (stop-the-world) problem on collection, and they are often not friendly to memory efficiency and real-time applications by default. Thus, GC is better opt-in for a general-purposed language.

# Avoidance of mandatory

Mandatory of some features may be problematic in general, although they can be opt-in. They are listed in following subclauses.

In general, making some features to be *derivative* (as *derivations*) in libraries (rather than being *primitives* specified by the core language rules) is beneficial for various reasons.

* There are more than one styles of primitives for a specific but not precisely-defined functionality. Each has pros and cons up to the specific target domains. Fixed design of primitives can be easily harm to the general-purposed property of the language.
* Fixed primitives design may rely on other language features (like complex type systems) too eagerly. The dependencies are not easy to eliminate in practice, and it is particularly annoying when different pieces of the specification clash due to the premature complexity in developing a language.
* On the other hand, derivations in libraries can be tested with different designs for experience on different domains, leaving the freedom of choices to the users.
* Derivations are immune to the overhead and complexity (both in the language specification and use cases of userland) when they are not used.
* Derivations may be developed and verified separately without interfere compatibility issues among language specification updates. This enables the ability of parallelism in development of the language design.

So better avoid a feature being ruled as primitives; otherwise, evaluate the gain vs. cost before introducing primitives to ensure such choice is indeed preferred.

Some other features are considered harmful because of introducing the unwanted assumptions and such assumptions are often unneeded restrictions. It is at least suspicious to consider them as "features" for a general-purposed language.

## Phases and stages

*Phases of translation* and *stages of execution* are concerned with language implementations, most defined in specification with styles of program transformation). They are not desired to be fixed in the specification in the very general sense.

Such rules distinguish different kind (most time, so-called "static" and "dynamic") of properties of program execution. This is not always desired, as it more or less prevents mixed static-dynamic features like [gradual typing](https://en.wikipedia.org/wiki/Gradual_typing) and advanced general implementation architecture like [supercompilation](https://en.wikipedia.org/wiki/Metacompilation).

A multi-phased design is always more complicated to a uni-phased design for a general-purposed language, whatever set of programming techniques it is targeting (or even when not for programming).

Moreover, separation of phases cause confusions. Informally, there [exists people](https://webcache.googleusercontent.com/search?q=cache:A-bM1xSkqUsJ:https://plus.google.com/+FrankAtanassow/posts/HB7tkcHgBxC+&cd=1&hl=en&ct=clnk&gl=us) who relies on separation of phases to express [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns). To be specific, the related original text is quoted here:

> If anything, I would like to see an even clearer distinction between these phases so that I can 1) automatically obtain or verify interesting information (like invariants) about a program without having to run (deploy) it, and 2) express interesting information (like invariants) about a program before I have to express details about what specifically it computes. In other words, I want to be able to read and write blueprints before I start building something (separation of concerns).

Both points do not work in general, because:

* 1) "To run" means *a priori* separation, as an explicit emphasized form of "runtime" phase in the lifetime the product.
	* Although this is not in the phases of *translation* in strictly sense, it is still largely redundant for similar reasons proposed here.
	* It is possible to eliminate this separated "run" phase by some real-time analysis which "runs" simultaneously *before deployment*. This *unified* approach should provide same (if not more) benefits for automatic verification.
	* The situation of separation vs. unified phases here is a bit like [AOT](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) vs. [JIT](https://en.wikipedia.org/wiki/Just-in-time_compilation) compilation. Both cases require some trading off to determine how much dynamic properties to be resolved later. However, while JIT does have some obvious weakness about the cost and quality of the result, it is not yet clearly known how unified approach is more deficient in practice.
* 2) Separation of concerns has essentially nothing to do with separation of phases in nature.
	* Separation of concerns is a principle towards better interface design *all around*, rather than something buried in subtle language-specific details to implement the principle. The required "distinction" approach will either leak abstraction based on assumptions not always available in the interface design or make it simply not feasible and implementable.
	* The original text is in a response to criticize the Kernel language, so it actually not only requires the *distinction* literally, but also requires it being more aggressively *built-in* in the language. Even distinction between different phases can convey the idea well enough, *separation* them with *globally* ordered subset of rules in the language is overspecified. In contrast, avoiding assumptions on phases *ruled explicitly by the language* still does not prevent *users* to introduce required distinctions *locally*.
	* Arbitrary distinction of phases is directly broken when a solution needs to express similar things across different phases, so any predefined phases will undermine the separation and/or code bloat (as similar code duplicated across phases).

Besides, bisection of static and dynamic phases is never guaranteed enough. For generative programming, particularly with multiple explicit stages, there is no absolute "static" or "dynamic" phases in exact one place of the pipeline. So, there can be more confusion. Ironically, [the one who wants separation of phases also declines the necessity of "dynamic" in a funny way](http://lambda-the-ultimate.org/node/1562#comment-18623). Such hostile attitude still cannot defeat the fact that something (e.g. user input) will not obey any "static" disciplines in some general problem domains, though.

On the other hand, to make the language "static" does have effect on the specialization, but it does not stop users to *use* it dynamically to eliminate the static restrictions. As said above, avoiding repeating same things across phases can be the motive of users in such cases (if they still stay with the language). Ultimately, all dynamic phases can be equal to no phases at all, or at least phases provided by users in DSLs. There is even no magic to prevent users doing so, before rendering the host language poor of features and largely useless.

### Examples

Both ISO C and ISO C++ specify phases of translation.

[Racket](https://racket-lang.org) has [compile and run-time phases](https://docs.racket-lang.org/guide/stx-phases.html).

[MetaOCaml](http://okmij.org/ftp/ML/MetaOCaml.html) facilitates multi-staging programming as the prominent feature, which assumes separately handling of staged code.

### Counterexamples

Kernel and [PicoLisp](https://picolisp.com) do not mandate explicit phases.

## Forms of translation

Some language like [Java](https://docs.oracle.com/javase/specs/jls/se11/html/index.html) specifies the translation as compilation (even now it has a [REPL (read-eval-print loop)](https://en.wikipedia.org/wiki/Read¨Ceval¨Cprint_loop)-style interface named [jshell](https://openjdk.java.net/jeps/222)). This is not desired in general.

## Concrete type systems

In essence, modeling type systems is one of the target domains of a general-purposed language. Any built-in complicated type system difficult to be model should be prevented.

### Explicit static typing

Explicit static type systems and typechecking rules shall not be built-in as primitive features because it relies on the "static" concept based on multiple phases of translation, which also limits the  reflective modeling of the type systems.

That said, static typing is still allowed, by implicit manners which do not expose the details of phasing, e.g. with type inference.

See also discussion [here](http://lambda-the-ultimate.org/node/2828).

### Mandatory typechecking

The typechecking is not allowed overall. It is users' freedom to derive the invariance kept by the type systems except the rules necessary to make the semantic model sufficiently meaningful.

Although the concrete judgement of qualification is subjective, but clearly, the requirement type system shall at least allow undefined behavior in the most general cases.

In this sense, it should be noted [the bottom type](https://en.wikipedia.org/wiki/Bottom_type) has special meaning on the purpose of the specification. It stands for "unpredictable (by design)" but not necessarily the "crashed" state (like [this](http://okmij.org/ftp/continuations/undelimited.html#introduction)).

## Restrictive computability

Properties like [totality](https://en.wikipedia.org/wiki/Total_functional_programming) shall not be mandated in the language directly, because:

* It directly restricts the computability of the language (the possible computability of any well-formed programs in the language) strictly less than the [most powerful computational models](https://en.wikipedia.org/wiki/Computability#Formal_models_of_computation) whose equivalences are known implementable in reality (namely, only less powerful than [hypercomputation](https://en.wikipedia.org/wiki/Hypercomputation)).
* There is no simple practical way to extend computability of a programming language without redesigning most parts of it.
* Providing both sublanguages to different choices of restrictions is redundant, because a general-purposed language shall already be able to allow users derive some features implementing the idea of enforcement (e.g. totality checking) with various computational effects.

Such properties may be beneficial or even vital in some problem domains like [automated theorem proving](https://en.wikipedia.org/wiki/Automated_theorem_proving) and the typechecking implementation of languages. This certainly does not cover the general cases where computation and programmability are needed. It should be possible to make some DSLs serving to such domains, with some derivation of language features to allow users enforce such properties, if needed.

Note mandatory of such properties usually needs to introduce some sorts of concrete type systems. This is also disallowed by related subclauses above.

## Various control primitives

Control effect shall be reified, not by providing more build-in control primitives.

### Sequential control

Sequential control forms, or *statements*, consist the imperative style of the control of programs. Nevertheless, it is not the fundamental one and it can be derived from primitives in existed calculi with (lambda or vau) *abstraction* constructs. The sequential semantics are essentially buried in the rules about order of applicative function calls, and there is no need to introduce a new special rule specific for this purpose.

### [Exception handling](https://en.wikipedia.org/wiki/Exception_handling)

Exception handling, at least synchronous ones, can [be formed with delimited continuations](https://gist.github.com/sebfisch/2235780), albeit many languages make it built-in for different reasons.

See [here](http://lambda-the-ultimate.org/node/2966) and [this paper](https://www.cs.bham.ac.uk/~hxt/research/exn-and-cont.ps) concerned with expressiveness of exception handling and undelimited continuations.

Native implementations of built-in exception handling may have ABI compatibility problems, e.g. [C++'s SjLj/Dwarf2/SEH models in MinGW](https://sourceforge.net/p/mingw-w64/wiki2/Exception Handling/).

### Examples

ALGOL-like languages have explicit semicolons (`;`) to provide the sequential control. It can be implicit in some languages (like [ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm) dialects). 
There can be other syntactic contexts for `;` with different meanings, so different replacements are needed (e.g. the `for` statement in C uses `;` for different meanings, and `,` operator is used for sequential effects in the condition clauses). The latter design is inconsistent in nature. C++, Java, C# and more C-like languages share the same design here.

Scheme's `lambda` expression has built-in sequential control implied by evaluation order among subexpressions in its body. The `begin` expression expanded from `lambda` uses this to express the exact control effect.

Many languages including ALGOL-like ones have iterative statement to express [loops](https://en.wikipedia.org/wiki/Control_flow#Loops), although they can be simulated by mutable variables and `goto` (if any).

Synchronous exception handling is built-in in Ada, C++, Java and so on.

### Counterexamples

The Kernel language does not rely on sequential control effects in its primitives. On the other hand, it has `$sequence` derived from `$vau` and various other primitives for the purpose. The `$vau` operator are then derived again based on the `$sequence` operator, giving the result has similar implicit   control effects in the body of Scheme's [`lambda`](https://schemers.org/Documents/Standards/R5RS/HTML/r5rs-Z-H-7.html#%_sec_4.1.4) expression. The derivation of `$lambda`, `$let` and similar constructs also keep the property.

[Haskell](https://www.haskell.org/) does not have built-in support of sequential control. On the contrary, the overall design is pure, and it is against control with specified order by default. Nevertheless, it still provides the [`do`-notation](https://en.wikibooks.org/wiki/Haskell/do_notation) [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar) based on [monads](https://en.wikibooks.org/wiki/Haskell/Understanding_monads) to model sequential effects interested in side-effectful programming. [Agda](https://wiki.portal.chalmers.se/agda/pmwiki.php) has `do`-notation and [desugaring](https://agda.readthedocs.io/en/v2.5.4.1/language/syntactic-sugar.html#do-desugaring) in a similar flavor.

Scheme does not have loops in primitive features. It encourages recursion (with mandatory proper tail recursion) instead. Kernel does the exact same.

## [GC](https://en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29)

See discussions in the subclause about necessity of deterministic deallocation above. This directly disallows relying on [tracing GC](https://en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29#Tracing).

GC also effectively encourages blur on object *ownership* and *access rights*, and further avoids using first-class objects as abstraction of resources whose relations of ownership can be naturally expressed as [DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph)s by succinct use of [RAII](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) idiom. Note in such cases, [cyclic references with equal ownership](https://en.wikipedia.org/wiki/Memory_leak#Reference_counting_and_cyclic_references) should be in general avoided by external owners or [weak references](https://en.wikipedia.org/wiki/Weak_reference). Someone may argue [this discipline harms idiomatic recursion use of closures](http://lambda-the-ultimate.org/node/5007#comment-81721), but the right of ignorance of ownership actually does not exist in nature: either it has to be managed by external owners implicitly (like a GC), or it has to be done explicitly. GC has indeed nothing to do with [closures](https://en.wikipedia.org/wiki/Closure_%28computer_programming%29) for historical reasons without the concrete background of some specific languages (which may sometimes imply the GC is always used). The so-called [funarg problem](https://en.wikipedia.org/wiki/Funarg_problem) can be resolved without aid of GC after the clarification of object ownership. (Actually, GC is the special case that uses the external owner.)

There are other implementation concerns to avoid general-purposed GC by default.

* Notably, GC often incurs memory consumption problem, that is, requiring [a lot more backing memory than the amount being needed](https://sealedabstract.com/rants/why-mobile-web-apps-are-slow/index.html).
	* Note this is actually the instance identified by "leak" as per the definition of [[Clinger98]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.83.8567&rep=rep1&type=pdf), being stricter worse than deterministic release of memory in block scope variables of ALGOL-like languages.
	* This increases the risk of [page faults](https://en.wikipedia.org/wiki/Page_fault) in modern systems, which is even worse for performance.
* Many GC incurs the [STW (stop-the-world)](https://en.wikipedia.org/wiki/Tracing_garbage_collection#Stop-the-world_vs._incremental_vs._concurrent) problem. This can seriously degenerate responsibility of applications by poor latency and causes bad user experience in cases of interactive applications.
	* Generational or incremental GC may relieve the problem, but not totally avoid. And the complexity of GC implementation can increase a lot.
	* Most concurrent GC implementations have no better situations except that the stop time would be less. But the complexity can be even more.
	* The [C4](https://www.azul.com/files/c4_paper_acm2.pdf) claims it can totally avoid the stop. However, it is only meaningfully available on a system equipped with huge amount of memory (say, several TBs in one instance).
* These performance problems make it not suited for most real-time or low-latency applications. Although in theory GC is not necessarily inconsistent with these requirements, it is already quite hard to implement. With limited memory resources, this is even harder.
* Some languages with need of "system programming" like C, C++ and Rust avoid GC by default. In particular, (general-purposed) GC violates [zero overhead](https://webstore.iec.ch/preview/info_isoiec18015{ed1.0}en.pdf) principle in C++, and similarly, [zero overhead abstraction](https://blog.rust-lang.org/2015/05/11/traits.html) in Rust.

Note the discouragement of GC does not cover the following facilities.

* Specific resource management schemes sharing some properties with GC, like resource pools (esp. [memory pools](https://en.wikipedia.org/wiki/Memory_pool)), are recommended as the replacement of GC for specific resource usage patterns.
* Transformations based on analysis of resources usage include [escape analysis](https://en.wikipedia.org/wiki/Escape_analysis) and [region inference](https://en.wikipedia.org/wiki/Region-based_memory_management#Region_inference) (sometimes referred as the _static GC_).
* Deterministic local collectors can be used to suppress leaks further than ALGOL-like block structures (see [[Clinger98]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.83.8567&rep=rep1&type=pdf)), including the case of implementation of TCO without GC. 

## [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) dependency

Dependency on ABI in the language level may benefit developers in various ways.

* It provides simpler mental models of the implementation.
* It often grants users the direct access underlying implementations of the language.
* It can lead to easier interoperability with underlying environment.

However, it does not work in general without [leaks abstraction](https://en.wikipedia.org/wiki/Leaky_abstraction) of concrete details of underlying implementations which implies:

* It essentially undermines the ability of [abstraction](https://en.wikipedia.org/wiki/Abstraction_%28computer_science%29) provided a language and defeats the major purpose of having a high-level language.
	* A formal description of can be found [here](https://digitalcommons.wpi.edu/cgi/viewcontent.cgi?article=1042&context=computerscience-pubs). However, this is based on comparison between different formal systems. Without the choices of concrete designs, users are difficult to expect which abstraction is desired. So, the conservation way seems more feasible in practice: first being least dependent, then adding features required.
* It will introduce unnecessary dependencies of underlying implementations like machine ISA, whether it is used by the user at all.
	* This has security concerns, see [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).
	* There are more variants of the rule in different domains: [rule of least power](https://en.wikipedia.org/wiki/Rule_of_least_power) and [principle of least knowledge](https://en.wikipedia.org/wiki/Rule_of_least_power). However, the former is not respected directly here because it contradicts a general-purposed language design. The effect of the rule can be achieved by derivation of such a language and using the resulted DSL instead.
* It will depend on external design (like ISA), or worse, implementations without any detailed specifications can be verify, leading to more cost of development and compromised portability of source programs.
	* A specification of ISA is usually provided by the vendor implements it. This will also be the base specification to be extended. The latter can be across multiple ISAs, e.g. [Itanium C++ ABI](https://itanium-cxx-abi.github.io/cxx-abi/abi.html). Nevertheless, each ABI specification is less portable interested in source programs.
	* Multiple ABI specifications are *fragile* than a single language specification of the language. However, this generally does not mean a full language specification including the ABI specification will be better at all, because each update of underlying implementation relevant to ABI has opportunity to affect the specification of the language. This is not modular and impractical in engineering, unless there is only one vendor provides both of the language specification and the language implementation, which makes it suspicious on portability and general-purpose property.
* Assumptions of ABI dependency encourages some concrete deployment forms (like binary files) over others and eventually limits the way of development and the general-purposed property of the language.
	* This is even true when the language specification does not mandate it, but the users have abused it for a while. For example, the C and C++ languages (see also detailed discussions in the counterexamples subclause below).
	* As an instance of the effect, C and C++ source files are usually not tended to be transferred directly. Instead they are often [AOT-compiled](https://en.wikipedia.org/wiki/Ahead-of-time_compilation), withstanding a long time overhead each time, even though both language specification (ISO C and ISO C++) do not mandate they have to be compiled and there do exist at least [one industrial implementation of REPL](https://root.cern.ch/cling). The fact of high overhead of translation in most implementations makes the language less general-purposed, because it is not easy to apply in environments requiring fast deployment of source (e.g. in browsers). Even the specifications do not mandate compilation, most implementations would still do so; moreover, proposals and adoption of new language features have been mostly towards such use cases in mind, which harms general-purposed property even more.

### Examples

D [assumes flat memory spaces with specified machine width](https://dlang.org/overview.html#features_to_drop).

### Counterexamples

Both ISO C and ISO C++ lack specified ABI specification within the language rules. Only limited *linkage* support is mandated by C++. However, users may believe the false illusion that "C has defined ABI" and "C++ does not". This is perhaps caused by various reasons.

* Implementations of C are often treated as the *de-facto standard*.
	* Historically, C was targeting ISA-level "native" platforms. This is still true in most modern implementations.
	* Notably, hosted C implementations provide API majority operating systems (although all of the cases actually use C dialects rather than strictly conforming ISO C).
	* This often makes the implementation of C be the *de-facto standard* of "native" (binary) interoperability in each platform. Many other languages' [FFI (foreign function interface)](https://en.wikipedia.org/wiki/Foreign_function_interface) are also based on such ABI.
	* The ISO C standard library as well as POSIX extensions are often implemented as [libc](https://en.wikipedia.org/wiki/C_standard_library) provided by system vendors. There can also be system-independent implementations like [newlib](https://www.sourceware.org/newlib/). The name of libc is also used to be the "system" component or an extension of [architecture triplet](https://www.gnu.org/software/autoconf/manual/autoconf-2.65/html_node/Specifying-Target-Triplets.html) describing the platform configuration (the [system type](https://www.gnu.org/software/autoconf/manual/autoconf-2.65/html_node/System-Type.html#System-Type)) used in cross-platform development by GNU toolchains and similar implementations (even though there exist alternatives like [this](https://wiki.debian.org/Multiarch/Tuples#Used_solution)).
* A conforming C++ implementation is often a conforming C implementation. Many reasons make it a feasible and intentional choice.
	* Historically, C++ was designed based on C. ISO C code is still well-formed ISO C++ in many cases (although the languages have many subtle but essential differences in the very basic specifications, e.g. compound types in C++ vs. derived types in C). ISO C++ also mandates the "C" linkage, respecting the (linkage) compatibility of (binary) target files.
	* ISO C++ mandates (possibly modified) majority of ISO C standard library in its standard library. As a result, almost all practical ISO C++ standard library implementations depend on ISO C standard library implementation, which is either provided together by the vendor of the implementation or implied provided by the vendor of the system (and sometimes they are the same).
	* ISO C++ mandates more features than ISO C. The features may require ABI-level support in typical implementations.
		* Overloading and namespaces are usually supported by [name mangling](https://en.wikipedia.org/wiki/Name_mangling) on symbols in binary file formats. Implementation of C also uses similar (but almost always simpler) strategy in practice, but this is not mandated by the language specification.
		* ISO C++ does mandate more than ISO C, including the language support library within the standard library required by core language features (for support of exception handling, [RTTI](https://en.wikipedia.org/wiki/RTTI) and so on). These specifications make the runtime library is not avoidable in a complete conforming implementation in practice. The additional parts are usually strictly more complicated than libc and are often the reasons of binary incompatibility besides name mangling.
		* These additional complexities may be worth having a standalone ABI specification like Itanium C++ ABI. Nevertheless, compatibility is still limited among different versions of the ABI specification, depending on feature addition of targeted versions of ISO C++.
	* ISO C++ did attempt to address underlying ABI issues, e.g. allocator pointers compatible to [both near and far pointers](https://en.wikipedia.org/wiki/Intel_Memory_Model#Pointer_sizes), but the effect was suspicious. On the contrary, ISO C does not do so, although there are separated technical reports like [ISO TR18037](http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1169.pdf), which are far less well-known.

## [Metaprogramming](https://en.wikipedia.org/wiki/Metaprogramming)

Metaprogramming is the programming mechanism and technique to manipulate a program by another program (called the object program and the meta program respectively). There are several motivations for this specific method of programming.

* Program transformers: in the general sense, this includes translators as the most implementations of programming language.
* [Generative programming](https://en.wikipedia.org/wiki/Automatic_programming#Generative_programming): automatically generation of the program code.
* Metaprogramming allows similar programming techniques used among different phases with more or less shared (or even the exact same) forms of the language constructs, rendering that only a quite small set of core features is need and *would* be supported by the same language.
	* This approach benefits greatly both generality and simplicity at the same time.
	* It makes learning the language features easier by promoting the coherence of the relationship between general purposes and general features. It also improves the utility of the efforts of learning, because the features can be heavily reused in a natural and uniform way.
	* With given benefits above, it is hardly feasible to be replaced by other means.
	* Nevertheless, not all styles of metaprogramming share these properties. Namely, it *can*, but not *must*, have the benefits. The fact suggests that such benefits are from some more basic elements of the language design, but not by the allowance of metaprogramming itself.

While the purpose is plausible, it is not the feature that a general language to support directly. The various implementation techniques (see below) can be broken down into more fundamental feature sets. They are hereby basically superseded by explicit `eval` style code of vau abstractions with more consistent rules for several reasons.

* The computational expressiveness is guaranteed by formal models (at least compared to macros). (Note [macros solely in C-like preprocessor itself is not Turing-complete](https://stackoverflow.com/questions/3136686/is-the-c99-preprocessor-turing-complete)).
* This approach also avoids need of predefined multiple phases of translation.
* This is clean and powerful in abstraction, particularly, the fact about no need to separate language features restricted in different phases which cannot be achieved in almost any other way (like candidates in the examples subclause).
* This is loosely coupled and mostly orthogonal with the design of the type system.

### Examples

[Macros](https://en.wikipedia.org/wiki/Metaprogramming#Macro_systems) are traditionally used to implement reflective features. They share similar pros and cons. See the specific subclauses below.

[Metaclasses](https://en.wikipedia.org/wiki/Metaclass) are generative reflection facilities which are also [proposed for C++]([http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0707r3.pdf]).

C++ templates are hacked to support some of reflective features like introspection. The abused [template metaprogramming](https://en.wikipedia.org/wiki/Template_metaprogramming) techniques are also studied and introduced by design in languages like D.

[Back quotes/quasiquotes](https://en.wikipedia.org/wiki/Grave_accent#Use_in_programming) in Lisp dialects are also considered an instance of template metaprogramming, which is different to C++ because the "template" is not a concrete language construct, but the list interacting with first-class macros (see below). Note this style is directly avoided by direct `eval` styles discussed above.

C and C++ have ability of [preprocessor metaprogramming](http://saadahmad.ca/cc-preprocessor-metaprogramming-2/). As the preprocessor macro plays the central role, they are also named "macro metaprogramming". However, the macro system in such languages are more restricted than Lisp dialects, the result is also restrictive, and the technique is far less used. There are various libraries specifically for the purpose: [Boost preprocessor libraries](https://www.boost.org/doc/libs/1_68_0/?view=category_preprocessor) (including [Boost.Preprocessing](https://www.boost.org/doc/libs/1_68_0/libs/preprocessor/doc/index.html), [Boost.Utility/IdentityType](https://www.boost.org/doc/libs/1_68_0/libs/utility/identity_type/doc/html/index.html), [Boost.VMD](https://www.boost.org/doc/libs/1_68_0/libs/vmd/doc/html/index.html)), [order-pp](https://github.com/rofl0r/order-pp), [Cloak](https://github.com/pfultz2/Cloak) and [components in P99](http://p99.gforge.inria.fr/p99-html/).

[Staged metaprogramming](https://en.wikipedia.org/wiki/Metaprogramming#Staged_metaprogramming) is available within several ML-like languages, which requires predefined *stages* (in turn, transformation across phases).

[A typed attempt](https://en.wikipedia.org/wiki/Metaprogramming#With_dependent_types) based on [dependent types](https://en.wikipedia.org/wiki/Dependent_type) is also considered in researching designs.

## [Reflection](https://en.wikipedia.org/wiki/Reflection_%28computer_programming%29)

Reflection is the special case of metaprogramming where the object language and the metalanguage are the same. It is also special that many languages have specific support (particularly, for *introspection*).

Although reflection is often cared only when there is the so-called "runtime" phase, it is essentially not restricted by phases.

As a special case of metaprogramming, the special-purposed feature is superseded as a side effect of the strategy to implement metaprogramming (see the discussion of related subclauses above).

### Examples

Some reflective Lisp dialects (see fexprs discussed above for example) have the built-in support of reflection features with similar power. Most reflective features in other languages are *ad-hoc*, namely, requires more special rules to interoperate with other features in the language.

[Many languages](https://en.wikipedia.org/wiki/Reflection_%28computer_programming%29#Examples) have reflection-based [introspection](https://en.wikipedia.org/wiki/Type_introspection).

## [Macros](https://en.wikipedia.org/wiki/Macro_%28computer_science%29)

Macros are general facilities to map from patterned input to output. In programming languages, they are features effective via *expansion*. As such, it is an important approach to implement macro metaprogramming.

Based on the reasons discussed in the metaprogramming subclauses above, macros should be not required by design because it introduce the necessity of phases of translation. If necessary indeed, they should be able to be derived by user code. See discussions below.

### [Hygienic macros](https://en.wikipedia.org/wiki/Hygienic_macro)

The Scheme language first introduced hygienic macros to overcome the interfered lexical environment problem by traditional macro expansion. There are more than one macro systems, see [SRFI-46](https://srfi.schemers.org/srfi-46/srfi-46.html) and [SRFI-72](https://srfi.schemers.org/srfi-72/srfi-72.html) for example.

Scheme-style hygienic macros has problems on phases, as well as problems like special treatments on the top-level environment. (Links TBD.)

Hygienic macros and other reflective features can be replaced by explicit `eval` style code of vau abstractions.

Macros without hygiene are not safe nor efficient to be used by default.

## Namespace separation

The namespace separation of values is the design to distinguish a term denotes callable entities (*functions*, or *combiners* in vau-calculi parlance) from others.

This perhaps first well-known by Lisp communities with [the article about *function cells* and *value cells*](http://www.nhplace.com/kent/Papers/Technical-Issues.html). It is interested because functions used as a first-class entity is concerned particularly by Lisp dialects and it involves very different looks in resulted idiomatic code. The separation is called "Lisp-2" for the purpose, and the remained dialects are instances of "Lisp-1".

Note the term "namespace" is not a language-supported entity here, but a mechanism in *name resolution*. The etymology is still plausible, though. See discussion about terminology below.

Separation of namespaces is better avoided for the general-purposed language design at least for 3 different reasons: simplification of the language rules in the specification about name resolution, simplification of interactions with first-class functions used in the code, and avoidance of confusion about duplicate name in same scope.

### Terminology

It is argued that ["Lisp-1" and "Lisp-2" are bad jargons](http://ergoemacs.org/emacs/lisp1_vs_lisp2.html). This is sometimes inappropriate.

> ¡°lisp-2¡± should be called multi-value-namespace languages.

> ¡°lisp-1¡± should be called single-value-namespace languages.

This is good and even better for non-Lisp languages... but lengthy. The "value" is also a source of imprecision when it is defined differently in the object languages.

> lisp-1 lisp-2 are Opaque Jargon

There seem no better choices in general, see below.

> Not a Important Language Issue

*(Editorial note: The title is kept to the original text.)*

Despite the value judgement of the opinion, the original text is based on the misconception about various distinguishing properties of the design strategy. Because the resulted code is quite different, it should not be easily ignored by users. The example used to neglect the significance (PHP) works because it is lacking first-class functions used as either Scheme or Common Lisp's ways.

> "The terms ¡°Lisp-1¡± and ¡°Lisp-2¡± were invented to expressly AVOID mentioning Common Lisp or Scheme, therefore we shouldn't use the terms such as Common Lisp model or Scheme model to describe it."

This is technically wrong because there are Lisp derivations neither dialects of Scheme nor of Common Lisp (see the fexpr discussed above for example), but they still share properties either with Lisp-1 or Lisp-2.

> Lisp does not use the term ¡°namespace¡± differently from other languages. The meaning is the same, only the implementation details are different.

This is true. However, there seems no better replacements in general, besides the Lisp-specific "cells", which is technically not even in the language specifications but left as implementation details. The variant of term "namespace" is also used out of Lisp dialects, see subclause of ISO C below. (Also note [Racket uses its namespaces to implement environments](https://docs.racket-lang.org/inside/im_env.html), which may be more proper usage of the original meaning.)

### Examples

ISO C has different *name spaces* (be careful, there is a space in the term) for *identifiers*. This does not cause Lisp-1 vs. Lisp-2 concerns because C does not have first-class functions. However, the complexity of rules is still existed, and the duplication of names may still likely introduce confusion if not carefully used. Declarations like `typedef struct name {...} name;` are often used to render the omission of `struct` allowed, but it is hard to tell readers whether or which is intentional by the code (depending on the attitude to [TIMTOWTDI](https://en.wikipedia.org/wiki/There's_more_than_one_way_to_do_it)), undermining the consistency of the code style.

ISO C++ merges *type-specifier* name separation by default but allowing exceptional cases via *elaborated-type-specifier* syntactic elements in a semi-compatible way to C, albeit with more complicated rules. This makes some code simpler (without need of `typedef`s) and reduces possible metaprogramming issues on type specifiers, although the complicated rules may lead to defects like [this](https://wg21.cmeerw.net/cwg/issue95). In all, the language itself is equipped with separation rules, but intentionally pretended to be a single-namespace one.

## Module systems

Modula systems are useful in organizing modular code components consistently. However, they can be derived from primitive constructs as libraries.

The way of derivation is considered superior than built-in primitive features for various reasons. The general reasons of primitives vs. derivations are all suited here.

For the case specific to the module systems, derivations are also allowed to be used as basic building blocks of more abstracted high-level facilities for engineering purpose (e.g. [building systems](https://en.wikipedia.org/wiki/Build_automation) and [CI (continuous integration)](https://en.wikipedia.org/wiki/Continuous_integration). A single fixed design of modules without multi-level reflective derivation in mind is hardly convincingly support the ideas well in general.

(Rationale and examples TBD.)

## Concrete arithmetic systems

[Arithmetic systems](https://en.wikipedia.org/wiki/Arithmetic) provide abstraction of numbers. They are useful for plenty of problem domains, but the necessity varies. Any such system shall be replaceable in some forms in a general-purposed language. While providing the default options may be justified, they *should* not be mandated normatively in the most fundamental piece of the language (the *core language*), including:

* Formal systems used to reason about the properties of a program in the language should not be mandated in the core language.
	* System of numbers are generally not needed to make the reasoning work.
	* System of some basic forms of arithmetic (like first-order [Peano arithmetic](https://en.wikipedia.org/wiki/Peano_axioms) and [Heyting arithmetic](https://en.wikipedia.org/wiki/Heyting_arithmetic)) renders unnecessary restrictions on the model without significant benefits.
		* Notably, [G?del's first incompleteness theorem](https://en.wikipedia.org/wiki/G?del's_incompleteness_theorems#First_incompleteness_theorem) shows that any [consistent](https://en.wikipedia.org/wiki/Consistency) formal systems containing elementary arithmetic are incomplete.
		* Further, [G?del's second incompleteness theorem](https://en.wikipedia.org/wiki/G?del's_incompleteness_theorems#Second_incompleteness_theorem) shows that such formal systems cannot formalize a consistency proof to themselves on the meta level.
* The system providing arithmetic operations should not be mandated primitively in the core language.
	* Despite the logical problems above, arithmetic systems have different difficulties on computation. No one is the choice fit to all problem domains, particularly when efficiency is taken into account.
	* Explicit encoding based on the calculi (e.g. [Church numerals](https://en.wikipedia.org/wiki/Church_encoding) encoded in lambda calculi) are general in inefficient. The [computational complexity], both in time, space and determinism, are poor for most practical use cases. So, such encodings are never enough. Although there can be mapping between the entities in such model and external entities of numbers, it is still suspicious to be worthy for a general-purposed language.
	* All other existing methods to implement them involve independent disciplines of internal encoding schemes (e.g. [floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic)).
		* Such disciplines expose many different concrete properties not needed by the problem domains (e.g. hard-coded representable numerical boundary and precision limitations) , which needs to be worked around further.
		* An improper model of numbers may introduce too many unnecessary dependencies difficult to correctly abstract away.
			* Notably, fixed-width machine integers enforce [explicit range limitations due to its representation methods](https://en.wikipedia.org/wiki/Integer_%28computer_science%29#Value_and_representation). Such imprecise to the mathematical numbers forces users to make assumptions on the values being processed in the programs unless treated extremely careful, which is error-prone.
			* Signedness of integers can [interfere typing](integer-type-reasoning.md). The choice of use may be far from idiomatic, depending on design choices of the language being used and communities of users.
		* An improper model of numbers makes it difficult to work portably.
			* For example, it is known that floating-point arithmetic systems may behave differently in different floating-point environments even within a single program.
			* Fixed-with machine integers have different representations of values. Though rarely occurred, mixture of them makes problems.
			* Dependencies on the assumption of [endianness](https://en.wikipedia.org/wiki/Endianness) is also problematic in general. Although this is essentially not specific to arithmetic systems, it is quite common in reality for representation of numbers.
		* Extensibility (of the extents) of most machine-oriented representative models is usually poor compared to the corresponding [mathematical notion](https://en.wikipedia.org/wiki/Number#Main_classification).

For arithmetic systems not in the core language, some points above are still suited. The [numerical tower](https://en.wikipedia.org/wiki/Numerical_tower) is suggested here to be the prototype for general-purposed practical use, as it avoids most shortcomings of the computational concerns.

# Why not ...

Here are some examples to illustrate how existing languages is not satisfying for the requirements. Each one should have one or more pros to be endorsed.

## A brief list of requirements

The following list are properties a needed language shall meet. Some languages superseded by them are also explicitly listed.

* (A) The language has a specification independently to the implementations. This makes it supersedes many toy languages.
	* (A.1) Further, the specification is normative. This makes it supersedes many "scripting languages".
	* (A.2) Further, the language has a formal grammar.
		* (A.2.1) Further, the language allows a free form grammar which makes the leading whitespaces insignificant in its lexical (source) form. This makes it supersedes languages like Python and Haskell.
		* (A.2.2) Further, the language allows identification of a well-defined context-free set from the rules, namely the syntactic grammar. This makes it supersedes ISO C (for context-sensitive *declarator* syntaxes) and ISO C++ (like ISO C and more on vexing parse).
	* (A.3) Further, the specification has a formal model to specify its semantics. This makes it supersedes most languages other than some Lisp dialects.
	* (A.4) Further, the specification of the program in the language is free from concrete translation forms. This makes it supersedes languages like Java (explicitly mandatory of bytecode "compiled to" the input of JVM) and PicoLisp. 
* (B) The language allows variables and procedural abstraction. This makes it supersedes languages like Brainfuck and Unlambda.
	* (B.1) Further, the language uses lexical scoping by default. This makes it supersedes to languages using dynamic scoping like PicoLisp and Emacs Lisp.
	* (B.2) Further, the language guarantees proper tail calls. This makes it supersedes to some languages impossible to implement it practically (like Java). This also makes it supersedes most other languages which do not mandate it (most languages other than R<sup>n</sup>RS Scheme, Kernel and a few more).
		* (B.2.1) Further, such tail recursive forms do not need specifically syntactic notation used in the source code. This makes it supersedes languages like PicoLisp and Clojure.
	* (B.3) Further, the language support first-class environments. This makes it supersedes most languages other than Kernel and PicoLisp.
* (C) The language can support various computational effects with first-class objects.
	* (C.1) The language is not free of side effects. This makes it superseded any "purely" functional languages like ML family (Standard ML, OCaml, F#, ...) and Haskell.
	* (C.2) Further, the language allows first-class side effects appertaining the use of first-class objects. This makes it supersedes most languages other than ISO C++, which allows side effects along with lifetime of objects in the language.
* (D) The language is homoiconic. This makes it supersedes most languages other than Lisp or Forth dialects.
	* (D.1) The language does not have phases limitations. This makes it supersedes recent Scheme dialects since R<sup>5</sup>RS Scheme.
	* (D.2) Further, it does not separate namespaces. This makes it supersedes Lisp-2 dialects like Common Lisp.
* (E) The language allows a clear DAG of ownership in resource management, which means it does not have features to necessity a GC in the conforming implementation. This makes it supersedes most languages other than ISO C, ISO C++, Rust and a few more.

## Kernel

It meets most requirements above but C.2 and E.

Comparatively, some languages fail strictly more:

* Scheme fails additionally at B.3 and D.1.
* PicoLisp fails additionally at A.1, A.3, A.4, B.1 and B.2.1.

## ISO C++

It meets many requirements above but A.2.2, A.3, B.2, B.3, D and D.1 (when D.2 is worked around by omission of *class-key* in some type names).

Comparatively, some languages fail strictly more with exception of A.2.2:

* ISO C fails additionally at C.2 and D.2 (on type names).
* Rust fails additionally at A.1.
* Java fails additionally at A.4, C.2, D.2 (on type names) and E, but not A.2.2.
* C# fails additionally at C.2 and E, but not A.2.2.

# Conclusions

While there are languages and implementations following criteria of one or more of the desired features above, no one have all of them. So, a new language is required.

Most designs also lack simplicity due to failure of avoiding the features in the avoidance list built-in. The new language should outperform these existed ones and leave the features not needed as general-purposed language away by design. For instance, language with only syntactic flavor differences should be derived (by hygienic macro expansion or vau abstractions, for example) from the "real" general-purposed language instead. This also renders the latter as a DSL, if not a dialect of the former.

