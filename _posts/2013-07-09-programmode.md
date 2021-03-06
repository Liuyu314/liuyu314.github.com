---
layout: post
title: "有关编程范型的思考"
description: ""
category: programming
tags: [programming]
---
{% include JB/setup %}

最近看了一篇文章[声明式编程和命令式编程的比较](http://blog.jobbole.com/42178/?utm_source=feedly)，学习了两个新词：命令式编程和声明式编程。

来看看文中是如何解释这两个名词的：

> 命令式编程：命令“机器”如何去做事情(how)，这样不管你想要的是什么(what)，它都会按照你的命令实现。

>声明式编程：告诉“机器”你想要的是什么(what)，让机器想出如何去做(how)。

文中使用javascript举了两个例子：

>举个简单的例子，假设我们想让一个数组里的数值翻倍。

>我们用命令式编程风格实现，像下面这样：

{% highlight javascript %}
var numbers = [1,2,3,4,5]
var doubled = []
for(var i = 0; i < numbers.length; i++) {
	var newNumber = numbers[i] * 2
	doubled.push(newNumber)
}
console.log(doubled) //=> [2,4,6,8,10]
{% endhighlight %}
>我们直接遍历整个数组，取出每个元素，乘以二，然后把翻倍后的值放入新数组，每次都要操作这个双倍数组，直到计算完所有元素。

>而使用声明式编程方法，我们可以用 Array.map 函数，像下面这样：

{% highlight javascript %}
var numbers = [1,2,3,4,5]
var doubled = numbers.map(function(n) {
	return n * 2
}
console.log(doubled) //=> [2,4,6,8,10]
{% endhighlight %}
>map 利用当前的数组创建了一个新数组，新数组里的每个元素都是经过了传入map的函数(这里是function(n) { return n\*2 })的处理。

>map函数所作的事情是将直接遍历整个数组的过程归纳抽离出来，让我们专注于描述我们想要的是什么(what)。注意，我们传入map的是一个纯函数；它不具有任何副作用(不会改变外部状态)，它只是接收一个数字，返回乘以二后的值。
初看一眼，感觉挺高端的，但是细想一下又觉不妥，上面例子中，第一个是一步一步的计算，第二个是用个高级的map一步到位。那我们可不可以把第一个一步一步的过程写成一个函数，把值传进去得到输出，似乎与第二个有点那么类似了。第二个例子只是有个写好的“map”，所以不用关注“how”。

晕头转向了吧，我们来看看维基百科怎么定义[命令式编程](http://zh.wikipedia.org/zh-cn/%E6%8C%87%E4%BB%A4%E5%BC%8F%E7%B7%A8%E7%A8%8B)和[声明式编程](http://zh.wikipedia.org/zh-cn/%E5%A3%B0%E6%98%8E%E6%80%A7%E7%BC%96%E7%A8%8B)的：

>声明式编程（英语：Declarative programming）是一种编程范型，采用了和命令式编程对立的方向。它描述目目标性质，让电脑明白目标是什么。声明式编程通过函数、推论规则或项重写（term-rewriting）规则，来描述变量之间的关系。它的语言运行器（编译器或解释器）采用了一个固定的算法，以从这些关系产生结果。声明式编程语言通常用作解决人工智能和约束满足问题。

>命令式编程（英语：Imperative programming），是一种描述电脑所需作出的行为的编程范型。几乎所有电脑的硬件工作都是指令式的；几乎所有电脑的硬件都是设计来运行机器码，使用指令式的风格来写的。较高级的指令式编程语言使用变量和更复杂的语句，但仍依从相同的范型。食谱和行动清单，虽非电脑程序，但与命令式编程有相似的风格：每步都是指令，有形的世界控制情况。因为命令式编程的基础观念，不但概念上比较熟悉，而且较容易具体表现于硬件，所以大部分的编程语言都是指令式的。

有点明白了，命令式编程每部都是具体的命令，运行时就是一步一步的执行。而声明式编程是通过函数、推论规则或项重写规则来实现整个过程。命令式编程需要程序员知道每一步都是如何做的，而声明式编程不需要程序员了解每部都怎么做，只需知道最后要什么，再知道个实现它的规则就好。

所以，就算我们在第一个例子中自己实现一个函数，那也不算是声明式编程，因为这个规则还是程序员所清楚的，是程序员设计的如何去做。

那么看起来声明式编程比命令式编程高级多了，一个就像工人，一个就像老板。别高兴太早，我们只要查查维基百科的[编程范型](http://zh.wikipedia.org/wiki/%E7%B7%A8%E7%A8%8B%E5%85%B8%E7%AF%84)就会吓一跳，竟然有如此多的编程范型，举几个大家听过的例子：

>结构化编程：结构化程序设计（英语：Structured programming），一种编程范型。它采用子程序、代码区块、for循环以及while循环等结构，来取代传统的 goto。希望借此来改善计算机程序的明晰性、质量以及开发时间，并且避免写出面条式代码。

>函数式编程：函数式编程（英语：Functional programming）或者函数程序设计，又称泛函编程，是一种编程范型，它将电脑运算视为数学上的函数计算，并且避免状态以及可变数据。函数编程语言最重要的基础是 λ 演算（lambda calculus）。而且λ演算的函数可以接受函数当作输入（引数）和输出（传出值）。

>面向对象编程：面向对象程序设计（英语：Object-oriented programming，缩写：OOP），指一种程序设计范型，同时也是一种程序开发的方法。对象指的是类的集合。它将对象作为程序的基本单元，将程序和数据封装其中，以提高软件的重用性、灵活性和扩展性。

这还不算，我们来看看维基百科给出的编程范型的层次图（图太长，不在此截图了，图在[编程范型](http://zh.wikipedia.org/wiki/%E7%B7%A8%E7%A8%8B%E5%85%B8%E7%AF%84)的最右侧）。

有这么多种的编程范型，且有很多层次关系！比如，函数式编程属于声明式编程。面向对象编程属于结构化编程，而这两者都属于命令式编程。看来命令式编程和声明式编程是两大阵营啊。

搞什么啊，这么多怎么学？其实我们并不是学习编程方法论而是学习编程，重要的学习思想，萝卜白菜各有所爱，你完全可以不管什么编程范型，能解决问题就是好范型，除非你是某个范型的拥趸。。。
