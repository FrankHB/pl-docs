# [科普][FAQ]MinGW vs MinGW-W64及其它

Created @ 2014-07-25, r2 rev 2015-09-15, markdown @ 2015-09-15.

　　部分参照[备忘录原文](https://bitbucket.org/FrankHB/yslib/src/50c3e6344a5a24b2382ce3398065f2197c2bd57e/doc/Workflow.Annual2014.txt?at=master&fileviewer=file-view-default#Workflow.Annual2014.txt-452)。

　　试试问答体。首先得绕个远路，从 Win32 开始说起，否则之后容易乱……

## 什么是 Win32 ？

　　嘛，32 自然是指 32 位了？不一定。

　　正式地说， Win32 主要是指跑在 Windows NT 内核上的 Win32 子系统。现在 x64 的 Windows 上的大部分程序也是跑在这个子系统上的，`system32` 目录也没叫成 system64 。

　　尽管 32 的语源的确来自于“32 位”。

## 那么为什么还有 Win64 ？

　　这倒可以肯定，这里的 64 是指 64 位目标平台，因为没有上面的那种歧义。

　　有一点值得注意，在 MSVC 中， 32 位环境（当然是说跑的 Intel 兼容CPU的PC）预定义宏 `_WIN32` ，但 64 位环境同时预定义了 `_WIN32` 和 `_WIN64` 。

　　顺便，通常 64 位主要指 x86_64 （微软称为 AMD64 ，这个兼容 x86 的基础架构一开始的确是AMD先搞出来的，后来才有 Intel EM64T ）。

　　64 位 Itanium 也有 `_WIN64` ，不过一般见不到且跟MinGW没什么关系且现在都不正式支持了，不管了……

　　对于 MinGW 来说，这里也有类似的坑：预定义宏得先优先检查 64 位的。实际情况更加复杂，另说。

## MinGW 和 MinGW-W64 有什么区别？

　　这是个关键问题，但是……是个很长的故事。没有铺垫不好回答。

　　首先， MinGW 是 GNU 工具（包括编译器 GCC 和 GNU binutils 和调试器 GDB 等）在 Win32 上的一个移植，是从 Cygwin 里 fork 出来的。当初只考虑 32 位。和 Cygwin 相比，不强调 POSIX 兼容性而相对强调性能和减小依赖。

　　具体来说 MinGW 除了以上工具外，还提供了一个适配于 Win32 的运行时环境。其中 C 标准库实现的二进制文件直接用微软随 Windows 分发的 MSVCRT 。 MinGW 自己的运行时库依赖于MSVCRT和其它系统库。

　　而 MinGW GCC 依赖于 MinGW 运行时以及 libgcc 和其它系统库。编译出来的程序一般也要依赖这些库，所以才会写死在默认 specs 里（可以用 `gcc -dumpspecs` 查看）免得用户随便编译链接个程序还得手动指定一大堆 `-l` 选项。

　　用三元组表示目标平台，当年的 MinGW 是指 i386-pc-mingw32 。这里 i386 也可以是 i486 等等……总之是 32 位 x86 指令集架构的名称；中间的 pc 可选，表示厂商名； mingw32 表示系统名。特别注意，事实上成为标准的“专有名词” mingw32 里的 32 是固定的；另外，所有这些大小写一般也是固定的。 GCC 等的源码配置里面也有硬编码进去。

　　然后，因为只支持 32 位，有人觉得不够用。这里的一个主要人物，就是现在 MinGW-W64 的主要维护者 Kai Tietz 。[因为工作需要重新实现提供 x64 支持的 MinGW](http://sourceforge.net/p/mingw/mailman/message/23108552/) 后提交到上游但[被拒绝](http://sourceforge.net/p/mingw/mailman/message/23100595/)，于是 fork 为单独的项目，这就是 MinGW-W64 的由来。

　　可见， MinGW-W64 和原版 MinGW 有所渊源，但是独立的两个项目。

　　W64 虽然用意是Win64，但是也算是个专有名词，在三元组里占据厂商名，例如常见的：i686-w64-mingw32。（在GCC源码的配置文件中， \*-w64-mingw32 和 \*-pc-mingw32 是分别对待的。）

　　MinGW-W64 是同时支持 32 位和 64 位的，甚至还支持 32 位和 64 位的交叉编译（启用 multilib 支持的 MinGW 发行版例如 mingw-builds 可以用 `-m32` 或 `-m64` 指定）。

　　显然， W64 和支持的架构无关。上面 i686 就不是 64 位的平台（而且可以看出这里的 32 也和架构没关系）。支持 64 为的对应三元组是x86_64-w64-mingw32。（另外 w32 是 GNU 惯用的对 Win32 的略称，也沿用到包括 MinGW 在内的一些项目中——如 [w32api](http://sourceforge.net/projects/mingw/files/MinGW/Base/w32api/) ，可能造成一些额外的混乱。）

　　……容易让人头疼的是，这两个项目现在都没死，偏偏还很容易因为这些字面上的原因搞错。为了下文描述方便，原版 MinGW 称为 MinGW.org 。

　　这里有一点非常重要：只有 MinGW-W64 是 GCC 官方支持的（尽管 [mingw32 平台是二等公民](https://gcc.gnu.org/gcc-4.9/criteria.html)）。 Kai Tietz 拥有 GCC 官方 repo 的提交权限。

　　所以，使用 MinGW-W64 的 GCC 一般比 MinGW.org 有更新更全面的支持，所以现在一般推荐 MinGW-W64 发行版。

　　说到这里……[维护 mingw.org 的 Keith Marshall 还和 Kai Tietz 等GCC官方人员在 bugzilla 上对噗过](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=52015) 。其中 Keith Marshall 对 MinGW-W64 使用 MinGW 一名造成混淆表示愤慨。嘛，这倒也是事实。

　　当然，也不是说 MinGW.org 就一无是处了。 \*-w64-mingw32 原则上向后兼容 \*-pc-mingw32 ，不过也有一些接口上的差别。 BSD 流的 `DT_*` 在 MinGW.org 上能用，在 MinGW-W64 的就没有。（虽然 `DT_*` 也不怎么推荐用就是了……）

## 什么是 MinGW 发行版(distribution) ？

　　这个说法习惯上可以说是从 Linux 等软件中借用过来的。

　　类似 Linux 内核，不管 MinGW.org 还是 MinGW-W64 ，本身都是相对集中于特定软件包（ MinGW 运行时库）开发的项目，并不着力于提供整个开箱即用的环境。

　　因此除了官方的一些编译版本外，有很多人基于 MinGW 运行时上进行定制封装供用户下载整个环境，有的还提供包管理服务等。这就是发行版。一般提供直接解压加上 `PATH` 就能用的环境和/或安装包。

　　早期比较著名的有 TDM-GCC 、 rubenvb 等。以前用的 MinGW.org ，不过现在主要转到 MinGW-W64 上来了。

　　比较新的发行版，一开始就着眼于 MinGW-W64 。最著名的发行版之一应该是 mingw-builds ，基本上近年来（ GCC4.7 以后） Windows 上能用支持最新版本最快的，支持交叉编译。

　　mingw-builds 一开始在 sf.net 上有[自己的项目](http://sourceforge.net/projects/mingwbuilds/)，不过后来表示要求加入 [MinGW-W64项目](https://sourceforge.net/projects/mingw-w64) 作为 official builds ，所以停更了，更新都在 MinGW-W64 里面，不过残念的是好像到现在 MinGW-W64 看来都不提供唯一的官方发行版，所以还是叫做 personal builds 。

　　另外提一下还有微软 VC++ 标准库（ Dinkumware 生产）维护者之一 [Mr.STL(Stephan T. Lavavej)](http://nuwen.net/stl.html) 个人的[发行版](http://nuwen.net/mingw.html) ，很早就在默认 specs 里加了 `-std=c++11` ， 而GCC 5 改用 `-std=c++14` 。（[官方 GCC 6 会用 `-std=gnu++14` 。](https://gcc.gnu.org/gcc-6/changes.html)）

　　还有 MSYS2 项目的 MinGW 发行版（这里可能有新的混乱，下文再说），也是 mingw-builds 一伙人搞的， 4.9.1 比 mingw-builds 更新还快几个小时。

　　其它发行版可以参照 mingw-w64.sourceforge.net ，更新相对没那么快。

　　最后，不嫌闲着蛋疼也可以自己编译。不过迫不得已外最好别这样做（ GCC 的编译过程和 hacking 实在无力吐槽）。重复一遍，非常不推荐。

## 上面为什么要强调更新呢？

　　如果不想使用新的特性生成更高质量的代码，其实也没必要盯着上面这么多版本混乱的 MinGW 了。即便要兼容性，也可以用古董嘛（逃……

　　对于 C++ 前端来说 MinGW 尤为重要，现阶段根本没有能顶替的。作为系统默认 ABI 新锐代表的 MSVC2015 ——前端还是残的……各种 bug 。

　　GCC 也有各种傻缺 bug ，不过至少在前端来说，程度上绝逼打不过 `cl` （ Microsoft C&C++ Optimizing Compiler ）。

　　VC++ 调试支持当然好得多，但是编译器一坑爹集成调试再好也没用了。

　　嘛， Clang++ ？ libc++ 什么时候能在 Windows 上跑顺再说——即便这样 MinGW 兼容的还是得依赖 MinGW 的 libgcc 。至于和 VC++ 兼容的 `clang-cl` ，看起来还在折腾微软的坑爹ABI黑箱（这没像大部分平台上 GCC 用的 [Itanium ABI](https://mentorembedded.github.io/cxx-abi/abi.html) 公开文档），一年半载别指望了。

## 什么是异常模型和线程模型，用哪种比较好？

　　这两个都是对于 C++ 实现（ G++ 、 libgcc 、 libsup++ 等等）而言的。

　　首先，异常模型。 C++ 标准没规定异常怎么实现。 MinGW GCC 使用的 Itanium ABI 也没规定必须怎么实现（但规定了一些公共接口），这部分由实现自行考虑。

　　GCC 一般提供了 SjLj （ C 的 `setjmp`/`longjmp` ）实现的 stub 。对于 x86 ，允许使用 Dwarf2 调试信息的实现。两者的区别在于 sjlj 比较通用，但是即便不抛出捕获异常而只是使用异常中立的风格隐式传递异常，也有运行时开销。而 Dwarf2 兼容性（考虑多层 C++ 和 C 的 DLL 互相调用来看）相对较差，但没有这种开销。

　　两者 ABI 并不兼容（知道 C++ 坑爹了吧，不仅不同实现不兼容，同一个编译器同一个平台自己都能跟自己不兼容……）——前者依赖类似 `libgcc_s_sjlj.dll` 这样的 DLL ，后者则是类似 `libgcc_s_dw2.dll` 这样的。旧一点的可能也没有这种后缀差异。

　　另外， libstdc++ 作为 C++ 标准库实现显然依赖异常，但名字一样的 DLL 可能依赖的不是同一种。所以混用多个版本 MinGW GCC 且没把 `PATH` 清理干净的时候容易出现找不到符号定义导致链接失败。这还不是最坑的，有时候 `gdb` 载入不同位置的 DLL 在运行时挂掉，还不只是一个 `PATH` 的问题……这种情况下先拿 [Sysinternals](https://technet.microsoft.com/en-us/sysinternals/bb545021.aspx) 的 [Process Exporler](https://technet.microsoft.com/en-us/sysinternals/bb896653.aspx) 之类的工具看看进程加载的 DLL 是不是预期的再说。

　　为什么说要有这么坑爹不兼容的，像 VC++ 一样用一种不就好了……其实 Win32 x86 上最理想的应该是和 VC++ 一样[基于 SEH （ Windows 结构化异常处理）的实现](https://en.wikipedia.org/wiki/Microsoft-specific_exception_handling_mechanisms)，但是 [Borland 关于这个的专利](https://www.google.com/patents/US5628016)才没过期几天……所以你懂的。

　　x64 上没专利的麻烦，有 SjLj 和 SEH 的实现，一般还是 SEH 。

　　第二，线程模型。

　　主要有两个， Win32 和 POSIX ，[对标准库线程的支持不一样](http://stackoverflow.com/questions/13212342/whats-the-difference-between-thread-posixs-and-thread-win32-in-gcc-port-of-windo) 。

　　Windows 线程 API 和 [POSIX(pthread)](https://en.wikipedia.org/wiki/POSIX_Threads) 有很大不同，而 ISO C++ 的 `std::thread` 为代表的接口是很接近 pthread 的。

　　所以在 libstdc++ 上实现这些接口，首先依赖的是 pthread 在 Win32 上的移植 libwinpthread ，也就是使用 POSIX 线程模型。因此发布的时候需要带上 `libwinpthread-1.dll` 之类的dll。

　　至于 Win32 线程模型， [GCC mailing list 是有提过](https://gcc.gnu.org/ml/libstdc++/2012-05/msg00020.html)，不过到现在还是没实现。也就是说 ISO C++ 的实现是残的，没法用。如果只打算用 Win32 多线程 API 倒是的可以用。

　　所以取决于具体需要。要兼容性好点的一般还是 POSIX 。

　　另外还有单线程的 single 模型…… Windows 上应该没啥必要用。

## 什么是MSYS，和MinGW有什么区别？

　　MSYS(minimal system) 原本是 MinGW.org 项目的一个组件，旨在 Windows 上提供一套类 UNIX shell 为基础的“系统”。它本身不提供编译器或者大小写敏感的文件系统支持（其实 [NTFS 倒是支持这里的“ POSIX 语义”](http://superuser.com/questions/364057/why-is-ntfs-case-sensitive)，但基本没看见有谁用……）。

　　和作为原生 Win32 程序的 MinGW 不同， MSYS 环境下编译的本机程序依赖于额外的特定的 MSYS 运行时，更接近 Cygwin （强调 POSIX 兼容性），会有性能损失（但一般意义上比 Cygwin 轻量）。对应的[三元组](http://sourceforge.net/p/mingw-w64/wiki2/TypeTriplets/)是 \*-pc-msys （通常其中的 pc 可以省略即缩写为 \*-msys ）。 MSYS 提供了一个 sysroot 环境（下面有 `/bin` 和 `/etc` 等），因此移植 POSIX 环境的程序一般更方便。

　　所以常规的实践是，如果只是开发 Windows 程序，能用 MinGW 就不要用 MSYS 原生的编译器来构建。当然，使用 MSYS 上的 `sh` 等工具还是没问题，跟 GNU 工具配套怎么说比 `cmd` 好用。（虽然也有不少琐碎的兼容性问题。）

## 什么是MSYS2，MSYS2上的MinGW发行版是怎么回事？

　　字面意思，MSYS 2.0 。比起 1.0 来说更加像 Cygwin （例如 `/etc/fstab` 配置）。项目[在 sf.net 上托管](http://sourceforge.net/projects/msys2/)。

　　其中的一个特色是基础系统附带 [ArchLinux](https://www.archlinux.org/) 移植的包管理器pacman，可以同时独立部署 `/mingw32` (i686-w64-mingw32) 和 `/mingw64`(x86_64-w64-mingw32) 下的开发和运行环境。注意和 mingw64 并列时 mingw32 自然指的不只是三元组的最后一项了。

　　下载依赖相当方便（就是没有靠谱的镜像时网速可能非常拙计）。具体使用参考 [Arch Linux Wiki](https://wiki.archlinux.org/index.php/Pacman)。对应的交叉编译环境在 Arch Linux 上也有[官方的包](https://www.archlinux.org/groups/x86_64/mingw-w64/)支持。

　　虽然 MSYS2 提供的 MinGW 上主要的编译器不直接支持交叉编译，不过可以同时装不同的目标平台的编译器（现在也有交叉编译器的包，没测试）所以不是什么问题，一定程度上比 mingw-builds 的 `-m32` 和 `-m64` 来说更加稳定靠谱。（现在也另外提供支持交叉编译的包。）

　　只提供 Dwarf2 异常模型和 POSIX 线程模型对于成套系统也不是什么大问题。包虽然比不上 ArchLinux 那么丰富不过常用的很多都有，免去自己编译的麻烦。打算长期使用 MinGW 和相关工具的，推荐使用。
　
　　虽然滚挂了也没多大事，不过版本是个问题。如果需要特定版本的 GCC 就不适用（比如规避GCC 4.9的坑爹bug……），除非有耐心自己找到 `.xz` 手动安装。

　　相关配置（包括 `pacman` 的一些中国大陆镜像）可以看[这里](https://bitbucket.org/FrankHB/yslib/wiki/Prerequisitions.zh-CN.md]) 。

## 部署程序需要提供哪些文件？

　　Windows 默认安装自然不带 MinGW 运行时环境，所以除了编译出来的程序和可能附带的数据，一些dll是要准备好的——除非有耐心折腾全部静态链接。

　　不同版本不同语言不同编译器编译出来的东西都不太一样。最简单暴力也可靠（？）的方法就是复制可执行程序到没装环境的白板测试机上看少了哪些东西（不过未必一目了然）。

　　简单可靠的方式是用 [Dependency Walker](http://www.dependencywalker.com/) 等工具查看依赖。

　　对于 C++ 程序，除非不用 POSIX thread 可以省掉 libwinpthread ，一般至少得确保上面异常模型和线程模型讨论中提到的三个 DLL （注意就算不显式使用标准库，编译器生成的代码也可能用到——典型的如默认 `::operator new` ，所以得带上 libstdc++ ）。

## 现在还有什么新坑？

　　就提一个GCC 4.9的问题。

　　GCC 4.9的 LTO （链接时优化）默认使用新的目标文件格式，生成的文件不包含冗余的二进制代码。
但是 LTO 有特定的 phase （[内部会调用 `make` 多编译几个 pass](https://gcc.gnu.org/onlinedocs/gccint/LTO-Overview.html#LTO-Overview) ），传统的静态链接器(`ar`) 不知道这里的约定，所以原来好好的东西，升级 4.9 以后开了 `-flto` 就可能找不到符号链接失败。

　　许多 MinGW 发行版仍然没实现 `gcc-ar` （运行会提示没支持 linker plugin ）。兼容旧版本的行为还得加上 `-ffat-lto-objects` 编译选项。

