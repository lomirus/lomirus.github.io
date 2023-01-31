---
title: "生成语法入门系列（一）：头部方向参数 (Head-Directionality Parameter)"
description: "生成语法，掉光头发"
date: 2023-01-28T23:11:00+08:00
lastmod: 2023-01-30T00:04:00+08:00
image: "cover.webp"
categories: ["Language"]
tags: ["language", "lingustics", "syntax", "generative grammar"]
slug: "head-directionality-parameter"
---

## Background

最近在研究人工语言，特别是语序问题，不由得涉及了头部方向参数、X-bar Theory 等领域的知识，而且也这才意识到了乔姆斯基到底有多强orz。所以后面就着手写一下学习这些东西的一些笔记汇总，希望能帮到后面会用到的人。

目前计划分三篇文章完成，本篇主要介绍句法学的入门知识，第二篇会讲 X-bar Theory，第三篇则会涉及当前 generative grammar 的最新研究成果 minimalist program。

下面开始正文。

## Syntactic Category

在讲 syntactic category 之前，现引入两个新概念，lexical categories 和 phrasal categories。前者 lexical categories 就是我们在中学时学过的单词分类，比如动词，名词，副词等，lexical 即“语素的”之意。而 phrasal categories 在功能上与前者类似，不过针对的不是语素，而是短语，比如名词短语，动词短语等。

Syntactic category 则是句法理论中所假定的一种句法单元。在 phrase structure grammars 中，syntactic category 包括 lexical categories 和 phrasal categories。但是另一个理论 dependency grammars 中，syntactic category 则不承认 phrasal categories，只包括 lexical categories。本文依照前者为讨论基础。

由于在后面的讨论中，主要用到的是其英文及其缩写，所以先简单列举一下常见的词性：

| 中文       | 英文              | 缩写 |
| ---------- | ----------------- | ---- |
| 名词短语   | noun phrase       | NP   |
| 动词短语   | verb phrase       | VP   |
| 形容词短语 | adjective phrase  | AP   |
| 副词短语   | adverb phrase     | AdvP |
| 介词短语   | adposition phrase | PP   |
| 限定词短语 | determiner phrase | DetP |

## Head

在语言学中，对某个短语来说，我们用在这个短语中决定了该短语的 syntactic category 的词语称之为 head.

由于汉语本身的语序导致的歧义现象（这也是本系列要讨论的问题之一），所以下面我先用英语举几个例子：

- cute cat 是一个 noun phrase，因此其 head 是 cat
- eat fish 是一个 verb phrase，因此其 head 是 eat
- too cute 是一个 adjective phrase，因此其 head 是 cute
- too slowly 是一个 adverb phrase，因此其 head 是 slow

具体鉴定方法可以参考此处：[Heads - Grammar - Cambridge Dictionary](https://dictionary.cambridge.org/grammar/british-grammar/heads)。

## Head-initial & Head-final

在一个短语中，如果其 head 位于开头，我们称之为 head-initial，反之则称之为 head-final。这里继续用上面的例子：

- cute cat 中 cat 位于后面，所以是 head-final
- eat fish 中 eat 位于前面，所以是 head-final
- too cute 中 cute 位于后面，所以是 head-final
- too slowly 中 slowly 位于后面，所以是 head-final

## Head-Directionality Parameter

在语言学中，我们按照一个语言是 head-initial 还是 head-final，讲他们进行分类，并将这一参数称之为 head directionality。显然地。如果一个语言是 head-initial，那么以为它是 VO 结构，例如英语；如果是 head-final，则是 OV 结构，例如英语。

这时，我猜你可能会很疑惑：如果英语真的是纯粹的 head-initial，那么英语应当出现类似西班牙语的定语后置的现象，但是在英语中，尽管存在后置的定语从句，但是一般的形容词依旧是放在名词前面的，那么这就与 head-initial 相抵触了。

事实上，这么想的人不只你一个，我也一样。与其将语言粗暴地分成两个极端，不如在一个 continuum 的两端放上 head-final 和 head-initial，每一个语言都是条上的一点，而其语序越接近哪一个，就越靠近哪一端。浙大的刘海涛教授在2010年利用了 dependency treebank-based 方法，对二十余种语言进行了调查，而这次研究为该观点提供了有力的支持。在这种观念下，诸如英语或是德语将在 continuum 的相对中间的位置，而日语则会被放在靠近 head-final 的一端。

## Beyond this Article

终于知道为什么之前读其他大佬写东西总是中文里掺杂着一堆英文了。自己最近为了写这篇文章，查阅到的资料也基本都是英文文献。一是考虑是翻译的不可靠性，二是考虑到翻译所导致的多余劳动量，简单说就是吃力不讨好，所以干脆还是直接以英文原文为准吧，方便大家也方便自己。