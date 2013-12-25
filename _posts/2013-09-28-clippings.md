---
layout: post
title: "使用Python自动选取kindle paperwhite中的摘抄文件"
description: ""
category: Python
tags: [Python]
---
{% include JB/setup %}

使用kindle paperwhite的同学都知道，我们可以方便的选取文字进行标注，将标注摘抄至My Clippings.txt文件中。但是当我们在同时阅读多本书时，该文件中的摘抄将会非常混乱，需要人工的将内容选取出来。故我编写了一个简单的python脚本程序，实现自动选取摘抄文件并提取其中所需内容。

我们可以看到kindle paperwhite（其他类型请自行更改）中的My Clippings.txt文件格式如图：

<p><img src="/images/blogImgs/clippings.jpg"></p>

我们只需将这个文件与python代码拷贝至同一文件夹并运行该程序即可，代码如下：

{% highlight python %}
#!/usr/bin/python
#coding=utf-8

import os
import sys

'''
	This program can extract the clippings from the file "My Clippings.txt"
	You can input a number to choose a bookname and the contents will output
	in the "Extract.txt".

	Enjoy it! :)
'''

if not os.path.exists("My Clippings.txt"):
	print "Error: Missing My Clippings.txt"
	sys.exit()
else:
	num = 1
	book_names = []
	f = open("My Clippings.txt", 'r')
	while True:
		line = f.readline()
		if len(line) == 0:
			break
		next_line = f.readline()
		if len(next_line) == 0:
			break
		if next_line.startswith('-'):
			if not line in book_names:
				book_names.append(line)
				print "%d: %s" %(num, line),
				num += 1
	f.seek(0, 0)
	while True: 
		raw = input("Enter a number to choose the book:\n")
		if raw < num + 1 and raw > 0:
			break
		print "Error number"

	new_f = open("Extract.txt", 'w')
	while True:
		line = f.readline()
		if len(line) == 0:
			break
		if line == book_names[raw - 1]:
			line = f.readline()
			line = f.readline()
			line = f.readline()
			if len(line) == 2:
				pass
			else:
				new_f.write(line)
	new_f.close()
	f.close()
	print "\nCompleted! check the Extract.txt"

{% endhighlight %}

该代码可以实现将所有文件中涉及的书籍名列出，并根据用户的选择将指定书籍下的摘抄拷贝至同一目录下的Extract.txt文件中。Enjoy it and have fun!