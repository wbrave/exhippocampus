---
layout: page
title: 手机分配短讯id的面试题目（上篇：厘清需求）
categories:
    - misc
---

前阵子，笔者在 TopLanguage 论坛里参与讨论了一个不错的面试题目，在此和大家分享，也当作个人的讨论总结。本文列出该问题，并模拟应试者向面试官的对话，以厘清问题需求。

## 题目原文

事缘 [Dbger](http://www.cnblogs.com/baiyanhuang/) 发起的帖子中，[liuxinyu](https://sites.google.com/site/algoxy) 举了一个面试题目，原文如下：

> 有个老的手机短信程序，由于当时的手机CPU，内存都很烂。所以这个短信程序只能记住256条短信，多了就删。
> 
> 每个短信有个唯一的ID，在0到255之间。当然用户可能自己删短信，现在我们收到了一个新短信，请分配给它一个唯一的ID。说白了，就是请你写一个函数：
>
~~~cpp
byte function(byte* ids);
~~~
>
> 该函数接受一个`ids`数组，返回一个可用的ID，由于手机很破，我要求你的程序尽量快，并少用内存。注意：`ids`是无序的。

## 厘清需求

笔者认为，面试时，并不一定要急着解答问题。面试（interview）并非考试（examination），面试中双方可透过交谈去评估对方是否合适的雇主/雇员。若对面试题目有疑问，应尽量沟通厘清。这也反映实际软件开发时，程序员须具备足够的问题需求分析和沟通能力。笔者相信，大部分负责任的面试官，也喜见应试者提出切合实际的问题。

以下笔者尝试以对话形式，虚构一个厘清需求的情境。 （甲＝面试官，乙＝应试者）

> 甲微笑着说：你对纸上的题目清楚么？

乙认真地看着试题，犹豫片刻。

> 乙突然发问：在这个函数原型中，`ids`应该是一个非固定长度的数组，代表现时已分配的ID集合。那么该函数怎样可以得知ids数组的长度呢？我想到，由于0-255都是id的范围，似乎不可用简单的结束符，例如像null-terminated string那样用0去表示数组的结束。

突然而來的长问题，轮到甲犹豫了，立刻把纸上题目快读一遍，发现似乎真是个问题。

> 甲：嗯……这是个好问题，可能是出题同事的手误。这样吧，加上一个`size_t n`的参数，代表`ids`数组的有效元素数目。

乙轻轻点头，觉得这个修正最为简单直觉。之后更放心把想到的问题都提出来。

> 乙：想确认一点，`n`是否必然少于`255`，换句话说，该函数必然能正常输出一个`id`？
> 
> 甲：是的。本题目不要求处理其他输入错误的情况，例如`ids`为`NULL`、`ids`里有重复值等。可假定输入数据都是有效的。
> 
> 乙：想确认一点，传回的id只要不存在于`ids`数组便可，不需传回最小的id吧？
> 
> 甲：没错，没这个需求。
> 
> 乙有点不好意思地说：对于函数原型，我还有个问题……

甲心想不是还有手误吧。但甲还是带着微笑，示意继续问。

> 乙：其实也是关于`ids`数组的。由于原型没写明为`const byte *`，想问此函数可否改动`ids`数组的内容？或者再进一步说，此函数是否[纯函数（pure function）](http://en.wikipedia.org/wiki/Pure_function)，没有副作用（side effect）？

甲松了一口气，因为这不算是问题的缺陷。总算保住公司的面子。

> 甲充满信心地说：对于ids数组的内容是否能改变，你可分为两个情况去考虑。而除了这个参数之外，此函数不会有其他副作用。
> 
> 乙：哦，明白了。此外，虽然我未考虑周详，但似乎要获得更高运行效率，便需要额外的存储状态，或许要设另一个接口才行。
> 
> 甲喜形于色地回答：是的，也许可以透过空间换取时间。那么你对接口有提议么？
> 
> 乙边说边写：我想，或许另一种接口的形式是这样的……
> 
~~~cpp
struct manager {
     // 这里有一些状态变量（暂未决定）
     byte alloc();
     void dealloc(byte id);
};
~~~
> 
> 甲：好。这个是比原题目更抽像的情况。若时间足够，你也可在答题时对这种接口进行补充。
> 
> 乙：最后想问，该手机的CPU是32位的吧？

甲没有预先对此设限，就顺着回答了。

> 甲：就当作是32位的吧。
> 
> 乙：我暂时没想到其他问题了。
> 
> 甲：那好，你可以开始在纸上作答，我们一小时后再讨论。
> 
> 乙：谢谢您。我现在对题目的需求应有充分理解了。我会尽量想出多个解。

甲觉得乙有细心阅读题目，发问的内容和技巧也不错。乙也认为甲能耐心地聆听，并给与合理清晰的指示，甚至给与他一些弹性、一些发挥的机会。

最后一句中，乙提及的「多个解」也使甲留下印象和期待。因为很多时候，某个问题并不一定有最优解，能设想多个解，并分析比较每个解的优缺点，能体现程序员的基础能力。

## 预告

解答篇应于本周五发表（[已如期发表]({% post_url /misc/2010-09-03-idalloc-solution %})）。有兴趣的读者不妨自己试试，多想几个不同的解。若不熟悉 C/C++，也可用诸如 C#、Java等语言作答。其实本题并不难，适合一般编程职位的面试。笔者将会提供的解，有些来自笔者，有些来自网友，也非什么神奇特别的解，只是作为讨论的总结，和大家分享。也许读者会想到更好的解呢。

