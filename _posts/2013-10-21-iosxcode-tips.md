---
layout: post
title: "iOS&XCode tips"
description: ""
category:
tags: [iOS]
---
{% include JB/setup %}

* Jump to method shortcut

In Xcode 4, the "Symbols" drop-down appears when pressing Control-6. You can change this in the Xcode settings by changing the key binding for "Standart Editor > Show Document Items".

* iOS7 status bar back to iOS6

{% highlight objc %}
- (void) statusBarHack
{
    if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 7.0) {
        UIView *addStatusBar = [[UIView alloc] init];
        addStatusBar.frame = CGRectMake(0, 0, self.window.frame.size.width, 20);
        addStatusBar.backgroundColor = [UIColor blackColor]; //change this to match your navigation bar
        [self.window.rootViewController.view addSubview:addStatusBar];
    }
}
{% endhighlight %}
