---
layout: post
title: "Java protobuf missing classes"
description: ""
category: 
tags: []
---
{% include JB/setup %}
Run protoc on descriptor.proto, like this (from the root directory): 

$ src/protoc --java_out=java/src/main/java src/google/protobuf/ 
descriptor.proto 
[from Jon](https://groups.google.com/forum/?fromgroups=#!topic/protobuf/FRqMcmEDMTs)
