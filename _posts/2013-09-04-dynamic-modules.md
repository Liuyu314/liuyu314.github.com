---
layout: post
title: "Python 动态加载模块"
description: ""
category: Python
tags: [Python]
---
{% include JB/setup %}

使用python加载模块时，我们习惯于：  
{% highlight python %}
import os, sys
{% endhighlight %}

但是当我们想动态加载模块时，这种方法就行不通了。假设一种情景，我们有若干个模块要加载，但是数量我们不清楚或是可变，模块被放置在一个列表中。

那么我们来编写代码实现：  
{% highlight python %}
# module1.py
def hello1():
     print "hello1"
{% endhighlight %}

{% highlight python %}
# module2.py
def hello2():
     print "hello2"
{% endhighlight %}
我们使用__import__来实现动态加载这两个模块：  
{% highlight python %}
# test.py
#import module1, module2 #old method

module1 = __import__('module1')
anothername = __import__('module2')

module1.hello1()
module2.hello2()
anothername.hello2()
{% endhighlight %}
我们可以发现错误提示：   

    NameError: name 'module2' is not defined   

若我们把：**module2.hello2()**注释掉，代码能够成功打印。事实上经过__import__，模块真正的名字已经变成module1和anothername，如果你仅仅输入**"__import__('module1')"**而不赋给任何变量，模块是找不到的。  


细节弄清了后，我们来看看更复杂的实现：  
{% highlight python %}
# test2.py
#import module1, module2 #old method

#module1 = __import__('module1')
#anothername = __import__('module2')

modules = ['module1', 'module2']
mode = map(__import__, modules)

mode[0].hello1()
mode[1].hello2()
{% endhighlight %}

map的作用是将逗号前的方法遍历作用在后面的数据结构中。请注意括号里面是“逗号”，而不是“点”或“括号”。就如前面提到的，这时模块名其实是"mode[0]"、"mode[1]"。。。

这时也许你会提出更多要求，比如假设我们不知道模块名都是什么，当我们动态加载一个模块时，希望将模块名就命名为原模块的名字。

我的方法是：  
{% highlight python %}
# test3.py

modules = ['module1', 'module2']
for i in modules:
     exec("import " + i)

module1.hello1()
module2.hello2()
{% endhighlight %}

以上几种方法基本能够满足各种要求，我相信要实现python更加智能化，动态加载模块是必不可少的。

更多请查看[深入python](http://woodpecker.org.cn/diveintopython/functional_programming/dynamic_import.html)
