---
layout: post
title: "mysql select random rows"
description: ""
category: db
tags: [mysql]
---
{% include JB/setup %}

在Go语言中，function被视为一种类型，描述了一组相同签名和返回值的函数。

既然是一种类型，那么我们就可以把function类型当作普通的类型来操作，如下面的代码片段。

{% highlight sql %}
// get max id
select max(id) into @maxid from tb;
// generate rand ids 
select * from tb as a
	join (select floor(@maxid * rand()) as id from tb limit 10) as b
	on (a.id = b.id)
{% endhighlight %}
