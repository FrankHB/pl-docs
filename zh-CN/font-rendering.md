﻿# 字体渲染

　　很久之前就有计划总结关于一些字体渲染(font redering) 相关问题的一些意见，不过因为[话题过大](https://zh.wikipedia.org/zh-cn/%E5%AD%97%E4%BD%93%E6%8E%92%E5%8D%B0%E5%AD%A6)，一直坑着。（更大的一个话题是关于广义的“文字”的渲染，至少还包括整段文字表现的排版和复杂样式以及编码上的实现的一摊子烂账，这里先整体排除，只在必要时论及。）

　　这里的问题首先是关于用户角度“设计”的机制问题。具体来说，这样的设计，解决的问题——字体渲染——的预期结果（[字形(glyph)](https://zh.wikipedia.org/zh-cn/%E5%AD%97%E5%BD%A) 的可视化），应该在最基本的用户需求出发的意义上遵循的原则。因为一些原因，这是很多用户和设计者都会忽略的浅显问题。考虑到其中包含了和许多其它设计——包括程序语言——一样的问题，因此收录于此。

# 需求

　　在针对具体问题展开讨论之前，有必要认清楚，字体渲染（确切来讲，单指呈现在预期最终用户眼中的字形的可视化）包含的外延。简单来讲，这里包含两种情形：[印刷(printing)](https://zh.wikipedia.org/zh-cn/%E5%8D%B0%E5%88%B7#%E5%8D%B0%E5%88%B7%E7%9A%84%E5%8E%86%E5%8F%B2) 的效果和在普通显示设备（如显示器或者移动设备的屏幕）上的[光栅化(rasterization)](https://zh.wikipedia.org/zh-cn/%E5%AD%97%E4%BD%93%E5%85%89%E6%A0%85%E5%8C%96) 。

　　这两种场景有一个最重要的共性：有效地传递字形的视觉信息。这其中，最主要的原始需求是传递的文字的内容。这种场景的最终用户是文字的读者：通过接收字形的图像，转换为抽象的图形而解析出文字，以获知文字传达的内容。这种场景下，大多数读者是人类；近年来也有如[光学字符识别](https://zh.wikipedia.org/zh-cn/%E5%85%89%E5%AD%A6%E5%AD%97%E7%AC%A6%E8%AF%86%E5%88%AB)这样模仿人类读者的图像处理技术来自动地完成和人类阅读文字材料时相同的图像处理活动。这种场景下，字形的可视化结果被作为输入的图像；满足这类需求的输入质量的特性，首先是**足够清晰**，其次是**允许足够高效**；其它和文字内容无关的特性并不能直接影响读者成功有效地识别图像。

　　与此相对，把字形作为印刷或光栅图像传输，在现代的技术上并不是什么问题，但这满足的是另一种不同的原始需求。这种需求在一定意义上是和识别图像为文字的上述场景是矛盾的，它要求的首先是作为可视化结果的图像与某个参考模型相比**不失真**。这种模型可能是字体设计者定义的，也可能是更加主观的判断标准；因此，看到具体的模型之前，相对来讲并没有那么绝对意义上正确性。

# 人的实现策略

　　许多情况下，上述的两种需求在一般的人类读者面前差距并不明显，因为基本上能识别图像并正确转换为文字的读者，也能大体上分辨字形应该具有什么视觉上的结构才能算是满足基本要求——例如，笔画应当呈现清楚而不和其它相似的字体混淆这样不同需求中共同的特性要求。但是，这并不适合所有情况。对诸如字体设计之类的专业场景的要求来讲，读者对图形的处理逻辑可能全然不同。因为若要追求可视化效果和预期的模型尽可能一致，只要有清晰的模型图像甚至抽象的图形，都不需要了解其表达的文字是什么。这种活动本质上不要求读者理解文字是什么（只要能分辨具体不同的字形就可以），是书面语言中立的。

　　因为这些差异的存在，对人来讲，可以有不同的机制高效地处理问题。习惯阅读文字（不用说是专门训练过速读的读者）之后，读者对具体字形的细节差异往往趋向于普遍的不敏感——只要不造成误认别字而造成文字识别错误就可以。更有甚者，对整句和整段落的文字也可以有更激进的特征识别优化，乃至许多情况下存在“汉顺字序不影响阅读”的现象。这种现象也存在于其它书面语的视觉表现上，可以认为是语言中立的。这种优化和处理器前端一次解码多个指令的直接并行有类似之处，但排除单位时间效率问题，实际上更加强大——和文字的语义处理也是流水线并行化的，以至于只要顺序不是错得太厉害到怎么整理都不大可能有意义，人脑总是能够纠错。反之，要求字形和模型匹配的场合，本质上注重的是图像的相似和一致性，并没有经过这层深入图像到文字的识别甚至深入文字的语义的处理，是相当不同的活动。

# 歪门邪道

　　上述的两个需求和存在不同的合理用例和相当不同的实现，是应当自然地并存的。但是，在如何评价字形光栅化效果的优劣的原则问题上，有的用户不顾这种事实，武断地认为“还原”印刷效果的光栅化总是更好的，凡是和印刷效果不一致的问题，总是光栅化的设计错误。这种观点扼杀了大部分人对不同需求的自然的实现方式，要求不同意见的（应该是这个世界上的大多数的）用户屈从个别的、欠缺理智判断和分析能力的**教条**（据说史蒂夫·乔布斯以设计师的角度鼓吹这种过，暂时没找来源确认），抹杀了用户正常的不同需求表达和选择的自由，因此是一种**歪门邪道**。

　　这里正面点出的问题在逻辑上应当是很清楚的——尊重不同的容易理解存在意义的合理需求的设计，理应是所谓“正确的事”([the-right-thing](https://en.wikipedia.org/wiki/Worse_is_better#The_MIT_approach)) 。但是，许多用户并没有这样完整地分析过问题的来源，也不知道理所应当的问题，单纯以立场出发讨论，而在某些用户群体中引起了不必要的口水战。

　　这个问题的例子集中体现在[一个标题为《为什么 MacType 这种「邪门歪道」的软件会出现并流行？》知乎问题](https://www.zhihu.com/question/49223658/)上。原话“歪门邪道”，出自一个回答：

![据信 Micorsoft Windows 字体渲染相关开发者的回答](https://pic2.zhimg.com/f4fe52c0b58e5370f46df27fbc334821_b.png)

　　这里的含义并不清晰，有回答者概括了两点可能的缘由：

> * 不惜牺牲性能（当然这点性能损耗在今天不算什么），乃至冒着影响稳定性的风险也要 hook Windows 本身的字体渲染机制这种行为，是邪魔歪道；
> * 放着清晰的字体渲染风格不用，偏要去模仿 Mac 的模糊的风格，是邪魔歪道。

　　相比之下，毫无疑问的是，MacType（原名 GDI++ ）干预 Windows 字体渲染效果的做法是一种 hack 。这种 hack 的性能在现在来看普遍认为不会造成很大问题，但是稳定性仍然受到质疑。有评论指出“sb2装个mactype直接扑街”，估计指的是 [Surface Book 2](https://zh.wikipedia.org/zh-cn/Surface_Book_2) 。我不理解为什么分辨率 3000×2000 的屏幕需要特别去试 MacType 的效果，使用同款设备也不愿意冒风险验证，这个毕竟还是是主观偏好的问题，暂且按下不表。这里更重要是第二点——隐含“模糊的风格”的不赞同。不过，在展示为什么 MacType 渲染效果（在一些情况下）更好之后，该答主的理由有一些微妙的不同

> 我又直观地体会到「天呐，mactype 就是正义」\
> 以上只是一个例子，当有些软件在日常使用中不可代替且在字体显示上也存在不能忍的问题时，用 mactype 改善体验算是成本最小的途径了。对于用户，使用「邪魔歪道」的 mactype 是为了解决更「邪魔歪道」的问题。\
> **对于用户，使用「邪魔歪道」的 mactype 是为了解决更「邪魔歪道」的问题。**\
> Win10 RS1 都出了，为什么电脑字体不能像手机一样看的舒服。为什么我需要关心这个程序是 GDI 渲染，那个是 DirectWrite 渲染的，是否需要替换为没有 hinting 的字体，或者 @Belleve 改的字体。对于一名用户，在意这些已经够累了。至于**稳定性**，大概已经忘了这个词吧。


　　从用户的角度来讲，Microsoft Windows 缺乏技术上本应不难实现（考虑到至少基于 MacType 和其它系统普遍使用的 FreeType 都能实现），而能够改善用户体验的**可配置性**的**功能特性**，而又偏偏不实现这点，的确相当不友好。但是，与其说是歪门邪道，更恰当的评价是，（比起 MacType 这样的魔改都能做出更符合用户预期的方案来讲的）**无能**。虽然本质上只实现有限的方案并给配置制造麻烦同样是不尊重某些用户的需求，但比起**指鹿为马**地以**覆盖其他用户的需求**的形式制造祸端，仍然不是同一个级别的问题。

　　相比之下，另一个回答者更加清楚现实：

> 以上所说的所有问题，在MS那里，估计永远都不会正式提上议程。因为所有这些都不是Windows广大用户群体所关注的问题，估计放到知乎以外，都没有多少人知道咱们讨论的是什么。所以期待官方解决方案，实在是Naive.

　　按常理想，（即便只是潜意识地）区分清楚两种不同需求解决方案的上述“其他用户”显然占多数。但这更加说明，个别需求蕴含默认策略的不合理性——强求一般用户**总是默认**以图像的方式理解文字的可视化结果，本身就是**不合理**的。

　　作为满足小众用户需求的 MacType ，本身当然不会因此（有资格）成为歪门邪道。这样的小众需求，如果排除对其他用户选择的强迫，仍然可能是合理的，因为：

* 实现这种需求的操作的合理性已经从可行性和一致性（无矛盾性）中蕴含。即便小众用户因为个别的习惯甚至适应性缺陷（以至于不合时宜的习惯变成了明知问题却仍然被滥用的**瘾**）而选择了对其他多数读者更低效的实现，可以看出这并不妨碍他们解析文字和阅读理解文字的含义。对他们来说，这是合理的选择，这种事实判断不因其他多数人愿意选择的习惯而改变。
	* 但厂商是否愿意接受这种习惯为默认情况是另一回事。厂商因为不愿意推出**默认**支持这种方案的配置而被排斥，这样的理由一种**歪门邪道**。
* 是否适应字体可视化效果，很大程度上由用户的眼睛是否能够适应一定时间的阅读有关。不管是作为纯粹的图像还是抽象的图形，用眼疲劳会阻碍阅读是共通的事实。而是否引起视觉疲劳和用眼习惯以及环境相关，这并不是简单能一概而论的问题。
	* 以个人经验看，我明确认为字体普遍倾向不模糊的 ClearType 才是可接受的较长时间用眼负担较小的默认方案，因为模糊的文字会潜意识地动用睫状肌试图矫正边缘不够清晰的问题，使眼部负担明显增加而有效缩短可用时间。（这是假性近视患者普遍会加深病情的危险操作，然而散光到一定程度以及真性近视或许就没那么严重的后果。）然而若换上较不常用的深色背景和浅色文字，则 ClearType 和 MacType 风格的眼部不适的差异要小得多——这看来是后者的边缘模糊效果差异也小得多，而不需要动用潜意识的模糊的关系。
	* 反之，忽略这个问题而无意义地给用户增加用眼困扰的，不管是出自无知还是迷之自信，都更接近**邪门歪道**。
* 如之前提到的，Microsoft Windows 的在字体渲染的可配置性上的确欠缺功能，不限于默认配置的设计合理与否。而如一些回答提到的，不提供灰度以及 Y 轴子像素反锯齿，在 Windows Vista 后默认强制使用的 ClearType 的问题，确实是实际存在的并且很可能加深视觉疲劳的真实设计缺陷。就从允许自由选择和微调效果使之更好的意义上，MacType 提供一个可被最终用户容易实现的变通，这是合理的。
	* ClearType 需要配合字体优化才能实现更合理的效果，这在工程上有更大的工作量而更难实用。但是，技术上讲，全盘依赖不保证完全靠谱 auto hinting 而拒绝手动对字体的优化，这也是**歪门邪道**——特别是这些用户对 CJK 字体设计的繁重工作量没有提供实质贡献的时候。
* 使用 FreeType 这样成熟的方案来实现，技术上自然不是歪门邪道；不过，这和实现的效果如何没直接关系。
	* 利用 FreeType 实现一直以来的 Mac 策略的多数 Linux 发行版，个人体验视觉效果并不如 Mac 。MacType 默认配置观感整体还更差（即便无视和不被 MacType 修改渲染效果的界面的不协调问题）。
	* 把 FreeType 和 MacType 混淆，则接近直白的**愚蠢**。
	* 也应当注意 FreeType 并不只是能够实现这些效果。FreeType 的默认效果和使用的具体 API 和使用的参数及字体都有关。
	* 事实上，FreeType 也能光栅化点阵字体（虽然并不怎么直白，以至作为开发者，之前还修复过[误用灰度而无法正确支持 SimSun 点阵的问题](https://github.com/FrankHB/YSLib/blob/f1518054a117619e32d260a11ca30bdef9c2d51b/YFramework/source/YSLib/Service/CharRenderer.cpp#L99)）。
	* 虽然使用了 C 这样（对一般非“系统”应用来讲尤其）普遍[糟糕](c-wrongs.md)的语言，但项目的代码质量意外地挺不错，模块化和文档的质量都比较高，实现也比 [stb](https://github.com/nothings/stb/blob/master/stb_truetype.h) 之流干净多了（尽管有出现过诸如严格兼容 ANSI C 却仍误用 `_` 起始的保留标识符这样的琐碎问题，不过维护者发现[修正](https://git.savannah.gnu.org/cgit/freetype/freetype2.git/commit/?id=37412ff9f42212bcf4dd29d9762f3c35b5735768)了）。近期 FreeType 的 API 有影响到依赖关系的整体性冗余问题，但这是 [Unicode 造的孽](criticisms-on-UTF-8-everywhere-manifesto.md) 。

　　遗憾的是，该知乎问题下的回答，虽然不乏详尽的比较分析，基本上并没有看穿这里提到的最重要的合理性问题。

　　题外话，我也算是十多年前就用过 GDI++ Helium 版本的老用户了，当时的屏幕条件下就觉得不对劲，调了些配置也觉得原有效果并没有在整体上被明显改善，所以并没有持续使用。（大概是早就近视到无可救药了？）而现在即便 Windows 的渲染仍旧没什么实质改善，在高分辨率屏幕下瑕疵也缩小了很多。由于这类屏幕一旦习惯很容易由奢入俭难，不是没得选择，并不大会使人去使用旧的屏幕。所以大概是没什么机会再刻意体验了罢。

# 字体配置

　　除了以上问题之外，Windows 的 FontLink 和其它一些方案使用的 fontconfig 的一些不便使用的设计问题……先坑了。

# 其它相关话题

　　还有其它几个因素也影响阅读渲染后字形的视觉舒适性，和字体渲染问题相关，在此一并讨论。

## 字体

　　字体设计毫无疑问地可能影响具体字形可被辨认的难度。

　　排除不实用的符号和艺术字体等，在字号不太大时，一般衬线体比无衬线体略容易辨认，因为衬线体的特征相对离散，更不容易出现挤成一团不易分辨的情形。（当然，小到一定程度什么看起来都费力，另当别论。）这点中文比西文更明显，因为后者字形少得多，而更容易形成“肌肉记忆”绕过分辨字形的开销。而光栅设备分辨率提升有助于呈现细节，则会使衬线体和无衬线体的字形辨认差异普遍减小。所以在分辨率不那么大的设备上，正文字体更适合无衬线体，因为这样通常可以减轻长时间阅读的视觉疲劳。

## 光源

　　分辨在非自发光的显示设备上的图像需要外部照明；在完全没有光照时，会字面上地瞎眼，完全无法分辨其中的内容。照明不足同样可能阻碍分辨内容，长时间观看容易造成视觉疲劳。传统的书本在显示特性上属于非自发光设备，也符合这里的描述。

　　自发光设备对环境光亮度的需求较低。不过更容易被忽视的一点是，过强的照明，不论是背光还是环境光，都容易加重视觉疲劳。为了视觉效果，自发光设备的亮度和外部光照条件相适应。正常阅读时，应避免外部过强的光照，以免设备无法提供足够的对比度而不得不依赖眼睛自行适应。过强的环境光（如太阳光）即便经过反射，对眼睛也可能有一定的损害。而即便环境光线不那么强烈，亮度过高也是无益的。

　　另一点与之关联的要素是自发光显示设备或（非自发光设备附带的光源）的刷新率。过低的刷新率可能引起亮度间歇性变化而造成不适。极端情况下，可能出现可明确感知的闪烁；即便没有闪烁，也容易造成视觉疲劳。长时间观看一般需要 60Hz 以上；不过这在主流显示设备中普遍能够达到。

## 分辨率

　　在过低分辨率的光栅设备上，字形更加依赖点阵，且可能造成即便显示效果足够清晰也难以辨认的情形，这通常应当被避免。

　　但在更高分辨率的设备上，视觉疲劳可能更加严重。因为显示设备的分辨率会影响眼球的活动状况，这种活动原则上不受到意识的直接支配，但可影响到阅读的舒适性。特别地，当设备的分辨率明显不足以到达视觉极限时，人的视觉成像系统可能会潜意识地分辨图像的细节——即便这对阅读文本毫无必要。

　　解决方法是使用更高的分辨率的设备使之接近或超过视觉极限。经验常识表明，当无论如何分辨也不能获得不同的细节时，眼球应会停止这种加重视觉疲劳的运动。考虑到分辨率等效无限大的非光栅设备（特别是自然光源成像），这种极限的存在是很自然的：正常人的视觉不会试图从无法分辨出像素的来源强行区分出不同的像素。

## 背景色

　　先前已经提到，浅色和深色背景有一定差异。对相同字重的相同字形，深色背景和浅色文字的组合相对交换两者的颜色得到的浅色背景和深色文字组合提供的对比度一般更小，所以更依赖加深自重的“模糊”渲染，这是 MacType 在深色背景下表现更清晰的原因。

　　但是，大面积的深色背景使显示设备整体趋近于不发光设备，而更依赖环境光的合理使用。长时间在暗处使用深色背景，容易造成视觉疲劳。如果有可能，最好能有足够强的环境光照。不过，这未必方便（特别是在夜间）；此外，这种情况下，和环境的对比度大大增加，深色背景的界面不再适合称为所谓的“夜间模式”。而在环境光线不足的情况下，浅色背景搭配不过于强的自发光设备或背景光源对眼睛更友好。

