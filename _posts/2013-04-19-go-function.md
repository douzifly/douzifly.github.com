---
layout: post
title: "Go 语言的function"
description: ""
category: Go
tags: [Language]
---
{% include JB/setup %}

在Go语言中，function被视为一种类型，描述了一组相同签名和返回值的函数。

既然是一种类型，那么我们就可以把function类型当作普通的类型来操作，如下面的代码片段。

{% highlight go %}
	package main

	import (
	    "fmt"
	)

	type F func()
	type FS []F

	func main() {
	    c := FS{func() { fmt.Print("hello ") }, func() { fmt.Println("douzifly") }}
	    for i := 0; i < len(c); i++ {
		c[i]()
	    }
	}
{% endhighlight %}
