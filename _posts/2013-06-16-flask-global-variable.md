---
layout: post
title: "在Flash框架中使用全局变量"
description: ""
category: Python 
tags: [Python]
---
{% include JB/setup %}

Flask中的g属于request context的全局变量，current_app 为应用程序级别的全局变量，可以用过 current_app.config\[key\] 来进行保存。
