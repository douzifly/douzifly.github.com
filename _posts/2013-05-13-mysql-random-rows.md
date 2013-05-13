---
layout: post
title: "mysql random rows"
description: ""
category: db
tags: [mysql]
---
{% include JB/setup %}

{% highlight sql %}
// get max id
select max(id) into @maxid from tb;
// generate rand ids 
select * from tb as a
	join (select floor(@maxid * rand()) as id from tb limit 10) as b
	on (a.id = b.id)
{% endhighlight %}
