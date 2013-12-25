---
layout: post
title: "给你的博客加上公益链接"
description: ""
category: 网页设计
tags: [web]
---
{% include JB/setup %}

看过了一些人的博客，发现都有关于公益的链接，于是决定在自己简陋的博客上也加上这个功能。如果你和我一样，使用github和jekyll搭建的博客，只需按照如下步骤即可完成，其他类型的博客也都大同小异，如关注更多公益，请访问<a href="http://yibo.iyiyun.com/" rel="me">益播公益</a>。

其实过程很简单，在此略做描述，也鼓励更多的人参与到公益中来。

我们只是将自己的404页面链接上公益网站的链接即可，先找到自己的404.html文件，即404页面（我的404.html就在根目录下）。在你的页面中输入如下代码：
{% highlight html %}
<title>Page Not Found</title>
<center><h1>Whoops looks like we lost one!</h1>
<iframe scrolling='no' frameborder='0' src='http://yibo.iyiyun.com/js/yibo404' width='735' height='700' style="display:block;"></iframe></center>
{% endhighlight %}
title中显示的是你的页面标题，center使你的内容居中，src链接了公益网站的网址，其他部分起到调整大小等作用。
第一步顺利完成后，找到你的主页文件，即显示你主页布局的文件（我的文件名是default.html，位置在_includes/themes/the-program中）。在适当的位置上，只需加入如下代码即可：
{% highlight html %}
<h3>支持公益，请狠敲</h3>
<a href="http://liuyu314.github.io/404" rel="me"><img src="/images/blogImgs/404.jpg"></a>
{% endhighlight %}
前者网址是我的404链接地址，后者则是通过点击图片链接该网址，当然你也可以通过文字链接到你的404页面。第三步（如果使用图片链接）选择一个你喜欢的图标，保存在你的图片目录下即可（我的图片目录是images/blogImgs）。

更新博客后，你会发现你的404页面改为公益链接（寻人启示），而你的主页上也有了公益链接的提示。

方法其实很简单，后面的就看大家自己去设计喜欢的款式了。最后，请大家更多的支持公益！
