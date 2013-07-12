---
layout: post
title: "fix htc cant recv udp broadcast"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Wanting to receive broadcasts in a async routine I now use the following code just before the loop where the broadcast messages are received:

WifiManager wifi;
wifi = (WifiManager) getSystemService(Context.WIFI_SERVICE);
MulticastLock ml = wifi.createMulticastLock("just some tag text");
ml.acquire();

When the asynctask stops I do a 
ml.release();

This works perfect!
from:https://code.google.com/p/android/issues/detail?id=8407
