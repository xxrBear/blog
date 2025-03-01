---
title: "一文粗通字符编码：从ASCII到Unicode的演进与实战"
date: 2025-02-23T22:04:39+08:00
lastmod: 2025-02-23T22:04:39+08:00
author: ["熊大如如"]
tags: # 标签
  - "unicode"
description: ""
weight:
slug: ""
summary: "一文粗通字符编码：从ASCII到Unicode的演进与实战"
draft: false # 是否为草稿
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
  image: "https://cdn.jsdelivr.net/gh/xxrBear/image//Hugo/202502232209330.jpg" # 文章的图片
---

原本计划整理一篇关于`Python 2`和`Python 3`字符编码异同的文章，但在查阅资料的过程中，发现字符编码的世界远比想象中复杂。为了梳理这些知识，特此记录并分享。通过这篇文章，你将了解以下内容：

1. 字符、字符集、字符编码、码点的含义
2. 字节和字符的区别
3. 世界上主要的字符集和字符编码

感谢 deepseek 给我总结的标题 😂

## 一、字节和字符的区别

### 1）基本定义

字符(Character)：字符指的是任何字母，数字，符号。在不同的国家，字符的表示范围可以从简单的字母 abc 到复杂的汉字，甚至还有 emoji 表情 😗

字节(Byte)：字节是计算机储存信息的基本单位。一字节等于八位 (1 byte = 8 bit)

### 2）混淆与区分

为何常被混淆：在简单的英文编码系统中，如`ASCII`，一个字符与一个字节对应，这导致了两个概念间的混淆。

如何清晰区分：理解字符是文本数据的逻辑单位，而字节是物理存储和传输的单位是区分两者的关键。

## 二、字符、字符集、字符编码和码点的含义

### 1）字符

上面说过了

### 2）字符集

字符集（Character Set）：是指一组字符、码点、对应的二进制数据的集合。不同的字符集包含的字符个数不一样、包含的字符不一样、对字符的编码方式也不一样。

常见的字符集包括，`ASCII`、`GBK`、`BIG5`、`Latin-1`、`Unicode` 等等

### 3）字符编码

字符编码（Character Encoding）：字符编码是指一种映射规则，根据这个映射规则可以将某个字符映射成其他形式的数据以便在计算机中存储和传输。

常见的字符编码：`ASCII`、`GBK`、`UTF-8`、`UTF-32` 等等。

没错，`ACSII` 即是字符集也是字符编码！

