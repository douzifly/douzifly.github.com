---
layout: post
title: "protobuf in ubuntu 12.10 64bit,missing libprotoc.so.8"
description: ""
category: 
tags: []
---
{% include JB/setup %}

After run ./configure make make install in protobuf src directory, protobuf has been installed, but when run protoc command, I got error like this 

'  libprotoc.so.8:cannot open shared object file: No such file or directory '

the solution is: run *ldconfig* in src directory
