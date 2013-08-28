---
layout: post
title: "elipse crashed when use sun jdk6"
description: ""
category: IDE
tags: [eclipse jdk crashed]
---
{% include JB/setup %}

我使用的是Ubuntu 12.04 64bit的机器，使用Android的draw9patch工具时，出现异常无法运行，得知是open jdk的问题，于是安装了sun jdk6。这下draw9patch可以运行了，但是Eclipse却运行就闪退，从终端启动Eclipse得到一个错误的标识符 libjvm.so+0x1f125a ，放Google大神一搜，答案出现了。

在eclipse.ini里加入如下代码即可解决问题

{% highlight java %}
	-XX:CompileCommand=exclude,org/eclipse/core/internal/dtree/DataTreeNode,forwardDeltaWith
{% endhighlight %}

----------------------------------------------------------

其实上面的方法还是不能解决这个问题，使用sun jdk的时候，eclipse常常崩溃，无奈只好使用open jdk, 但是draw9patch依然不能正确运行。解决方案：copy swing-worker.jar into /path_to_android_sdk/tools/lib. That's all!
