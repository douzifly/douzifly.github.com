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

* handle UIWebView pan gesture

{% highlight objc %}
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer;{
  return YES;
}
{% endhighlight %}

* Change UITextFiled appreance

Import &lt;QuartzCore/QuartzCore.h&gt;

and for changing the border color use the following code snippet (I'm setting it to redColor),
textField.layer.cornerRadius=8.0f;
textField.layer.masksToBounds=YES;
textField.layer.borderColor=[[UIColor redColor]CGColor];
textField.layer.borderWidth= 1.0f;

* Fix UITextFiled rightViewMode bug

{% highlight objc %}
- (BOOL)becomeFirstResponder
{
    BOOL ret = YES ;

    ret = [super becomeFirstResponder] ;

    if( ret && ( _setupClearButtonMode == UITextFieldViewModeWhileEditing ) )
        self.rightViewMode = UITextFieldViewModeAlways ;

    return ret ;
}

- (BOOL)resignFirstResponder
{
    BOOL ret = YES ;

    ret = [super resignFirstResponder] ;

    if( ret && ( _setupClearButtonMode == UITextFieldViewModeWhileEditing ) )
        self.rightViewMode = UITextFieldViewModeWhileEditing ;

    return ret ;
}
{% endhighlight %}

