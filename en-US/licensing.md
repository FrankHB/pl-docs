# Introduction

This is a memo about licensing affairs met in various works of the author.

**Disclaimer from the author to readers: [INANL](https://en.wikipedia.org/wiki/IANAL). Ask your lawers before you make decisions.**

# The laws

Licensing is a process to specify the license on some works to ensure that the licensee will have the legal rights to interact with such licensed works when the related laws are capable to grant such rights. These laws include copyright laws, patent laws and so  on. When the lecense is missing, these laws apply and the intended users of the license may not have some of legal rights accroding to the fallback clauses in these laws. For example, with the jurisdiction in most areas in the world, users may not copy unlicensed software without a license or extra permissions by default. So, licensing should be substantial for the purpose of legitimation.

Licensing works with copyright and other laws which defines the involved rights. When it is unavailable, it may be also be used as a contract. This fact makes it as a generally available instrument to grant such rights.

# The rights

Sometimes the rights involved in licencing are collectively named as "intellectual property". However, there are [critisms](https://en.wikipedia.org/wiki/Intellectual_property#Criticisms) against the term of use. To avoid ambiguity, this article will avoid the use unless necessary.

## Copyrights

Copyrights are the main targets to be covered by a license.

### Copyrightable conditionals

Not all works can be copyrightable. For example, mathematical formulae are usually not the objects and excluded from the copyright laws explicitly. In essense, the theory of programming languages and the spirit of the designs are also not copyrightable. However, documents about such designs are definitely copyrightable.

Not all rights are qualified to be the legal targets in the license. Licensable copyrights are essentially rights over properties. The rights of authority is stripped away from the allowed list of the rights.

### Copyright holders

Except as otherwise specified by the laws, only copyright holders can ultimately decide  which sets of the rights are included in the license.

There are some exceptional cases qualified as the "otherwise specified" above. Under some conditions, the copyright laws may provide sections to explicitly grant a relatively narrow set of the rights without permission from the copyright holder.

### Derivation works

### Copyleft vs. permissive licenses

## Patents

## Public domain

## Sublicensing vs. relicensing

The term [sublicensing](https://en.wiktionary.org/wiki/sublicense)" is used to describe the act to license to third parties. Do not confuse it with relicensing (e.g. [software relicensing](https://en.wikipedia.org/wiki/Software_relicensing)), i.e. changing the existing lecense of the licensed works. The latter is mostly preserved to the holders, because arbitrary abilities of relicensing can make the license practically become the declaration of public domain.

# License compatibility problems

# History and philosophies

## The free software movement

## OSI definitions

## Policy differences

# Other media and license frameworks

## Creative Common

## Doujin works

# Examples of choices

## Copyleft licenses

## BSD vs. MIT

Strictly speaking, [BSD](https://en.wikipedia.org/wiki/BSD_licenses) and [MIT](https://en.wikipedia.org/wiki/MIT_License) are not good names to identify licenses. This make problems particlarly with [BSD](https://www.gnu.org/philosophy/bsd.html) because the versions are confusing. Each version has different conditions to meet. MIT also makes problems because there are at least 2 variants (X11 and Expat) widely used. Such issues are actually [more problematic](https://github.com/licensee/licensee/issues/98) when more variants are considered.

As stated in the link above, the original 4-clause BSD license has problems of verbosity and compatibility, it should not be used any longer in practice.

Conceptually 2-clause BSD license (the FreeBSD license) can derive 3-clause BSD license (the NewBSD license) and Expat license can derive X11 license in the same way (by adding the non-endorsement clause). The non-endorsement clause is not necessary since the [purpose](https://softwareengineering.stackexchange.com/questions/41128/what-is-the-purpose-of-the-non-endorsement-clause-in-the-new-bsd-license) (protecting against the endorsement) can be achieved by other means more effectively (e.g. by fraud law). So, only the former ones are compared below.

Most rights granted by 2-clause BSD license vs. by Expat license are same.

* Both grants permissions on use, redistribution, redistribution with modification.
* Both requires to retain the copyright notice and warranty disclaimer.
* Both are not copyleft.
* Both have no patent clauses.

While the license text seems more succint in the latter, here are some other detailed differents:

* The copyrighted works differ. The term of SOFTWARE in Expat license are considerably restrictive than it in 2-clause BSD license. The notion does not cover some other kinds of works, e.g. hardware design in the form of [Verilog](https://en.wikipedia.org/wiki/Verilog) code.
* The conditions differ.
	* 2-clause BSD license allows slightly different conditions on the source code and binary form of the software. Expat license makes the same conditions for the source code and the binary form of the software.
	* 2-clause BSD license enforce more explicity the reproduction of the disclaimer clause and the credits.
	* There are extra permissions (merging, publishing, sublicensing and selling) specified by Expat license explicitly. However, all can be derived from the text in 2-clause BSD license.
	* Expat license insists on "copy" of the license text. Although this is weaker compared with "verbatim copy" wording in licenses like GNU GPL, it is still more restrictive. On the other hand, under BSD-like license, the licensee is permitted to "reproduce" the clauses in other forms, for example, audio files containing the content of the license. The downstream sublicensed licensee can transform such form back to the text.

Provided with the differences above, the former is considered better for general use in the opinion of the author of this article.

