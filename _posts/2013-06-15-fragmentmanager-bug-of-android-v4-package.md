---
layout: post
title: "FragmentActivity bug of android v4 package"
description: ""
category: Android
tags: [Android]
---
{% include JB/setup %}

今天跟一个bug花了差不多1个小时左右的时间.
现象是这样的:
同事在主程序中新加了一个模块，只要一运行这个新加的模块，再运行我的模块就会崩溃。如果不运行同时的模块，运行我的模块就不会崩溃。
堆栈信息：Fragment xxx did not create a view。我的模块会通过LayoutInflator去动态加载一个带<fragment>标签的界面，但是我的xxxFragment在onCreateView中返回了View啊。通过加Log日志发现，框架根本没有回调到xxxFragment的onCreateView函数里来，这就奇怪了。

调用以下代码会抛出 Fragment xxx did not create a view的异常:
{% highlight java %}
View content = mContext.getLayoutInflater().inflate(
                    R.layout.transfer_search_popup, null);
{% endhighlight %}

R.layout.transfer_search_popup 的源代码如下：
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <fragment class="com.qvod.player.activity.transfer.TransferSearchFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:tag="TransferSearchFragment"/>
</LinearLayout>
{% endhighlight %}


遂开始研究源代码：
LayoutInflator 在 inflate 的时候，会调用到 FragmentActivity的onCreateView里面来。

{% highlight java %}
    	Fragment fragment = id != View.NO_ID ? mFragments.findFragmentById(id) : null;
        if (fragment == null && tag != null) {
            fragment = mFragments.findFragmentByTag(tag);
        }
        if (fragment == null && containerId != View.NO_ID) {
            fragment = mFragments.findFragmentById(containerId);
        }

        if (FragmentManagerImpl.DEBUG) Log.v(TAG, "onCreateView: id=0x"
                + Integer.toHexString(id) + " fname=" + fname
                + " existing=" + fragment);
        if (fragment == null) {
            fragment = Fragment.instantiate(this, fname);
            fragment.mFromLayout = true;
            fragment.mFragmentId = id != 0 ? id : containerId;
            fragment.mContainerId = containerId;
            fragment.mTag = tag;
            fragment.mInLayout = true;
            fragment.mFragmentManager = mFragments;
            fragment.onInflate(this, attrs, fragment.mSavedFragmentState);
            mFragments.addFragment(fragment, true);

        } else if (fragment.mInLayout) {
            // A fragment already exists and it is not one we restored from
            // previous state.
            throw new IllegalArgumentException(attrs.getPositionDescription()
                    + ": Duplicate id 0x" + Integer.toHexString(id)
                    + ", tag " + tag + ", or parent id 0x" + Integer.toHexString(containerId)
                    + " with another fragment for " + fname);
        } else {
            // This fragment was retained from a previous instance; get it
            // going now.
            fragment.mInLayout = true;
            // If this fragment is newly instantiated (either right now, or
            // from last saved state), then give it the attributes to
            // initialize itself.
            if (!fragment.mRetaining) {
                fragment.onInflate(this, attrs, fragment.mSavedFragmentState);
            }
            mFragments.moveToState(fragment);
        }

        if (fragment.mView == null) {
            throw new IllegalStateException("Fragment " + fname
                    + " did not create a view.");
        }

{% endhighlight %}

以上是onCreateView的关键代码。可以看到当判断到 mView == null 时便会抛出这个异常。
这时候，我们根据现象来观察代码，由于框架没有回调到我的
