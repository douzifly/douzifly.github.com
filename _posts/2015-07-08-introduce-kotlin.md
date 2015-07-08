---
layout: post
title: "Introduce Kotlin"
description: ""
category: 
tags: [Android, Kotlin]
---
{% include JB/setup %}

[Kotlin](http://kotlinlang.org/)简介 ， 其语法来自： Swift， C#， Go？

优点：

非常简洁的语法，代码量锐减，老板要扣薪水了！

与Java代码交互非常容易和简单

更好的泛型

安全的null运算， 跟Swift的一样

更好的Lambda支持

方法（函数）为一级公民

扩展方法

inline

还有一系列的语法糖: range, data class 等

以下是我在项目中的一些代码：

1，对String进行扩展方法后，网络请求变成了这样

{% highlight kotlin %}
// 异步搜索App
fun searchApps(callback: (AppInfo?) -> Unit) {
    var url = "http://www.xxxx.com/app/"
    url.addHeader("user-agent", "kotlin")
            .addQueryString("q", "GooglePlay")
            .httpGet {
                resp ->
                var app = resp.content?.toJson(javaClass<AppInfo>())
                callback(app)
            }
}
{% endhighlight %}

异步请求应用信息，并转换为AppInfo对象

网络请求封装了[AsyncHttpClient/async-http-client · GitHub](https://github.com/AsyncHttpClient/async-http-client)， Json转换封装了 [alibaba/fastjson · GitHub](https://github.com/alibaba/fastjson) （为了简约起见，忽略了其他逻辑，下同）

2， 为 Activity 和 Fragment 增加了异步操作扩展

{% highlight kotlin %}
class MainActivity : Activity() {
    fun asyncSample() {
        async({
            // 这里的代码执行在 *线程池* 中
            // 返回值自动传入下一个参数
            verySlowMethod()
         }) {

            // 读取上一个参数的值
            // 并且在 *主线程* toast出来
            str->
            str.toast(this)
        }
    }
}

fun verySlowMethod(): String {
    Thread.sleep(2000)
    return "kotlin"
}

// async 函数定义
fun async<T>(background: ()-> T, foreground:(T) -> Unit) {
    checkPool()
    pool?.submit() {
        val t:T = background()
        runOnUIThread {
            foreground(t)
        }
    }
}

// 为Activity增加扩展
inline fun Context.async<T>(noinline b:() -> T, @noinline f:(T) -> Unit) = ThreadExt.async(b, f)

{% endhighlight %}

也可以这样

{% highlight kotlin %}
fun requestApps() {
    showProgressBar()
    AppApi.getHotApps {
        hotApps ->
        ui {
            // 该块内的代码将执行在主线程
            hideProgressBar()

            if (hotApps.size() == 0) {
                // no data, retry
                showRetryButton {
                    v ->
                    requestApps()
                }
            } else {
                adapter?.apps = hotApps
            }
        }
    }
}
{% endhighlight %}

代码解释，功能是请求应用列表，请求时显示进度条，失败显示重试按钮，点击重试按钮，重新请求数据，如此反复，请求到数据到，将数据交给RecyclerView adapter， 并填充。

其中 getHotApps为异步函数，其参数为网络请求完毕后的callback。

ui为Fragment或者Activity的扩展方法，其块内的代码执行在主线程。

（Kotlin的语法糖，如果最后一个参数为Lambda表达式，那么可以不写在参数列表中， 如果Lambda表达式没有参数，可以不写 -> ）

3，对属性的支持，类似C#，麻麻再也不担心我写get set函数啦。

{% highlight kotlin %}
inner class AppAdapter : RecyclerView.Adapter<AppViewHolder>() {

      var apps: ArrayList<AppInfo>? = null
          set (value) {
              $apps = value
              notifyDataSetChanged()
          }
	
      // 省略其他代码
}
{% endhighlight %}

这里使用了set块，表示，如果该属性在被赋值时，会调用Adapter的notifyXX 方法来通知视图更新。

先介绍到这里，开发Android，写Java到很痛苦的小伙伴们，一起试试吧！

另外，Kotlin现在已经相对稳定，AndroidStudio ( IntellJ Idea) 对Kotlin的支持相当好，毕竟是自家的东西。

有了val， 在也不用纠结 static final 和 final static 了。 :-)