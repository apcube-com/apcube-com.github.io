---
title: "Unicode/UTF-8的区别"
date: "2015-07-28"
categories: 
  - "介绍"
tags: 
  - "encoding"
  - "unicode"
  - "utf-8"
  - "what-is-it"
  - "about-encoding"
---

## 什么是 Unicode?

历史上, 有两个独立的, 创立单一字符集的尝试. 一个是[国际标准化组织(ISO)](https://www.iso.ch/)的 ISO 10646 项目, 另一个是由(一开始大多是美国的)多语言软件制造商组成的协会组织的 [Unicode 项目](https://www.unicode.org/). 幸运的是, 1991年前后, 两个项目的参与者都认识到, 世界不需要两个不同的单一字符集. 它们合并双方的工作成果, 并为创立一个单一编码表而协同工作. 两个项目仍都存在并独立地公布各自的标准, 但 Unicode 协会和 ISO/IEC JTC1/SC2 都同意保持 Unicode 和 ISO 10646 标准的码表兼容, 并紧密地共同调整任何未来的扩展.

## Unicode vs ISO 10646

Unicode 协会公布的 [Unicode 标准](https://www.unicode.org/unicode/standard/standard.html) 严密地包含了 ISO 10646-1 实现级别3的基本多语言面. **在两个标准里所有的字符都在相同的位置并且有相同的名字**.

Unicode 标准**额外**定义了许多与字符有关的语义符号学, 一般而言是对于实现高质量的印刷出版系统的更好的参考. Unicode 详细说明了绘制某些语言(比如阿拉伯语)表达形式的算法, 处理双向文字(比如拉丁与希伯来文混合文字)的算法和 排序与字符串比较 所需的算法, 以及其他许多东西.

另一方面, ISO 10646 标准, 就象广为人知的 ISO 8859 标准一样, **只不过是一个简单的字符集表**. 它指定了一些与标准有关的术语, 定义了一些编码的别名, 并包括了规范说明, 指定了怎样使用 UCS 连接其他 ISO 标准的实现, 比如 ISO 6429 和 ISO 2022. 还有一些与 ISO 紧密相关的, 比如 ISO 14651 是关于 UCS 字符串排序的.

考虑到 Unicode 标准有一个易记的名字, 且在任何好的书店里的 Addison-Wesley 里有, 只花费 ISO 版本的一小部分, 且包括更多的辅助信息, 因而它成为使用广泛得多的参考也就不足为奇了. 然而, 一般认为, 用于打印 ISO 10646-1 标准的字体在某些方面的质量要高于用于打印 Unicode 2.0的. 专业字体设计者总是被建议说要两个标准都实现, 但一些提供的样例字形有显著的区别. ISO 10646-1 标准同样使用四种不同的风格变体来显示表意文字如中文, 日文和韩文 (CJK), 而 Unicode 2.0 的表里只有中文的变体. 这导致了普遍的认为 Unicode 对日本用户来说是不可接收的传说, 尽管是错误的.

## UTF-8

首先 **UCS 和 Unicode 只是分配整数给字符的编码表**. 现在存在好几种将一串字符表示为一串字节的方法. 最显而易见的两种方法是将 Unicode 文本存储为 2 个 或 4 个字节序列的串. 这两种方法的正式名称分别为 UCS-2 和 UCS-4. 除非另外指定, 否则大多数的字节都是这样的(Bigendian convention). 将一个 ASCII 或 Latin-1 的文件转换成 UCS-2 只需简单地在每个 ASCII 字节前插入 0x00. 如果要转换成 UCS-4, 则必须在每个 ASCII 字节前插入三个 0x00.

**在 Unix 下使用 UCS-2 (或 UCS-4) 会导致非常严重的问题**. 用这些编码的字符串会包含一些特殊的字符, 比如 '\\0' 或 '/', 它们在 文件名和其他 C 库函数参数里都有特别的含义. 另外, 大多数使用 ASCII 文件的 UNIX 下的工具, 如果不进行重大修改是无法读取 16 位的字符的. 基于这些原因, 在文件名, 文本文件, 环境变量等地方, **UCS-2** 不适合作为**Unicode** 的外部编码.

在 ISO 10646-1 [Annex R](https://www.cl.cam.ac.uk/~mgk25/ucs/ISO-10646-UTF-8.html) 和 [RFC 2279](ftp://ftp.funet.fi/mirrors/nic.nordu.net/rfc/rfc2279.txt) 里定义的 **UTF-8** 编码**没有**这些问题. **它是在 Unix 风格的操作系统下使用 Unicode 的明显的方法**.

UTF-8 有一下特性:

- UCS 字符 U+0000 到 U+007F (ASCII) 被编码为字节 0x00 到 0x7F (ASCII 兼容). 这意味着**只包含 7 位 ASCII 字符的文件在 ASCII 和 UTF-8 两种编码方式下是一样的**.
    
- 所有 >U+007F 的 UCS 字符被编码为一个多个字节的串, 每个字节都有标记位集. 因此, ASCII 字节 (0x00-0x7F) 不可能作为任何其他字符的一部分.
    
- 表示非 ASCII 字符的多字节串的第一个字节总是在 0xC0 到 0xFD 的范围里, 并指出这个字符包含多少个字节. 多字节串的其余字节都在 0x80 到 0xBF 范围里. 这使得重新同步非常容易, 并使编码无国界, 且很少受丢失字节的影响.
    
- 可以编入所有可能的 231个 UCS 代码
    
- UTF-8 编码字符理论上可以最多到 6 个字节长, 然而 16 位 BMP 字符最多只用到 3 字节长.
    
- Bigendian UCS-4 字节串的排列顺序是预定的.
    
- 字节 0xFE 和 0xFF 在 UTF-8 编码中从未用到.
    

下列字节串用来表示一个字符. 用到哪个串取决于该字符在 Unicode 中的序号.

<table border="1"><tbody><tr><td>U-00000000 - U-0000007F:</td><td>0<em>xxxxxxx</em></td></tr><tr><td>U-00000080 - U-000007FF:</td><td>110<em>xxxxx</em> 10<em>xxxxxx</em></td></tr><tr><td>U-00000800 - U-0000FFFF:</td><td>1110<em>xxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em></td></tr><tr><td>U-00010000 - U-001FFFFF:</td><td>11110<em>xxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em></td></tr><tr><td>U-00200000 - U-03FFFFFF:</td><td>111110<em>xx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em></td></tr><tr><td>U-04000000 - U-7FFFFFFF:</td><td>1111110<em>x</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em> 10<em>xxxxxx</em></td></tr></tbody></table>

xxx 的位置由字符编码数的二进制表示的位填入. 越靠右的 x 具有越少的特殊意义. 只用最短的那个足够表达一个字符编码数的多字节串. 注意在多字节串中, 第一个字节的开头"1"的数目就是整个串中字节的数目.

**例如**: Unicode 字符 U+00A9 = 1010 1001 (版权符号) 在 UTF-8 里的编码为:

> 11000010 10101001 = 0xC2 0xA9

而字符 U+2260 = 0010 0010 0110 0000 (不等于) 编码为:

> 11100010 10001001 10100000 = 0xE2 0x89 0xA0

这种编码的官方名字拼写为 UTF-8, 其中 UTF 代表 **U**CS **T**ransformation **F**ormat. 请勿在任何文档中用其他名字 (比如 utf8 或 UTF\_8) 来表示 UTF-8, 当然除非你指的是一个变量名而不是这种编码本身.

本文转自: [https://www.zeali.net/entry/86](https://www.zeali.net/entry/86)
