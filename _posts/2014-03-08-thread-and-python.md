---
layout: post
title: "python多线程"
description: ""
category: "python"
tags: [Python]
---
{% include JB/setup %}

前两天在豆瓣上看到一个相册，想把所有的图片都下载下来，但又懒于一张一张的下载（没错，我就是一个懒惰的程序员），所以就打算写个python代码来批量下载这些图片（貌似叫做“网络爬虫”云云）。完成后（其实写代码的时间足以将图片一张一张下载完），又想优化代码，在网上搜了一段代码，改了改，真的大大加快了速度。这种东西叫做——多线程。

好吧，讲了一大堆背景，先看一下代码（这段代码来自网络，后面会附上网址，我仅仅做了一些注释）：

{% highlight Python %}
import urllib2
from threading import Thread,Lock
from Queue import Queue
import time
 
class Fetcher:
    def __init__(self,threads):
        self.opener = urllib2.build_opener(urllib2.HTTPHandler)
        self.lock = Lock() #线程锁
        self.q_req = Queue() #任务队列
        self.q_ans = Queue() #完成队列
        self.threads = threads
        for i in range(threads):
            t = Thread(target=self.threadget) #括号中的是每次线程要执行的任务
            t.setDaemon(True) #设置子线程是否随主线程一起结束，必须在start()
                              #之前调用。默认为False
            t.start() #启动线程
        self.running = 0 #设置运行中的线程个数
 
    def __del__(self): #解构时需等待两个队列完成
        time.sleep(0.5)
        self.q_req.join() #Queue等待队列为空后再执行其他操作
        self.q_ans.join()
 
    #返回还在运行线程的个数，为0时表示全部运行完毕
    def taskleft(self):
        return self.q_req.qsize()+self.q_ans.qsize()+self.running 

    def push(self,req):
        self.q_req.put(req)
 
    def pop(self):
        return self.q_ans.get()
 
	#线程执行的任务，根据req来区分 
    def threadget(self):
        while True:
            req = self.q_req.get()

            # Lock.lock()操作，使用with可以不用显示调用acquire和release，
            # 这里锁住线程，使得self.running加1表示运行中的线程加1，
            # 如此做防止其他线程修改该值，造成混乱。
            # with下的语句结束后自动解锁。

            with self.lock: 
                self.running += 1
            try:
                ans = self.opener.open(req).read()
            except Exception, what:
                ans = ''
                print what
            self.q_ans.put((req,ans)) # 将完成的任务压入完成队列，在主程序中返回
            with self.lock:
                self.running -= 1
            self.q_req.task_done() # 在完成一项工作之后，Queue.task_done()
                                   # 函数向任务已经完成的队列发送一个信号
            time.sleep(0.1) # don't spam
 
if __name__ == "__main__":
    links = [ 'http://www.verycd.com/topics/%d/'%i for i in range(5420,5430) ]
    f = Fetcher(threads=10) #设置线程数为10
    for url in links:
        f.push(url)
    while f.taskleft():
        url,content = f.pop()
        print url,len(content)
{% endhighlight %}

如果想下载图片，首先要把上面links队列里面改为图片的地址。接下来在Fetcher那个类中的线程任务的方法中加入下载图片的代码即可。

参考：

- [用python爬虫抓站的一些技巧总结](http://www.open-open.com/lib/view/open1375945149312.html)
- [Python多线程学习](http://www.cnblogs.com/tqsummer/archive/2011/01/25/1944771.html)
- [python Queue模块](http://blog.csdn.net/yatere/article/details/6668006)
- [Python线程同步机制: Locks, RLocks, Semaphores, Conditions, Events和Queues](http://yoyzhou.github.io/blog/2013/02/28/python-threads-synchronization-locks/)