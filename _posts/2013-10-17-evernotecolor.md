---
layout: post
title: "如何修改印象笔记的笔记背景色"
description: ""
category: life
tags: [life]
---
{% include JB/setup %}


本来不打算写这篇博客，一方面，方法在网上也可以查到，另一方面，自己已经了记录方法，没必要再总结一次。但思前想后，毕竟总是从别人那获取也不好，还是要多多回馈给大家，于是重新总结了“如何修改印象笔记的背景颜色”这篇主题，添加了一些自己再研究中的心得。

印象笔记的编辑功能很弱，一直被用户所诟病，其实我倒是觉得这是印象笔记的一点特色——简约风格，同时让用户更加关注记录的内容而不是格式本身。但是话说回来，我们还是经常会希望编辑的笔记能够赏心悦目，当我们看到网上的背景色或字体色等很吸引人时，总会希望自己的笔记也能如此漂亮。

那先说说如果你喜欢某个字体的颜色该怎么办。其实很简单，只需要一个简单的工具查出颜色的数值，在笔记中设置一下颜色即可。比如mac下的“数码测色计”就不错。这是一款mac自带的工具，打开软件后选择“原生值”，你会发现你的鼠标选到哪里，就得到那个部分的RGB值。

<section>
<p><img src="/images/blogImgs/testcolor.jpg"></p>
</section>

有时候字体放大后，你会发现并不是每个地方都是一个数值，选择比较深的地方，整体的颜色会好很多。记录下RGB的值，进入印象笔记，打开字体的调色板，输入记录的RGB值就可以找到喜欢的字体颜色了，很简单吧。

<section>
<p><img src="/images/blogImgs/evercolor.jpg"></p>
</section>

那笔记的背景色该如何修改？首先你需要把你的RGB值转换为HEX值（十六进制值），不会转？没关系，找个工具就可以转，PS啊什么的，实在不行找个网站转换一下吧，这里有个可以转换的[网站](http://www.yellowpipe.com/yis/tools/hex-to-rgb/color-converter.php)。

记录这个HEX值，大概是“#ED1234”这样的格式。下一步，随便新建一个笔记，在笔记中随便输入些内容，然后导出该笔记。什么，不会导出？很简单，选中笔记，点击窗口最上面的“文件”，选择“导出文件”即可。使用记事本之类的软件打开这个导出的笔记。你应该会看到如下的内容：

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE en-export SYSTEM "http://xml.evernote.com/pub/evernote-export3.dtd">
    <en-export export-date="20131017T120611Z" application="Evernote" version="Evernote Mac 5.4.1 (402012)">
    <note><title>新笔记名称</title><content><![CDATA[<?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <!DOCTYPE en-note SYSTEM "http://xml.evernote.com/pub/enml2.dtd">
    <en-note style="word-wrap: break-word; -webkit-nbsp-mode: space; -webkit-line-break: after-white-space;">
    内容<span style="-evernote-last-insertion-point:true;"/>
    </en-note>
    ]]></content><created>20131017T120557Z</created><updated>20131017T120602Z</updated><note-attributes><latitude>30.75826315828443</latitude><longitude>103.9328063650891</longitude><altitude>542.8571166992188</altitude><author>yuliu314</author><reminder-order>0</reminder-order></note-attributes></note>
    </en-export>


在第六行那里，修改成如下代码：

    <en-note bgcolor="#ED1234" style="word-wrap: break-word; -webkit-nbsp-mode: space; -webkit-line-break: after-white-space;">

其实就是在en-note后面加上了bgcolor="#ED1234"，引号中的就是之间转换的数值。保存后用印象笔记重新导入就会看到笔记的背景颜色改变了。如图：

<section>
<p><img src="/images/blogImgs/module.jpg" width='580' height='190'></p>
</section>

好看了许多吧，将这个笔记保存为模板，以后复制一下这个笔记就可以了。无论笔记多么炫目，真正的价值在它的内容，善用印象笔记。