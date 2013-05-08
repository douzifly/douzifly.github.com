---
layout: post
title: "python rsa sign"
description: ""
category: 
tags: []
---
{% include JB/setup %}
{% highlight python %}
import M2Crypto, base64

def verify(data, sign):
    key = M2Crypto.RSA.load_pub_key_bio(M2Crypto.BIO.MemoryBuffer(settings.ALIPAY.RSA_ALIPAY_PUBLIC))
    m=M2Crypto.EVP.MessageDigest('sha1')
    m.update(data)
    digest=m.final()
    signature = key.verify(digest, base64.decodestring(sign), "sha1")
    return signature

def sign(data):
    key = M2Crypto.RSA.load_key("/home/douzifly/pri_key_ltx20130424.pem")
    m=M2Crypto.EVP.MessageDigest('sha1')
    m.update(data)
    digest=m.final()
    signature = key.sign(digest, "sha1")
    return signature

if __name__ == "__main__":
	signed = sign("a")
	print(base64.b64encode(signed))
{% endhighlight %}
