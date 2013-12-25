---
layout: post
title: "动态语言与静态语言"
description: ""
category: programming
tags: [programming]
---
{% include JB/setup %}

听多了动态语言、静态语言，我们会好奇什么是动态语言和静态语言。

根据[百度百科](http://baike.baidu.com/link?url=mkm0g0_FrVyQgVbvmwnc2FesBv5a_82p87uiKo0S1xHmrI_q37XjG6QG_r2P3iLDPorG8lwSfZhVOAxL6WjO__)的解释: 
> 动态语言，是指程序在运行时可以改变其结构：新的函数可以被引进，已有的函数可以被删除等在结构上的变化.而静态类型语言的类型判断是在运行前判断（如编译阶段）。

简而言之，可以理解为翻译时间是不同的，一个在运行时，一个在运行前。

尽管如此，早期编程语言尤其以静态语言为主，静态语言通过其运行效率、安全性等特性满足各种大型项目。而随着上个世纪90年代初期python和ruby等动态语言的发明，动态语言重新出现在人们的视线当中，逐渐活跃起来，成为广大编程人员所喜爱的工具。事实上，从第一门动态语言Lisp的发明，到shell脚本语言的出现，动态语言也可以说历史久远，但是为什么动态语言仅仅在最近几十年才活跃起来？

我想，这与历史不无关系。早期，计算机属于稀缺资源，运行效率的重要性不言而喻。而随着科技的发展，计算机资源已不在稀缺，而程序员本身的时间变得更加宝贵，如何提高开发效率已成为重中之重。动态语言凭借其简洁性，越来越受到大众的欢迎，它使开发效率更高，节省了更多程序员的时间。而随着技术的发展，动态语言的优化得到长远的进步，其运行效率也在不断提高。

这里，我们不得不提一下解释型语言。一般的静态语言，例如C/C++，我们先将其编译为可执行文件（机器可以读懂），再运行。而动态语言一般都有一个解释器，在运行程序时才翻译代码，且是一句一句的翻译，这也是为什么这些语言都叫做脚本语言。我们容易发现，一次编译完再执行显然快于每次运行再翻译。事实上，解释型语言对应的是编译型语言，按照翻译工具来分。静态语言与动态语言按照其特性来分。

最后，推荐一篇文章[编程语言和胖手指](http://www.aqee.net/programming-languages-vs-fat-fingers/)，从中我们可以发现，静态语言确实要比动态语言安全。

当然，无论是静态语言还是动态语言，都有其优缺点，我们需要权衡其利弊，找到最适合解决问题的选择才是最重要的。