---
layout: post
title: "小谈预编译头文件"
description: ""
category: C
tags: [C]
---
{% include JB/setup %}

不知道你在网上有没有见过一个奇怪的现象，使用Gcc编译时，有人会这样：

    gcc hello.c hello.h -o hello

如果有使用过Gcc编译器的经验，可以知道通常我们只会编译c文件，而不会编译h文件。事实上，在编译器的预处理阶段，include的头文件都展开在c的代码中了。那么为什么这里会出现如此奇怪的用法呢？

抱着这个问题，我在stackoverflow发了一个[帖子](http://stackoverflow.com/questions/17416719/do-i-need-to-compile-the-header-files-in-a-c-program)，得到了若干答案。事实上，编译时加入头文件实际上并没有什么意义，头文件会被当作独立的单元被编译。但有一种方法叫做预编译头文件——[Precompiled header](http://en.wikipedia.org/wiki/Precompiled_header)，这种方法将头文件预先编译。

为什么要预编译头文件？

假设你的头文件中有大量的代码，而包括该头文件的c文件很多，这无疑会造成编译器的巨大负担，整个编译时间会很长。[这篇文章](http://blog.csdn.net/btooth/article/details/980251)做了很好的解释：

> 预编译头文件将一些项目中普遍使用的头文件内容的词法分析、语法分析等结果缓存在一个特定格式的二进制文件中；当然编译实质C/C++源文件时，就不必从头对这些头文件进行词法语法分析，而只需要利用那些已经完成词法-语法分析的结果就可以了。

同时，他也为我们解释了为什么在windows下使用vs时，会要求包含一个stdafx.h头文件。这个头文件就是预编译头文件，如果有代码量很大的头文件或源代码，加到这个头文件中使其预编译会提升很大的速度。

到这里也许你会有疑问，那编译好的头文件在哪里？其实，我们将头文件预编译为gch文件，即最终头文件被预编译为header.h.gch。

Gcc实际上已经拥有了[这项技术](http://gcc.gnu.org/onlinedocs/gcc/Precompiled-Headers.html)，但绝不像我们上面的那句简单的编译。我们先写好hello.c，hello.h，然后输入下面这条命令：

    gcc -c hello.h -o hello.h.gch
    gcc hello.c -o hello
    ./hello

大功告成，即使你预编译头文件过后删除头文件，c代码的编译都可以成功，因为c文件会找到hello.h.gch文件并进行解析。

如果你还有疑问，在将来某一天遇到巨大的头文件时，不妨尝试一下预编译头文件的方法，也许会让编译时间有质的减少。