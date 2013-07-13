---
layout: post
title: "Android Checkbox padding issue"
description: ""
category: Android
tags: [Issue]
---
{% include JB/setup %}

Checkbox 设置完button属性后，文本和button之前在2.3以下没有padding，在4.0设备中确有相当大的padding。当然可以通过设置paddingLeft属性调整这个间距，但是这样4.0没问题，2.3问题有出来了。那怎么样来设置Checkbox的自定义选框且不产生padding的问题呢？那就是不要使用button属性(button="@null")，通过drawableLeft 来设置自定一选框，并且把backgroud设置为@null, 选框和文字的padding可以用过drawablePadding属性来进行设置。这样就不会出现在不同OS下，padding表现完全不一致的情况了。

PS：在Android2.3以下，FrameLayout 同样存在一个bug，如果父容器时FrameLayout的话，那么子控件设置layout_marginTop是没有用的，甚至margin是反方向的，解决方案：要么使用ReleativeLayout来替代FrameLayout要么通过scrollTo来设置这个margin。
