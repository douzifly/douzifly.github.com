---
layout: post
title: "Android tips"
description: "Android 开发的一些经验总结"
category:
tags: [Android]
---
{% include JB/setup %}

> 时间流逝，开发Android已经快2年了。在这期间，遇到了很多问题，也解决了很多问题，有必要在这里做一个记录总结。汇集成为自己的Tip集合吧。

#### ADB 技巧：
* adb root 访问：
> 有时候，需要把应用的数据库文件拷贝出来，有的同学喜欢用root过的文件管理器来拷贝database下的文件到sdcard再弄到电脑进行操作，这真的好麻烦。adb 可以轻松解决这个问题。
{% highlight shell %}
adb pull /data/data/xxx.xxx.xxx/databases/xx.db ~/sqlitedb/
{% endhighlight %}
> 这么简单的一句就搞定啦。但是有的手机会提示Permisssion denied。这是因为adb没有root权限。 
{% highlight shell %}
adb root
adb remount / 
adb pull /data/data/xxx.xxx.xxx/databases/xx.db ~/sqlitedb/
{% endhighlight %}
> 以root的形式挂在根目录，文件顺利拷贝出来。
> 如果手机的sqlite3命令可以用的话，也可以在 adb shell 里直接操作sqlite数据库.

