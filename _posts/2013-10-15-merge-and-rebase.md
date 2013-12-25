---
layout: post
title: "git merge与git rebase的区别"
description: ""
category: Unix/Linux
tags: [git, linux]
---
{% include JB/setup %}

最近一直在看[《版本控制之道——使用Git》](http://book.douban.com/subject/4813786/)，学习了很多关于Git的使用方法，之前曾经写过一篇[《git的简单用法》](http://liuyu314.github.io/unix/linux/2013/03/17/git-usage/)，这次算是深入学习了Git。

<section>
<p><img src="/images/blogImgs/git.jpg"></p>
</section>

在合并分支时，我想大家一定也遇到过分不清git merge和git rebase的区别，我也在网上搜索了大量的文章，欲求弄清其区别，反而是越看越乱，感觉有种人云亦云。所以，还是自己做个实验了测试一下这两个命令的区别。顺便解释一下这两种合并的名称，merge即合并，rebase网上翻译为衍合，书上翻译为变址。

    a -> a1 -> a2 -> a3 -> a4
    |
    -> b1 -> b2

我们做两条分支，一条是a1开始，一条是b1开始，a1做主分支（master），b1做新分支（new）。我们来做一些详细的描述，请按照如下规则添加和提交：

    a    ->    b1    ->    a1    ->    a2    ->    b2    ->    a3    ->    a4
    hello    hello1     hello2      hello3      hello2      hello2     hello2 
    0          1           2           4           3           3            2

顺序采取第一行的顺序，第二行是新建文件的名字，第三行是在该文件内添加的内容（数字）。即在a时创建hello文件，并将0写入到文件，添加跟踪文件，提交。然后创建分支new，检出new分支，创建hello1文件并写入1，在添加跟踪文件，再提交。后同。

当所有的操作都完成时，我们切换到master上，输入：

    git merge new

此时我们会遇到冲突，只需将hello2的内容只保留2就可以，我们接着输入：

    git log -p

查看日志的变化，-p可以查看每次提交的具体变化，我将日志简化如下：
    
    Date:   Tue Oct 15 00:12:38 2013 +0800
       new-commit
    commit 9b5fe772955775733b54c230cd6db3c1df85cd67
    Date:   Tue Oct 15 00:11:09 2013 +0800
        a4
    diff --git a/hello2 b/hello2
    --- a/hello2
    +++ b/hello2
    -3
    +2
    commit 3e0183434767e9cfabba77fa439eb7db210fc0ef
    Date:   Mon Oct 14 23:31:30 2013 +0800
        a3
    diff --git a/hello2 b/hello2
    --- a/hello2
    +++ b/hello2
    -2
    +3
    commit a0ab95ef07960bb8b4e67101a2cb0088863e4f56
    Date:   Mon Oct 14 23:31:07 2013 +0800
        b2
    diff --git a/hello2 b/hello2
    --- /dev/null
    +++ b/hello2
    +3
    commit 529db000382f277e63c722d2544ff8a4c0aa2753
    Date:   Mon Oct 14 23:30:49 2013 +0800
        a2
    diff --git a/hello3 b/hello3
    --- /dev/null
    +++ b/hello3
    +4
    commit 93052e3a0464c33f236cda7a9fd125ecb0ef9bdf
    Date:   Mon Oct 14 23:30:21 2013 +0800
        a1
    diff --git a/hello2 b/hello2
    --- /dev/null
    --- /dev/null
    +++ b/hello2
    +2
    commit e0dc938d8f5e453dffc7d812ab5d50ff84b206c2
    Date:   Mon Oct 14 23:30:02 2013 +0800
        b1
    diff --git a/hello1 b/hello1
    --- /dev/null
    +++ b/hello1
    +1
    commit 57df279233bb04c391c5c649eb29cff4566a575e
    Date:   Mon Oct 14 23:29:38 2013 +0800
        a
    diff --git a/hello b/hello
    --- /dev/null
    +++ b/hello
    +0

简化来看会有如下几个特殊的地方：

1  首先合并后提交顺寻是按照提交时间顺序排序，即：

    a -> b1 -> a1 -> a2 -> b2 -> a3 -> a4 -> new

和我们之前设计的提交顺序是一致的，那么是否每次都要修改冲突呢？并没有，我们发现似乎a3与a4都有更改，事实上a3的变化是由于a3提交时，hello2的内容由2变更为3，而a4的变化是由于我们修改了冲突。这也将是我们要说的第二个特殊的地方。

2  最后合并的仅仅是b2和a4，并合成新的提交

合并的提交是两个分支的最新的提交，而修改冲突后，我们需要重新跟踪这个冲突文件，并提交新的new-commit。

3  每个提交还是原来的提交

如果你尝试回溯的这些提交的任意一个提交点，你会发现和原分支的提交没有任何改变，这就像是把两个分支按照提交时间顺寻捏在一起，像插空一样，只有最新的两个提交合并成为new-commit提交。

我们返回到合并前，尝试输入：

    git rebase new

当然还是会产生冲突，修改冲突并跟踪文件后，我们输入：

    git log -p

输出如下：

    commit fc4ebfe51ce46c0fa1fa66d72d58e62ca6535e27
    Date:   Tue Oct 15 00:11:09 2013 +0800
        a4
    diff --git a/hello2 b/hello2
    --- a/hello2
    +++ b/hello2
    -3
    +2
    commit abbb01c75c2d96a214f6d464b8596524ae67ce4d
    Date:   Mon Oct 14 23:31:30 2013 +0800
        a3
    diff --git a/hello2 b/hello2
    --- a/hello2
    +++ b/hello2
    -2
    +3
    commit 4268db8ee3b4c6575cc6126d2d8f4f0381e17a2d
    Date:   Mon Oct 14 23:30:49 2013 +0800
        a2
    diff --git a/hello3 b/hello3
    --- /dev/null
    +++ b/hello3
    +4
    commit bee0c065490606fad4f19b59961f32b6c2185307
    Date:   Mon Oct 14 23:30:21 2013 +0800
        a1
    diff --git a/hello2 b/hello2
    --- a/hello2
    +++ b/hello2
    -3
    +2
    commit a0ab95ef07960bb8b4e67101a2cb0088863e4f56
    Date:   Mon Oct 14 23:31:07 2013 +0800
        b2
    diff --git a/hello2 b/hello2
    --- /dev/null
    +++ b/hello2
    +3
    commit e0dc938d8f5e453dffc7d812ab5d50ff84b206c2
    Date:   Mon Oct 14 23:30:02 2013 +0800
        b1
    diff --git a/hello1 b/hello1
    --- /dev/null
    +++ b/hello1
    +1
    commit 57df279233bb04c391c5c649eb29cff4566a575e
    Date:   Mon Oct 14 23:29:38 2013 +0800
        a
    diff --git a/hello b/hello
    --- /dev/null
    +++ b/hello
    +0


1  首先合并后两个分支的提交是按照主分支添加都new分支的后面，即：

    a -> b1 -> b2 -> a1 -> a2 -> a3 -> a4

这就要提到这个变址的概念，在master分支执行git rebase new是指将master的父节点（分支的起点a）由a变更为b2，就像嫁接一样，把主分支嫁接到new分支的最新提交上，所以顺序如上。

2  b2和a1进行合并，后续连续改变

我们观察上面的显示，可以发现，b2与a1先产生冲突，我们解决冲突，后续的提交或者自动解决提交冲突，或者我们再手动解决提交冲突，本例中，只需解决一次冲突即可，后续自动解决冲突。所以，实际上a1到a4的提交已经不是原来的提交，而是经过冲突解决的提交。

3  无需合并后再提交一次

不同于git merge，变址不需要最后再提交一次（merge中的new-commit提交），而是直接以新的a4结尾。

讲到这里，你应该知道类似[这篇文章](http://gitbook.liuhui998.com/4_2.html)的图是什么意思了，而如[这篇文章](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)所提到的三方合并，我还不清楚到底有什么实际用途，但是在变址合并时，提示确实提到了三方合并。[这篇文章](http://www.cnblogs.com/iammatthew/archive/2011/12/06/2277383.html)提到了两种合并方法合并的提交顺序的改变，但并没有更详细的阐述。

那么什么时候用合并，什么时候用变址？很明显，改动较多，可能引起冲突较多时，使用git merge比较合适，毕竟只修改一次冲突，而git rebase可能会修改多次冲突，毕竟嫁接点之后的所有提交都会改变。但是git merge的缺点是把两个分支的提交捏在一起，可能会比较混乱，而git rebase的合并使提交成为线性，比较干净整洁。网上有人说不建议使用git rebase，可能会带来麻烦，由于自己实战经验不多，理解不深，此处不做过多评论。
