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

{% highlight %}
-XX:CompileCommand=exclude,org/eclipse/core/internal/dtree/DataTreeNode,forwardDeltaWith
{% endhighlight %}