点此查看 [ASCII 表](https://www.runoob.com/w3cnote/ascii.html)

### 4）码点

码点（Code Point）：字符编码的核心，它表示字符在字符集中的**唯一编号**。

比如在`ASCII`字符集中，字母 A 经过`ASCII`编码得到的十进制值是 65，那么 65 就是字符 A 在`ASCII`字符集中的码点。

### 5）总结

**通俗解释：**

字符集就是把字符放到一起的一个集合。

而这个集合的每一个字符都对应一个数字，叫做码点。

这样就建立起来数字和字符之间的索引关系。那么，某个字符在计算机中怎么表示，具体占用几个字节等等，这些就需要编码规则来解决了，这个就是字符编码。也就是说，字符编码就是如何根据规则把这些数字（码点）转成计算机能理解的字节。

## 三、常用的字符集

### 1）ASCII 字符集

`ASCII`（American Standard Code for Information Interchange，美国信息交换标准代码，美国制定的一套字符编码规则，对所有英语字符与二进制位之间的关系做了统一规定，这编码规则被称为`ASCII`编码。

`ASCII`编码一共规定了 128 个字符的编码规则，这 128 个字符组成的集合就叫做`ASCII`字符集。在`ASCII`编码中，每个字符占用一个字节的后面 7 位，第一位统一规定为 0

`ASCII`编码可分为两部分，**控制字符** 和 **可显示字符**

`ASCII`中既是字符集，也是字符编码 😁

点此查看 [ASCII 表](https://www.runoob.com/w3cnote/ascii.html)

### 2）ISO 8859 系列字符集

`ISO 8859` 系列字符集也叫`Latin`系列字符集

前面说了，`ASCII`只包含了英语国家常用的 128 个字符。但是到了欧洲，这 128 个字符就不够用了，因为在法国或者俄罗斯，字符可能有 `ё`或者 `é`这样不同的变体。

为了解决字符不够用的问题，欧洲国家发明了一种 `ASCII`字符集的扩展。前面也提到了，`ASCII` 编码只用了七位，第一位固定为 0，那么扩展就使用了前面的一位，让其可以表示的字符从 128 个字符增加到了 256 个字符。

`ISO 8859` 系列是由国际标准化组织（ISO）制定的一系列字符集，主要用于表示西欧和东欧语言的字符。它们都是 **单字节编码**，通常每个字符集最多支持 256 个字符，同时兼容`ASCII`编码。

但是欧洲国家实在是太多了，每个国家都有自己的扩展字符集，从`latin-1`一直到 `latin-16`😂，每一个字符集之间都不一定完全兼容。

### 3）中文字符集

欧洲国家的问题解决了，但是中国的问题还是没有解决，之前说了，用一个字节来表示字符，至多只能表示 256 个不同的字符，这对于中文来说显然是不够用的，毕竟中文常用的汉字就超过了几千个了。

那么，怎么办呢？

简单，既然一个字节不够，那么我们可以使用两个字节来表示一个汉字，这样，理论上可以表示的字符就是 256 \* 256 = 65536 个。这对于常用的汉字来说是够了的。

是的，`GB2312` 就是如此，它定义了六千多个常用的简体汉字和符号等。

`GBK`是对 `GB2312` 的扩展，它额外增加了繁体中文、 少数民族文字等的支持。

中国大陆的中文字符集有 `GB2312`、`GBK`等，港澳台地区有 `BIG5`等。还是互不兼容，这就出现了和欧洲人一样的问题 🙃

> 虽然查阅 `GBK`更倾向于是字符编码，但我还是写上了

### 4）**Unicode 字符集**

因为每个国家或地区都有对应的字符集，很容易就导致字符编码的混乱。为了解决字这个问题，1987 年，Apple 工程师 `Joe Becker` 在一篇关于全球字符编码的备忘录中提出了`Unicode`这一概念。后来，来自 Xerox 和 Apple 的工程师们共同推动 `Unicode` 的制定。

`Unicode`字符集是一个很大的字符集合，现在的规模可以容纳 100 多万个字符。每个字符的码点都不一样，比如，`U+0639` 表示阿拉伯字母 Ain，`U+0041` 表示英语的大写字母 A，`U+4E25` 表示汉字“严”。

`Unicode` 为全球大部分书写系统的字符分配了唯一的码点，并不断扩展以支持更多的字符。如今，`Unicode` 已经成为全球最广泛应用的字符编码标准，并且不断扩展，支持所有现代语言以及一些古代文字，为全球信息交换提供了基础。

## 四、常用的字符编码

前面介绍了常用的字符集，现在该介绍常用的字符编码了。

我们首先要知道几个概念，字节是计算机上的最小寻址单位。而计算机中的信息都是以二进制值 01 储存的。

### 1）ASCII 字符编码

`ASCII`码一共规定了 128 个字符的编码，比如空格`SPACE`是 32（二进制`00100000`），大写的字母`A`是 65（二进制`01000001`）。这 128 个符号（包括 32 个不能打印出来的控制符号），只占用了一个字节的后面 7 位，最前面的一位统一规定为`0`。

也就是说，`ASCII`码的二进制从 `00000000`到 `01111111`共 128 个。

点此查看 [ASCII 表](https://www.runoob.com/w3cnote/ascii.html)

### 2）**Latin 系列字符编码**

以`Latin-1`为例：

`Latin-1`，也称为`ISO/IEC 8859-1`，是一种常见的字符编码标准。它扩展了`ASCII`编码，支持更多的字符，主要用于西欧语言（如英语、法语、德语、西班牙语等）。以下是关于`Latin-1`编码的详细介绍：

因为它是扩展了`ASCII`码，所以`00000000`到 `01111111`所代表的字符与`ASCII`一样。

具体`Latin-1`字符与二进制数值的对应，可自行谷歌。

### 3）GB2312 **字符编码**

`GB2312` 是最早简体中文字符编码。

`GB2312` 在单字节上是兼容`ASCII`的。

`GB2312`是对`ASCll`码的扩展，占用两个字节。具体的编码规则这边就不介绍了，感兴趣的读者可以参考这篇博客

[GB2312 编码规则与代码实现](https://blog.csdn.net/apollon_krj/article/details/77131045)

### 4）UTF 系列字符编码

因为 `Unicode`只规定了每个字符对应的码点，并没有规定每个字符以几个字节存储在计算机上，也就是对应的字符编码，所以衍生出来多种`Unicode`字符编码。例如：

`UTF-32`、`UTF-16`、`UTF-8` 等等。

`UTF` 系列字符编码太重要了，下面单独介绍一下。

## 五、UTF 系列字符编码

让我们用诞生时间先后顺序来逐一介绍

### 1）UTF-16 字符编码

`UTF-16` 使用 2 或 4 字节 来表示字符，适合大多数语言（尤其是西方语言和东亚语言），并且支持辅助平面（补充字符）。它是最早提出的能够支持全球范围字符集的编码方式之一。

不兼容 `ASCII`编码。

你可能不明白为什么 `UTF-16`不兼容 `ASCII`编码。

举个例子：

因为 `ASCII`编码存储的都是单字节数值，而`UTF-16` 最少需要两字节，当使用 `UTF-16` 解码 `ASCII` 编码存储的二进制数字时，会因为缺少第二个字节而导致不兼容问题。

### 2）UTF-8 字符编码

`UTF-8` 是一种变长编码，它使用 1 到 4 字节 来表示字符，能够兼容 `ASCII` 编码。`UTF-8` 对 `ASCII` 字符使用 1 个字节，对其他字符使用更多的字节。它是一种**变长编码**，适用于需要节省存储空间的场合，并且由于其兼容 `ASCII`，使得它成为 Web 和 互联网领域的广泛标准。

光看这些，你可能还是有些疑问，为什么 `UTF-8` 能够辨别不同字节长度的二进制数值？它是怎么做到的？

**其实很简单，只要记住两条规则：**

1. 对于单字节的符号，字节的第一位设为`0`，后面 7 位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。
2. 对于`n`字节的符号（`n > 1`），第一个字节的前`n`位都设为`1`，第`n + 1`位设为`0`，后面字节的前两位一律设为`10`。剩下的没有提及的二进制位，全部为这个符号的 `Unicode` 码。

下表总结了编码规则，字母`x`表示可用编码的位。

```plain
Unicode符号范围     |        UTF-8编码方式
(十六进制)          |       （二进制）
----------------------+---------------------------------------------
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

跟据上表，解读 `UTF-8` 编码非常简单。如果一个字节的第一位是`0`，则这个字节单独就是一个字符；如果第一位是`1`，则连续有多少个`1`，就表示当前字符占用多少个字节。

下面，还是以汉字`严`为例，演示如何实现 UTF-8 编码。

`严`的 `Unicode` 是`4E25`（`100111000100101`），根据上表，可以发现`4E25`处在第三行的范围内（`0000 0800 - 0000 FFFF`），因此`严`的 `UTF-8` 编码需要三个字节，即格式是`1110xxxx 10xxxxxx 10xxxxxx`。然后，从`严`的最后一个二进制位开始，依次从后向前填入格式中的`x`，多出的位补`0`。这样就得到了，`严`的 `UTF-8` 编码是`11100100 10111000 10100101`，转换成十六进制就是`E4B8A5`。

---

如果你看完了上面的例子还是不理解，没关系，不用怀疑自己，这是很正常的，为什么我这么说呢？

因为我自己也没理解 🤡

<del>多看看多想想~😂</del>

### 3）UTF-32 字符编码

`UTF-32` 是`Unicode`标准的一种编码方式，它使用固定长度的 4 字节来表示每个字符，因此能够直接对应 `Unicode` 码点。它也不兼容 `ASCII`编码，原因与 `UTF-16`基本一致。

### 4）UTF-8 字符编码流行度最高

`UTF-8` 使用可变长度的字节来表示不同的字符：对于 `ASCII` 字符（即 Unicode 范围内的 0 到 127），只需要 1 字节，而对于其他字符则使用 2 到 4 字节。这种可变长度的设计使得它能根据字符的需要动态调整字节长度，既能节省存储空间，又能处理全世界所有的字符。

## 六、编码和解码

你已经了解了什么是字符、字符集、字符编码。

下面我来为你介绍两个重要的概念：什么是编码？什么是解码？

要理解哦，这两个概念很重要！🙂

### 1）编码

**编码(encode)**：编码是将字符转换为对应的二进制数据（或十进制、十六进制表示）的过程。

### 2）解码

**解码(decode)**：跟编码相反，解码就是把二进制数据（或十进制、十六进制表示）转化为字符的过程。

### 3）用 ASCII 字符编码举个例子

编码就是把大写字母 A 用 ASCII 字符编码，变成计算机能理解的二进制数值 `0100 0001`。

解码就是用 ASCII 字符编码，把二进制数值 `0100 0001`变成大写字母 A

## 七、乱码：实战一下

说了这么多，没有实际的例子，你可能还是不能理解为啥会乱码？没关系，让我们来实战演示一下一个乱码的案例，让你有直观的感觉 💖

前文已经说了，不同字符集中各个二进制数所代表的字符不同，你在本地用 `GBK` 编码格式书写的文字，发给你的香港朋友，使用 `BIG5` 字符解码打开，就会出现字符编码不同而导致的乱码(**UTF-8 编码的重要性**🤪)，具体乱码的样式你可以查看下文

举例 🌰

创建一个`hello.txt`文件

```plain
# 使用 GBK 编码储存下面的文字
你好
```

使用 `BIG5` 字符集打开会显示如下文字，为什么呢？

```plain
斕疑
```

让我们查看对应的十六进制数字。

[GBK 对应](https://www.toolhelper.cn/Encoding/GBK)

[BIG5 对应](https://www.toolhelper.cn/Encoding/Big5)

查看`GBK`对应的十六进制码点，会看到 `你好`两个字对应的十六进制数分别是 `C4E3`和 `BAC3`

再查看`C4E3`和 `BAC3`对应的文字，确实就是`斕疑`。

至于 `锟斤拷�⊠`怎么显示的，我操作不出来 😅

但是它的本质还是编码和解码的方式不同导致的。你可以自行查看参考链接中的视频了解。

## 八、高低字节序

不明白，pass

## 九、总结

上世纪六十年代，在第三次科技革命的发源地美国，随着计算机技术的快速发展，信息交换的需求日益迫切。1963 年，`ASCII`编码应运而生，它解决了英语字符在计算机上的表示问题。然而，`ASCII`仅适用于英语，无法满足其他语言的需求。因此，各国纷纷开发了自己的字符编码标准，例如中国的`GB2312`、日本的`Shift_JIS`、俄罗斯的`KOI-8`等。这些编码虽然解决了各国的本地化需求，但在国际交流中却导致了严重的兼容性问题。

为了解决这一全球性难题，1991 年，`Unicode`编码体系诞生。`Unicode`旨在为世界上所有的字符提供一个统一的编码标准，涵盖了从拉丁字母到汉字、日文、韩文等几乎所有语言文字。通过`Unicode`，不同国家和地区的人们终于能够实现无障碍的信息交流，极大地推动了全球化和信息化进程。

欢迎指正~

## 十、参考链接

- [字符和字节的区别](https://docs.pingcode.com/ask/67475.html)
- [Unicode 之痛](https://pycoders-weekly-chinese.readthedocs.io/en/latest/issue5/unipain.html)
- [字符、字符集、字符编码的基础知识科普](https://zhuanlan.zhihu.com/p/260192496)
- [字符编码笔记：ASCII，Unicode 和 UTF-8](https://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
- [encode 和 decode——带你探索编码与解码的世界](https://www.cnblogs.com/tjp40922/p/14058694.html)
- [视频][你懂乱码吗？锟斤拷烫烫烫（详解ASCII、Unicode、UTF-32、UTF-8编码）](https://www.bilibili.com/video/BV1xP4y1J7CS/?spm_id_from=333.337.search-card.all.click&vd_source=9bcfca4212a15c95e7c4eaa938a7fc3f)
- [视频][锟斤拷�⊠是怎样炼成的——中文显示“⼊”门指南【柴知道】](https://www.bilibili.com/video/BV1cB4y177QR?spm_id_from=333.788.recommend_more_video.2&vd_source=9bcfca4212a15c95e7c4eaa938a7fc3f)
- [视频][快速搞懂🔍Unicode，ASCII，UTF-8，代码点，编码...](https://www.bilibili.com/video/BV1kD4y1z7ft?spm_id_from=333.788.recommend_more_video.3&vd_source=9bcfca4212a15c95e7c4eaa938a7fc3f)
