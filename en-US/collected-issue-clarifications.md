# Collected issue clarifications

This document is a collection of points to some wide-spread writings with confusions, false statements or outdated information needing clarification known by the author. It can be referenced by other documents. For this purpose, the titles should be stable to be used as hyperlinks.

## Preprocessor identification

It is probably well-known that [musl libc will not provide feature macros identifying the implementation](https://openwall.com/lists/musl/2013/03/29/13), as stated:

>  It's a bug to assume a certain implementation has particular properties rather than testing.

This is very wrong and has challenges against the commen sense by obvious reasons in a wider view from both users and maintainers:

* Assuming a certain implementation has particular properties is legitemate and parts of common practice.
	* It is legitemate to have different configurations for different needs of users.
		* Such configurations may contain separated sets of features which are opted in.
		* Having assumption by nominal (i.e. explicitly provided by named feature sets or named library components) set of configurations from underlying implementations is the usual case in portable software development.
	* The identification is a well-established way to gain portability as possible in common practice, esp. for a library as libc.
		* Macros with prefix `__STDC` in the names are standardized in ISO C.
		* Various macros like `__GLIBC__` are used to identify the implementation of libc.
		* There is no sufficient reason for the well-known (and even standard) way to make them "bug".
	* The use of identification is also necessary for maintainers to implement the standard features with opted-in configurations.
		* For example, a libc implementation conforming to versions of POSIX.1 is likely to have different functions defined as per the value from the expansion of macro `_POSIX_SOURCE`.
		* As the author of the implementation, the standard ones `_POSIX_SOURCE` is no different to other macros from the environment, except that such macros are known to be user-provided externally.
		* Libraries like Musl libc have no practical alternative strategies to prevent meeting the need of such macros.
	* Even without the need for opted-in features, working around the defects in the implementations often need to identify the dependency of specific configuarations (including versions).
		* The workaround may be in the status of best effort. Unnecessary workaround may decrease the quality for other implementations.
	* Rejection of providing a way in this means undermines the portability and quality of implementation in practice (if not totally failed to provide them), because the user cannot detect the required information to distinguish the implementation reliably at all.
		* Either the implementation is unsupported, or it is in the `#else` at last, without the capability to be distinguished with other ones also having no idendification macros.
* Testing is a less standard way to the preprocessing macro identification, and often worse compared to alternatives for various reasons (see below), so it cannot replace the latter.
	* Identification using the preprocessor, in general, is testing during translation by the C (and others, e.g. C++) implementation, which is almost the most portable form of testing during the build.
	* Testing other than identification can be often less reliable.
		* They may have significantly worse properties by having false positive and false negative cases.
		* Compared to identification, they may be more costly, having more complexity to implement, more difficult to port or reuse in practice.
	* Some properties of programs may be undetectable at all in theory.
		* See [Rice's theorem](https://en.wikipedia.org/wiki/Rice%27s_theorem) for the general case in the sense of computability.
		* Some properties in object language level (i.e. the program code), cannot be reliably covered for the state spaces (a formal parlance of the set of test cases) by normal execution or simulation, instead of formal verification in the meta language level (i.e. the semantic rules of the object language expressed in some formal languaged).
			* A canonical example is [PTC](https://www.geeksforgeeks.org/what-is-a-proper-tail-call/). The reason for failure on the coverage by testing is due to the physical infeasibility to construct an environment having unbounded resources for the activation records.
	* Practical computational resource boundary (e.g. limited time) may apply (esp. for runtime testing).
	* Runtime testing, even feasible, may often have unnecessary performance impacts, even for normally use cases for end-users which have no interest in the topic here at all.
	* Configuration-time is out of the standard semantics of programming language implementations, so it can be unavailable at all.
		* Even when available, it requires a host environment which requires more assumption on host implementations (e.g. operating systems and shells), hence less portable.
* There are many practical examples showing that lacking identification making less reliable and portable software, as well as making the development experience worse.
	* The earlier versions of libstdc++ cannot be detected uniformly, because `__GLIBCXX__` can have versions depending on the distrobution of the GCC package.
		* Only [the official (FSF) releases have a well-known mapping from the date to version](https://gcc.gnu.org/onlinedocs/gcc-14.2.0/libstdc++/manual/manual/abi.html#abi.versioning.__GLIBCXX__), and it is still quite inconvenient.
		* To solve the problem, [since GCC 7 there is also a `_GLIBCXX_RELEASE` macro](https://gcc.gnu.org/onlinedocs/gcc-14.2.0/libstdc++/manual/manual/using_macros.html).
	* Any bug in musl libc will make the detection necessary.
	* Here is [an article](https://catfox.life/2022/04/16/the-musl-preprocessor-debate/) discussing how the identification can be useful with some detailed conditions showing that lacking identification makes trouble.
	* Here is [a question](https://stackoverflow.com/questions/58177815) showing the existence of real need of detecting musl libc.

That said, the maintaince of a versioned idenfication in the source code can be more costly, and a software component can have less need of such identification for various:

* The component is a module in a larger software component, and it will be upgraded together with the module, so an individual idenficiation can be redundant.
	* In such cases, the addtional identification may even be harmful due to confusion.
* Users agree that alternative identification or detection method (like testing) is sufficient, when:
	* The component is deployed in some controlled or private environment.
	* The component provides its API design and implementation together and there is no need to distinguish different implementations (opposed to the libc cases which are the implementations to ISO C and POSIX.1), so it will have few portability impact.
		* Version detection may still be useful.
		* Runtime detection (by providing specific APIs) may suffice for the purpose of version detection which can be configured by build command (e.g. something like `-DVERSION=1` in te GCC command line) instead of updating the versions in the source code regularly.

However, neither of them applies to musl libc. Lacking the identification is of a problem of poor QoI and the decision to reject the identification is a bad idea of design in practical software engineering.

