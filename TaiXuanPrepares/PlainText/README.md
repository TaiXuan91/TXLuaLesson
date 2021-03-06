# Plain Text

Plain Text一词可以翻译为纯文本，指的是用二进制编码表示字符的一种只包含文本信息的文件。

由于涉及到“编码”问题，我建议诸位读者不妨先读一读[Charles Petzold](https://en.wikipedia.org/wiki/Charles_Petzold)所写的[*Code: The Hidden Language of Computer Hardware and Software*](https://en.wikipedia.org/wiki/Code:_The_Hidden_Language_of_Computer_Hardware_and_Software)一书。在后续的话题中我就假设诸位已经读过这本书。对于书中所介绍过的内容我就尽量不重复说明。当然，如果你已经对计算机的原理有一个大致的理解，不读这本书也没什么关系。

Petzold的书中提到了摩尔斯电码，布莱叶盲文和ASCII码。抛开具体的物理载体，这些编码方案本质上都是用自然数代表字符。它们之间的区别在于，对于同样的字符（或者字符组合），不同的编码方案可能采用了不同的数字与之对应。因此，一封用摩尔斯电码编码的信如果按照ASCII码的规则解读，很大可能完全弄不明白这封信本来要说什么。这就是所谓的编码方案之间的不兼容。

对于一个普通人来说，如果他明知一封信是用摩尔斯电码编码后发给他的，他一般不会故意用ASCII或布莱叶盲文的规则去解读。因此，编码不兼容的问题看似并不是一个特别值得思考的问题。但是，当计算机时代来临后，编码不兼容的问题却发展成了一个大麻烦。

## 巴别塔的余波

《圣经》中说，最初的人类都说着一样的语言。但上帝为了阻止人类齐心协力建造直通天堂的高塔，故意变乱人类的语言。这样说着五花八门语言的人类无法沟通协调，最终放弃了修造高塔的计划。而那座被放弃的高塔则被称作“巴别塔”。

现实中，人类发展出五花八门的语言的原因并没有这么戏剧性。可能单纯因为一些随机因素和地理的分隔，不同地方的人就创造出了不同的语言。

经过成千上万年的历史变迁，不同地方的人交流融合，语言也随之融合。总体上，语言的种类是越来越少了。但是，直到现在母语使用者破千万的都还有几十种[^1]。文字体系的数量要比发音体系的数量少一些，但现存的种类也很多。

计算机发明以后，用不同语言的人都想把自己的语言输入计算机中保存和处理。由于当时没有靠谱的国际组织负责协调语言文字编码的问题，有能力制定自己语言编码的组织都各自为政制定了自己的一套专属编码。但这些编码系统彼此不兼容，这就让语言不通的问题从人类之间蔓延到了计算机领域。欧美做的文本处理软件不一定能正常显示日语。日语系统下正常显示的软件到了中文系统可能就乱码了。甚至汉字都有好几套编码方案，这几套方案还互不兼容。

## 秩序之光

终于，在混乱之中出现了The Unicode Consortium这个组织。他们制定了一套Unicode标准。这套标准实际上就是尽量给每一个人类使用或者用过的字符分配一个独一无二的编号。不论是拉丁字母，阿拉伯数字，汉字，日语假名还是梵文字母统统放到一起编号。比如0061（十六进制的）分配给了小写字母“a”，这个编号就不会分给大写字母“A”、其他拉丁字母、汉字、假名或者别的什么字符。

Unicode指定的编号还不能直接作为字符的编码。因为Unicode编号的位数是不确定的，随着被编码的字符越来越多，所需要占用的最大位数也会越来越大。而现在计算机中保存一般数据都是以字节为单位的。我们还需要一种转换方法把Unicode字符的编号转换为变长编码。这种变长编码才实际在计算机上通用的，可以表示各种主要书写体系字符并且还在不断完善的编码系统。

最常用的转换方法称为UTF-8。它转化Unicode编号所得的变长编码称为UTF-8编码。“UTF”是“Unicode Transformation Format”的缩写。数字“8”表示这种转换方法以8位二进制数为1节。每当转换码快用尽时就追加8位。具体的转换方法见[Wikipedia: UTF-8](https://en.wikipedia.org/wiki/UTF-8)。

>**吐槽时间**
>
>为了统一标准而发行了一个新标准这种事必须有地位的组织做才行。不然只会让标准混乱的情况更加严重。如果当年秦国没有统一六国的实力却制定了一套新的货币、文字、度量衡，那只会让本就乱糟糟的战国更加混乱。

> **拓展资料**
>
> Unicode标准可以在[Unicode官网](https://www.unicode.org/)看到。
>
> 如果有兴趣了解一下目前Unicode用了多少编号以及这些编号如何分布，可以参考[Wikipedia: Plane (Unicode)](https://en.wikipedia.org/wiki/Plane_(Unicode))

## UTF-8的兼容性

ASCII以前是一套在电传打字机等设备上使用的定长编码方案。后来也由于计算机的字符编码。它用7位二进制数编码，一共编了128个符号（其中包含一些控制字符，不全是可打印字符）。在以8位为1字节的计算机中处理ASCII编码的文本时，1个字节保存1个字符编码，字节最高位始终为0，剩下7位存放ASCII编码。这些符号满足 了英语文本的需求，但是对于那些使用了和英文不同字符集的语言就很不友好。

国际标准化组织（ISO）和国际电工委员会（IEC）制定了ISO/IEC-8859标准来扩展ASCII。这个标准把ASCII闲置的最高位利用起来，把编码长度凑成1字节。这样可用编码就扩展到了256个。前128个编码和ASCII保持一致。后边128个编码对应的内容则是ISO/IEC-8859中新增的。

为了让尽可能多的字母文字有编码可用，ISO和IEC用了一个现在看起来很像是昏招的办法——在ISO/IEC-8859中一口气定义了16个编码方案。这16个方案的后 128位各不相同。分别针对不同的字母文字系统进行了字符补充。比如1号方案在后128个编码中增补了西欧语言中用到但是英语中没有的字母，2号方案增补中欧和东欧语言，3号南欧，4号北欧，7号希腊，13号波罗的海诸国等等。由于这些方案所涉及的语言主要是拉丁语系的，所以这些方案都以“Latin”加数字或者单词来命名。比如1号方案叫“Latin-1 Western European”，简称“Latin-1”。2号方案叫“Latin-2 Central European”。7号方案叫“Latin/Greek”。

ASCII和它的这些扩张在Unicode制定的时候影响力很大。Unicode在设计之初就决定要兼容已经存在的主要的编码方案。ASCII自然是要兼容。但是ISO的这16个互相不兼容的编码方案显然不能都顾及到。Unicode最终选择了兼容影响力最大的Latin-1（包含法语，德语，意大利语，西班牙语等）。因此现在的Unicode编码表上00到7F与ASCII完全对应，80到FF与Latin-1对ASCII完全对应（也就是00到FF与Latin-1完全对应）。其他Latin-1没有收录的字符按顺序往后排。

> **吐槽时间**
>
> Unicode的几个拉丁字母补充集都用英文字母标号，唯独Latin-1拓展ASCII的这一部分名称中带数字，称作“Latin-1 Supplement”。可能Unicode把后边的补充集换成用字母编号是为了和当初用数字编号的这一系列拉丁字符集区分。

这个话题到这里还不算完。我们实际上使用的是UTF-8而不是Unicode的字符编号。按照UTF-8的规则，00到7F的编号不需要转换，Unicode编号就是UTF-8编码。所以一个ASCII字符串也是一个UTF-8字符串。一个只含有00到7F字符的UTF-8字符串也是一个ASCII字符串。但是Latin-1中拓展的那些字符就没这么好运了。按照规则80到7FF这一段编号要转化为2字节编码。因此Latin-1编码的字符串并不是合格的UTF-8字符串。UTF-8字符串也不能当作Latin-1字符串来处理。

## 新的混乱

Unicode和UTF-8正在统一混乱的字符编码方案。但是在这个过程中还是出现了一些新的混乱。

其一是UTF-16和UTF-32等同属UTF系列的编码方案的存在。UTF-8编码以1字节为单位长度。最短的UTF-8编码是1字节。每次编码不够的时候就增长1字节来扩展编码空间。而UTF-16以2字节为单位长度，最短的UTF-16编码是2字节表示一个字符，每次增加2字节。这样UTF-16的所有字符编码的长度都是2字节的整数倍。而UTF-32则以4字节为单位长度，所有UTF-32的编码长度都是4字节的整数倍。除了基本单位的长度不同，这3中编码的思路和转换方法都是一致的。

有的人认为UTF-16编码支持一次处理2字节，UTF-32编码支持一次处理4字节，有助于发挥现在高位宽的运算优势。但这会带来存储空间上的浪费。一个纯英文文本用UTF-32存储所占用的空间是UTF-16的2倍，UTF-8的4倍。况且，在计算机速度已经非常快的今天，这点速度提升在文本处理中并不是很吸引人。为了追求这一点蝇头小利，更换标准编码十分不值得。因此UTF-16，UTF-32实际上目前还没有太大的应用价值。

但是就是有这么一个叫微软的奇葩公司，非要在他们的Windows操作系统中用UTF-16编码。而且还把用UTF-16编码称为“Unicode编码”。这就造成了非常大的混乱，首先Windows上默认保存为UTF-16编码的文件拿到其他不支持UTF-16的计算机上会乱码。第二，混淆了UTF-8，UTF-16和Unicode的概念。让用过Windows的人经常把这三者混为一谈。

而且这还不算完。微软还干了另一件添麻烦奇葩事。由于UTF-16用2个字节为单位长度，在保存到计算机中时，每一段的2个字节如何排列也是一个问题。如果读写的时候顺序不一样，编码就会变乱。为了说明是高位字节对应内存高地址还是低位字节对应内存高地址，需要在文件开头额外占用几个字节说明字节排列顺序。这几个字节的标记称为BOM（Byte Order Mark）。BOM和其他控制字符类似，不会直接打印出来，文本编辑器和浏览器会自动读取和处理。UTF-32由于每段用了4个字节，可能的排列数多达24种[^2]，所以更需要BOM来理清顺序。

Unicode的标准中也允许给UTF-8加BOM，但不推荐给UTF-8使用BOM[^3]。因为UTF-8每段只有1个字节，根本不存在搞乱的可能。但是奇葩的微软偏偏要在保存UTF-8的时候带BOM。这样保存的文件拿到其他系统，用默认不带BOM的软件打开就会遭遇麻烦。

## 编码和信息量

字符编码的长度会影响到文本文件的大小。比如一个汉字在GBK中占2个字节，但在UTF-8中一般占3个字节。这样用UTF-8编码保存的汉字文本是GBK格式的约1.5倍。

但是，个人仍然建议所有非英文文本都用UTF-8保存，尤其是一些存在国际交流的场合。否则很可能遇到编码识别不了的尴尬。

在现在，由于编码方式造成的文件大小的差异其实无关紧要。一方面存储器很便宜了，没必要锱铢必较。另一方面，可以压缩来减少文件占用的空间。根据我的实验，GBK编码的汉字文本和UTF-8编码的汉字文本压缩后体积差不多大。因为虽然编码格式不同，但是信息量是差不多的。UTF-8中虽然汉字占了3个字节，但是每个汉字的UTF-8编码的最高几位都是相同的。压缩的时候这些重复的东西就会被压缩掉。

再说了，一旦在重要场合出现乱码问题，节省再多的存储空间也没有意义。

## 小结1

综上所述，推荐使用UTF-8（不带BOM）编码文本。

另外，注意以下问题：

1. 由于UTF-8可以视为ASCII的超集，所以ASCII编码的纯英文文本也可以视为是UTF-8编码的。
1. Unicode标准中都不推荐给UTF-8加BOM。所以以后提到UTF-8都默认不带BOM。

## 控制字符

本文的前半部分讨论了字符编码方案的问题。现在我们已经把字符编码方案确定到了UTF-8（包括ASCII）这个方案上。

但是这并没消除所有纯文本格式分歧。我们还需要解决一些控制字符带来的问题。

ASCII中所编码的字符，可以大致分为两类。编码20到7E的称为可打印字符（其中包括了空格）。编码00到1F的字符，外加7F，称为控制字符。ASCII的控制字符一共33个。就这么33个控制字符，却引发了很多争论和混乱。而且其中的一些问题到现在都没有统一意见。

## 控制什么

控制字符当然是用来“控制”的。就ASCII所处的时代而言，主要是控制电传打字机（[teleprinters](https://en.wikipedia.org/wiki/Teleprinter)）的。比如编号0A控制打字机里的纸张移动，0D控制打字头移动，02控制开始文本，03控制结束文本，07控制打字机上附带的电铃响一下等等。

后来人们用带屏幕和键盘的字符终端机（[
computer terminal](https://en.wikipedia.org/wiki/Computer_terminal)）链接计算机，这些控制字符就接着用来字符终端机。

![VT100终端机](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/DEC_VT100_terminal.jpg/1024px-DEC_VT100_terminal.jpg)

图1：VT100终端机

可以说，一开始人们只是把打字机和终端机有的功能对应到了控制字符上。并没有考虑这些功能以后可不可能扩展，可不可能改变。当人们不再使用电传打字机和终端机的时候，这些控制字符还作为标准残留在ASCII中，又被Unicode继承，成了历史遗留问题。

## 大多被弃用

既然控制字符的作用是控制机器，那么显然和人类书写系统的关系不大。所以，所谓“控制字符”本质上是命令，而非“字符”。当初为了使用方便而把控制命令和字符混在一起编号，如今看来并不是一个很高明的决定。再加上现代计算机超级复杂的功能很难再和古老的电传打字机或者终端机的功能一一对应起来，大部分ASCII控制字符随着计算机的发展逐渐被废弃了。

> **说明**
>
> 编码方面比较现代的做法，是把数据和指令分离。比如现在主流计算机系统中CPU指令和字符编码（或者整数编码、浮点数编码）是分开的。不会像ASCII那样，把CPU命令和字符（或者整数、浮点数）混在一起编码。

虽然大部分ASCII控制字符都废弃了，但兼容ASCII的Unicode，仍保留着些控制字符。这就非常尴尬。如果要把它们当普通字符显示出来，它们又没有明确统一的字形（因为本来就不是字）。个人认为一个比较好的做法就是尽量避免在文本中出现这些控制字符。但是还有编号00，0A，09，0D的这几个特例需要考虑。

> **表示控制字符的符号**
>
> Unicode兼容ASCII把控制字符作为了字符，但是这些字符又没有字形。在提到它们的时候会用一些别的特殊符号或者缩写来表示这些符号。久而久之这套用来表示控制字符的符号也相对固定下来。所以Unicode就又把这些表示控制字符的符号也作为字符收录进字符表。只不过ASCII控制字符和表示控制字符的字符的编码并不相同。
>
> 比如你在HTML中输入`&#x0001`，一般是渲染不出字的。但是它对应的表示它的字符`&#x2401`，在字体支持的情况下是可以渲染的（一般显示为SOH的合字）。
>
> 真不知道以后会不会出现专门为了表示ASCII控制字符的表示字符的字符。（被这句话绕晕了没？:laughing:）

## 作为结束的开始

编号00的NUL（空字符）在C/C++中表示字符串的终止。由于一旦出现它就认为字符串终止，这个字符不适合直接出现在文本文件中间。在源码中要用转义字符串`\0`来表示这个字符。

对于很多C/C++初学者来说，这个字符简直就是噩梦。考题中经常会考“`"abc"`在内存中占3个字节还是4个字节”，“`"a"`和`'a'`在内存中所占空间是否一样”，“`"12"`中有2个字符还是3个字符”之类的问题。

好在，用Python之类的语言可以避开这些讨厌的问题。在Python中你不需要记住字符串的结尾以什么为标值。编号00的NUL就是一个普通字符。但对于从C转Python的人来说，还是会有很多困惑。比如这段Python代码

```python
a="hello\0world"
print(a)
```

和这段C代码

```C
char *a="hello\0world";
printf("%s",a);
```

看似做的是一样的事情。但实际上Python代码会输出

```
hello world
```

因为NUL是普通字符，所以当作普通字符输出。又由于没有对应的字形，所以当成空格输出。

而C代码会输出

```shell
hello
```

（注意`hello`后边没有空格，没有换行，什么都没有。）

这是因为在C语言中NUL截断了字符串，所以字符串到`hello`就结束了。

##  换行之争

以前，电传打字机把换行打字分解为纸张移动一行和打字头回到最左端这两个动作。对应于这两个动作，分别使用换行符（编号0A，缩写为LF）和回车符（编号0D，缩写为CR）两个控制命令表示。到了计算机时代，字符终端也可以显示换行。但是工程师们在怎么表示换行的问题上产生了分歧。主流的观点就用三种。一种延续打字机的做法用CR LF表示（Windows的做法），一种认为只用CR就够了（早期Mac的做法），一种认为只用LF就够了（Unix，Linux的做法）。另外还有想用LF CR表示的，想用其他符号或者发明新符号表示的。这种争论是永远没有终点的。主流的编辑器一般兼容以上所有的换行表示方法。但我个人的意见是和Unix/Linux保持一致，使用LF。

> **吐槽**
>
> 微软真的是麻烦制造者。换行符的问题，BOM的问题，混淆UTF-16和Unicode的问题都和微软的Windows有关。

## 空白之争

空格被认为是显示为什么都没有（打印为一块空白）的字符，所以也是可显示，可打印的字符。

编号为09的控制字符HT（水平制表符），则用于控制打字头（或者光标）在文字中水平留下一段空白。HT的主要用途有两个：

1. 用在行首表示一层缩进样式。
1. 在没有Excel的时代用于调整文字的列对齐，使文本表现出表格的样子。

有一天，一群人发现敲几个空格和输入一个HT的显示效果是一样的。但是由于空格和HT的编号不同，在人看来一样的文本在计算机中又会被当成不同的字符串。这群人中有的支持用连续的空格替换HT，有的则坚持用HT。持不同观点的人谁也不服谁，就开始在社区论坛里论战。

不过这么多年过去了，这个问题仍然没有结果。如果你希望和朋友们割袍断义，反目成仇，拳脚相向，你死我活，不妨在聚会时讨论讨论这个话题。在人多的时候配合“用CR LF还是LF”，“用Vim还是Emacs”一起使用效果更佳。

现在的很多编辑器可以提供一种功能。当你按键盘上的<kbd>Tab</kbd>的时候，不向文本中添加HT，而是转换成若干个空格。有的还能自动计算列对齐所需的空格个数，根据插入的位置不同调整空格数量。这样既有了Tab党追求的快捷，也有了空格党追求的空白宽度的精确。顺便还满足尽量减少文本中控制字符的方案。我个人是比较倾向于这种巧妙伪装过的空格党方案的。

## 分离排版信息

LF，HT这类控制字符虽然不能直接打印成一个字形，但也是有显示效果的。这个显示效果不是字符级别的，而是排版级别的。换行，缩进等都是排版样式的一种。但是排版中所能展现的样式绝对不仅仅只有换行和缩进这两种。如果为每一种可能的排版样式都分配一个Unicode编码，又麻烦又不好用。而且由于Unicode一般不修改已经发布的编码。万一过几年流行起来新的排版方式，一大批表示排版信息的编码估计就废了。

现在的做法是把排版信息和字符编码分离开。Unicode专注于字符编码。而HTML这样的标记语言中，用一些约定好的字符串标记来表示被标记内容的排版信息。但不论是作为标记的字符串，还是作为文字内容的字符串都是可打印字符组成的。这样排版和字符编码互不干扰，更换字符编码不影响排版效果，排版标记的变动也不影响字符编码。

有了标记，继续使用表示表示排版信息的控制字符实在是没有必要。控制字符表示不了的排版信息全部用标记表示。控制字符能表示的排版信息，也都可以用标记表示。比如HTML中就用标记`<br/>`表示换行，替代了LF的作用。

## 一个大胆的想法

现在我们聊聊完全停用ASCII控制字符的可能性。

首先，完全停用ASCII控制字符是非常不经济的。比如LF和HT大量用在编程中。这两个符号在很多编程语言中还被赋予了语法意义。要完全停用这两个控制字符，就要把所有现有的语言规范，以及代码文件全修改一遍。

但是我现在想说的是，虽然不经济，从原理上说却是可能的。占用字符编号的控制字符也好，HTML中的标记也好，其实都只是表示信息的一种方式。而信息是可以在不同形式之间相互转化的。

在文字排版领域，我们可以用LF表示换行，也可以用`<br/>`之类的标记表示。不想用LF，就直接把LF搜索替换成对应的标记就行了。

在程序源码中LF除了表示排版信息，还有语法意义。例如有的语言用LF来表示语句结束。这也不是什么难事，约定一个字符串表示语句结束就行了（比如用分号）。可能有人会问没有LF了怎么换行？要知道，换行本来就是排版信息，而程序源码中没有必要包含排版信息。就像HTML和CSS把网页内容和样式分开保存那样，我们也可以把代码和代码的显示样式分开。要想源码分行显示，只需要给编辑器写一个样式配置文件。让编辑器显示代码的时候，遇到句尾标记（比如分号）就换行显示。你也可以写更复杂的样式，遇到函数头换行，遇到类名加粗放大，遇到注释改变字体颜色等等。

## 小结2

注意这一段完全是**私货时间**。

关于控制字符的这几个争论，目前还没有完全统一的意见。因此只能是以我个人建议的形式给出。主要是这两条：

1. 使用LF表示换行。
1. 使用空格表示缩进，而不使用HT。

如果你不赞同使用空格表示缩进，请坚持整个文本或项目中都是用HT缩进。混用空格和HT表示缩进怎么看都不是个好主意。

另外，请注意，我这两条规则都是针对字符编码的。而不是按键。不管你用LF换行还是CR LF，在打字的时候按的都是<kbd>Enter</kbd>。如果编辑器支持，你按<kbd>Tab</kbd>可以输入HT，也可以输入2空格，4空格，8空格。

[^1]: 按母语统计。数据参见https://en.wikipedia.org/wiki/List_of_languages_by_number_of_native_speakers。
[^2]: 实际上并没有那么多。Unicode标准中仅仅允许从大到小和从小到大这两种顺序。其他的可能由于太奇葩都被排除了。
[^3]: 见<http://www.unicode.org/versions/Unicode5.0.0/bookmarks.html>，2.6  Encoding Schemes。

