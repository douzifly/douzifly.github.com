---
layout: post
title: "Use fragment with TabActivity"
description: ""
category: Android
tags: [Android]
---
{% include JB/setup %}

有时，我们需要不修改现有代码的情况下，让TabActivity支持Fragment。但是，TabActivity并不继承自FragmentActivity。
所以我们需要拷贝TabActivity以及ActivityGroup的代码到项目中，并让ActivityGroup继承自FragmentActivity。
此时会出现一个问题，无法重写dispatchActivityResult这个函数了，因为该函数的访问性为包内可见。没有关系，我们可以去掉@Override，运行时，依然可以调用到这个dispatchActivityResult里面来。但是也要做简单的修改，如下。


{% highlight java %}
	void dispatchActivityResult(String who, int requestCode, int resultCode,
            Intent data) {
        if (who != null) {
            Activity act = mLocalActivityManager.getActivity(who);
            if (act != null) {
//                act.onActivityResult(requestCode, resultCode, data);
                callActivityResult(act, requestCode, resultCode, data, false);
                return;
            }
        }
//        super.dispatchActivityResult(who, requestCode, resultCode, data);
        callActivityResult(this, requestCode, resultCode, data, true);
    }
    
    void callActivityResult(Activity act, int requestCode, int resultCode, Intent data, boolean superClass) {
        Class c ;
        if (superClass) {
            c = act.getClass().getSuperclass();
        } else {
            c = act.getClass();
        }
        try {
            Method mOAR = c.getDeclaredMethod("onActivityResult", int.class, int.class, Intent.class); 
            mOAR.setAccessible(true);
            mOAR.invoke(act, requestCode, resultCode, data);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (IllegalArgumentException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        } 
    }
{% endhighlight %}
