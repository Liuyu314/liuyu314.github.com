---
layout: post
title: "小谈缓冲区溢出"
description: ""
category: os
tags: [os, c]
---
{% include JB/setup %}

C/C++对于数组不进行任何边界检查，因此造成的bug数不胜数，最近学习Java，知道Java会自动检查数组是否越界，这可真是个好特性。不检查数组边界极易产生严重的问题——缓冲区溢出：

> 缓冲区溢出：输入到一个缓冲区或者数组保存区域的数据量超过了其容量，从而导致覆盖了其他信息的一种状况。

举一个例子，该例来自[《操作系统：精髓与设计原理》](http://book.douban.com/subject/5064311/)，我做了一些改变：

{% highlight c %}
#include <stdio.h>
#include <string.h>

int main()
{
    int valid = 0;
    char str1[8];
    char str2[8];

    gets(str2);
    if (strncmp(str1, str2, 8) == 0)
        valid = 1;
    printf("buffer1: str1(%s), str2(%s), valid(%d)\n", str1, str2, valid);
    return 0;
}
{% endhighlight %}

编译该程序，也许你的编译器会提示你：

    warning: this program uses gets(), which is unsafe.

没错，我们就要使用这个“倒霉”的**gets()**函数，这个函数从它产生开始就引入各种问题。gets函数只简单的从标准输入中读取一行，再把它送回到标准输出。我们来测试一下这段代码：

    gcc bufferoverflow.c -o bufferoverflow

    ./bufferoverflow
    abc //input
    buffer1: str1(), str2(abc), valid(0) //output

    abcdefghijklmnop //input
    buffer1: str1(ijklmnop), str2(abcdefghijklmnop), valid(0) //output

    abcdefghabcdefgh //input
    buffer1: str1(abcdefgh), str2(abcdefghabcdefgh), valid(1) //output

这段代码的作用是判断str1和str2是否相等，若想等，则valid为1，否则为0。显然我们并未往str1输入任何数据，但是第三个测试却使valid为1，即这两个数组相等。其实道理很简单，我们在str2中写入了16个字符，由于gets函数并不检查数据是否越界，则这16个字符都赋给了str2。而str2的数据空间只能包含8个字符，多出的字符自动赋给了str1。由于这段数据特点为两段相等的字符串（每8个字符），比较时仅仅比较str1和str2的8个字符，故相等。

为什么上例中，打印信息显示str2有16个字符的输出？因为gets收到换行符后在字符串结尾加上null标识符，str2数组输出时找到这个null才结束输出。

如果定义str1和str2的位置调换，则输出会有所不同，即：

{% highlight c %}
int main()
{
    int valid = 0;

    char str2[8];
    char str1[8];
    ...
}
{% endhighlight %}

输入：

    ./bufferoverflow
    abcdefghabcdefgh //input
    buffer1: str1(), str2(abcdefghabcdefgh), valid(1684234849) //output
    Abort trap: 6 //output

不同的环境会有不同的输出，有的环境会直接报错。之所以会这样，是str1和str2的存储位置所决定的，该例子为：

    栈帧  //高地址
    valid
    str2
    str1 //低地址

当向str2写数据时，str2由低地址向高地址写数据，超出的部分直接覆盖栈帧等（也会覆盖valid），而不会覆盖str1。故会报错（该环境下则是str1没有任何数据，且valid数据异常，并丢出中止）。而第一个例子则是：

    栈帧
    valid
    str1
    str2

向str2写数据，则由低地址到高地址，会覆盖到str1，且数据刚好没有覆盖valid。

如果覆盖到重要的数据部分，则程序会出现严重的错误。当有些重要的寄存器被覆盖时，程序不能恢复（如第二例）。而攻击者会利用缓冲区溢出，覆盖返回地址，而把程序的返回地址修改到他希望的地方，用以执行攻击代码。

不要急，现代计算机使用了多种方法来降低缓冲区溢出的危害。广义的保护策略分为：

<ul>
<li>编译时防御系统：目的是强化系统以抵制潜伏于新程序的恶意攻击。</li>
<li>运行时防御系统：目的是检测并中止现有程序中的恶意攻击。</li>
</ul>

GCC编译器也提供了像栈随机化和栈破坏检测，以及限制可执行代码的区域等技术来限制入侵者。

我们不能仅仅寄希望于这些防御技术或编程语言自身的特性，还要养成良好的编程习惯和素养，才能以不变应万变。

参考资料

<ul>
<li>《深入理解计算机系统》</li>
<li>《操作系统：精髓与设计原理》</li> 
</ul>