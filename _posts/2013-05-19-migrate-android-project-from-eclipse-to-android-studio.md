---
layout: post
title: "Migrate Android project from Eclipse to Android Studio"
description: ""
category:Android 
tags: [Android]
---
{% include JB/setup %}

There are some points to be record:

	1, upgrade adt tools to version 22.
	2, right click eclipse project choose export , then Genrate Gradle build files;
	3, Open Android Studio, Choose import project, find your old project root directory and click next.
	4, If your project denpends on some other library project like ActionBarSherlock, choose import Module locate at your library root directory (for ActionBarSherlock you should choose sub direcotry called library), then select Create module from existing source.
	5, Add module for your porject.
	6, change module Scope to Provided for such library used with more than one modules, Such as android-support-v4.jar.

 
