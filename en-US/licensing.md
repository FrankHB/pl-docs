# Introduction

This is a memo about licensing affairs met in various works of the author.

**Disclaimer from the author to readers: [INANL](https://en.wikipedia.org/wiki/IANAL). Ask your lawers before you make decisions.**

# License and licensing

A [license](https://en.wikipedia.org/wiki/License) is an agreement on the licensed works for specific granted permissions. It is used to transfer the rights from the owner (the licensor) to the reciever (the licensee).

Licensing is a legal process to specify the license on some works to ensure that the licensee will have the legal rights to interact with such licensed works when the related laws are capable to grant such rights. The laws defining such rights include copyright laws, patent laws, and so on.

## How it works

Although laws may differ in various jurisdictions, many rights are protected similarly. This is implemented by widely adopted international conventions like [Berne Convention](https://en.wikipedia.org/wiki/Berne_Convention). Such laws often reserve the rights to limited owners exclusively by default.

Technically, a license is intended to be enforceable as a contract. Contract laws make the license clauses be legally effective and the limitation of exclusive rights can be bypassed. This fact makes it as a generally available instrument to grant such rights.

When the license is unavailable, infringement can easily occur. For example, with the jurisdiction in most areas in the world, users may not copy unlicensed software without a license or extra permissions by default. So, licensing should be substantial for the purpose of legitimation.

## Licensee

The licensee of a license can be specific or uncertain individuals or organizations whichever supported by the laws.

Both [EULA](https://en.wikipedia.org/wiki/EULA) and public licenses should be similar legal effects.

It is reported that [one of the mostly used free public license is an enforceable contract in US](https://www.theregister.co.uk/2017/05/13/gnu_gpl_enforceable_contract/).

# The rights

Widely adopted licensable targets include copyrights, patents and trademarks.

Sometimes the rights involved are collectively named as "intellectual property". However, there are [critisms](https://en.wikipedia.org/wiki/Intellectual_property#Criticisms) against the term of use. To avoid ambiguity, this article will avoid the use unless necessary.

## Copyrights

Copyrights are often the main targets to be covered by a license.

Historically, there are different views of emphasis on the concerned rights involved in modern copyright laws. The traditional Anglo-Saxon concept of "copyright" only deals with economic concerns, contrasting to the French "right of the author" (droit d'auteur). As one of the most important international framework of copyright laws, Berne Convention are influenced by both aspects.

### Copyrightable works

Not all works can be copyrightable. For example, mathematical formulae are usually not the objects and excluded from the copyright laws explicitly. In essense, the theory of programming languages and the spirit of the designs are also not copyrightable. However, documents about such designs are definitely copyrightable.

Not all rights are qualified to be the legal targets in the license. Licensable copyrights are essentially rights over properties. The rights of authority is stripped away from the allowed list of the rights.

### Copyright holders

[Copyright holders](https://www.csusa.org/page/Definitions#copyrightholder) are owners of copyrights. Note the ownership of the copyrights are separate from the ownership of the copyrighted works, so they can be different at the same time.

Except as otherwise specified by the laws, only copyright holders can ultimately decide which sets of the rights are included in the license.

There are some exceptional cases qualified as the "otherwise specified" above. Under some conditions, the copyright laws may provide sections to explicitly grant a relatively narrow set of the rights without permission from the copyright holder, like [fair use](https://en.wikipedia.org/wiki/Fair_use).

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

