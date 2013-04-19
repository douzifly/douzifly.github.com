---
layout: post
title: "Go 闭包"
description: ""
category: Go
tags: [Language]
---
{% include JB/setup %}

1，如果闭包使用的变量为全局变量，那么每个闭包都共用这一个变量。

{% highlight go %}
package main

import "fmt"

var sum = 0
func adder() func(int) int {
	return func(x int) int {
				sum += x
				return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(pos(i),neg(-2*i))
	}
}
{% endhighlight %}


2，如果闭包使用的变量为局部变量，那么每个闭包都会有一个该变量的副本。

{% highlight go %}
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
				sum += x
				return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(pos(i), neg(-2*i))
	}
}
{% endhighlight %}
 

使用闭包实现fb数列

{% highlight go %}
package main

import "fmt"

// fibonacci 函数会返回一个返回 int 的函数。
func fibonacci() func() int64 {
	var index = 1
	var a, b int64
	return func()int64{
			var ret int64
			switch{
			case index==1, index ==2:
				a, b = 1, 1
				ret = 1
			default:
				ret = a + b
				a = b
				b = ret
			}
			index ++
			return ret
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}

{% endhighlight %}
