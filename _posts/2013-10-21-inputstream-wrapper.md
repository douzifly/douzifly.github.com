---
layout: post
title: "InputStream wrapper"
description: "Java 包装器模式应用"
category: 
tags: [Android, Java]
---
{% include JB/setup %}

今天在开发Android局域网文件传输的时候，遇到这么一个需求，当客户端向HTTP SVR请求文件时，HTTP SVR应答的数据流来自多个文件。比如，客户端发起a.rmvb文件下载请求,而在服务端上，a.rmvb并不真实存在，而是存在a1.rmvb, a2.rmvb, a3.rmvb 等。

服务器端采用的时开源的简单HTTP Server NanoHttpd, 但是NanoHttpd暴露的接口是一个InputStream对象。 

方案一：
服务器端将文件合并后发送。这样就可以new FileInputStream(mergedFilePath)给NanoHttpd了。
这样做虽然简单，但是缺点非常明显，服务器并不需要一个完整的文件，这么做效率底下，还占用而外的磁盘空间，数据会进行多次拷贝。

方案二：
修改NanoHttpd让其支持接收InputStream[ ]数组这样的数据结构.
缺点：对现有代码修改，在项目周期紧的情况下，风险较高。

方案三：
写一个InputStream的包装，接收多个InputStream。
直接上代码：
{% highlight java %}
/**
 * Author:Xiaoyuan
 * Date: Oct 21, 2013
 * 深圳快播科技
 */
package com.qvod.tuitui.network;

import java.io.IOException;
import java.io.InputStream;

import android.util.Log;

/**
 * 
 * 
 * @author Xiaoyuan
 *
 */
public class MultiInputStream extends InputStream{
	
	final static String TAG = "MulitInputStream";

	private InputStream[] mStreams;
	private int mReadStreamIndex;
	private int mAvai = 0;
	
	public MultiInputStream(InputStream[] streams) throws IOException {
		if(streams == null || streams.length == 0) {
			throw new IllegalArgumentException();
		} 
		mStreams = streams;
		calcAvai();
	}
	
	public void calcAvai() throws IOException {
		for(int i = 0; i < mStreams.length; i++) {
			mAvai += mStreams[i].available();
		}
		Log.d(TAG, "calc:" + mAvai);
	}
	
	@Override
	public int available() throws IOException {
		return mAvai;
	}
	
	private void close(int index) {
		Log.d(TAG, "close index:" + index);
		try {
			mStreams[index].close();
		} catch(Exception e) {
			Log.e(TAG, "close exp:" + e.getMessage());
			e.printStackTrace();
		}
	}
	
	@Override
	public int read() throws IOException {
		int value = -1;
		int streamIndex = mReadStreamIndex;
		InputStream s = mStreams[streamIndex];
		value = s.read();
		if(value == -1) {
			Log.d(TAG, "read -1, check next");
			// check have next stream
			if(mReadStreamIndex < mStreams.length) {
				close(mReadStreamIndex);
				mReadStreamIndex++;
				Log.d(TAG, "read next :" + mReadStreamIndex);
				// read next byte from next stream
				return read();
			}
		}
//		Log.d(TAG, "read return:" + value + " stream index:" + mReadStreamIndex);
		return value;
	}
	
	@Override
	public int read(byte[] buffer, int offset, int length) throws IOException {
		int readCount = -1;
		int streamIndex = mReadStreamIndex;
		InputStream s = mStreams[streamIndex];
		readCount = s.read(buffer, offset, length);
		if(readCount == -1) {
			Log.d(TAG, "read -1, check next");
			// check have next stream
			if(mReadStreamIndex < mStreams.length) {
				close(mReadStreamIndex);
				mReadStreamIndex++;
				s = mStreams[mReadStreamIndex];
				Log.d(TAG, "read next :" + mReadStreamIndex);
				// read next byte from next stream
				return s.read(buffer, offset, length);
			}
		}
//		Log.d(TAG, "read return:" + value + " stream index:" + mReadStreamIndex);
		return readCount;
	}
	
	@Override
	public synchronized void reset() throws IOException {
		Log.d(TAG, "reset");
		mReadStreamIndex = 0;
		for(int i = 0; i < mStreams.length; i++) {
			try {
				mStreams[i].reset();
			} catch(Exception e) {
				e.printStackTrace();
			}
		}
	}
	
	@Override
	public void close() throws IOException {
		Log.d(TAG, "close");
		for(int i = 0; i < mStreams.length; i++) {
			try {
				mStreams[i].close();
			} catch(Exception e) {
				e.printStackTrace();
			}
		}
	}

}
{% endhighlight %}

需要注意的是，InputStream的read是一个抽象函数，你必须重写它，但是read(buf, offset, len) 却有一个默认的实现，如果不重写的话，效率低的可怜。因为内部实现是循环调用read来进行buf填充的。


