---
layout: post
title: "Android_Listen_PKG_ADD_EVENT"
description: ""
category: Android
tags: [Android]
---
{% include JB/setup %}

if **"package"** isn't be add to scheme, you won't receive the broadcast.

> IntentFilter fi = new IntentFilter(Intent.ACTION_PACKAGE_ADDED);
> fi.addDataScheme("package");
> ctx.registerReceiver(receiver, fi);

