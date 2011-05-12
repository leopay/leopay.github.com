---
layout: post 
title: RC4 Implementation in Python
---

####RC4算法介绍

[RC4][] 加密算法Ron Rivest(非常有名的非对称加密算法RSA三巨头之一)在1987年设计的，密钥长度可变的流加密算法簇。之所以称其为簇，是由于其核心部分的S-box长度可为任意，但一般为256字节。该算法的速度可以达到DES加密的10倍左右，且具有很高级别的非线性。RC4起初是用于保护商业机密的。但是在1994年9月，它的算法被发布在互联网上，也就不再有什么商业机密了。RC4也被叫做ARC4（Alleged [RC4][] ——所谓的[RC4][])），因为RSA从来就没有正式发布过这个算法。

备注：[RC4][] 是对称密钥加密算法，而RSA是非对称的加密算法。由于[RC4][]算法加密是采用的xor，所以，一旦子密钥序列出现了重复，密文就有可能被破解。那么，[RC4][] 算法生成的子密钥序列是否会出现重复呢？由于存在部分弱密钥，使得子密钥序列在不到100万字节内就发生了完全的重复，如果是部分重复，则可能在不到10万字节内就能发生重复，因此，推荐在使用[RC4[]算法时，必须对加密密钥进行测试，判断其是否为弱密钥。根据目前的分析结果，没有任何的分析对于密钥长度达到128位的[RC4][]有效，所以，**[RC4][]是目前最安全的加密算法之一**。

####Python实现
{% highlight python %}
    def RC4(data, key):
        x = 0 
        s = range(256)
        for i in range(256):
            x = (x + s[i] + ord(key[i % len(key)])) % 256 
            s[i], s[x] = s[x], s[i]
        x = y = 0 
        out = []
        for c in data:
            x = (x + 1) % 256 
            y = (y + s[x]) % 256 
            s[x], s[y] = s[y], s[x]
            out.append(chr(ord(c) ^ s[(s[x] + s[y]) % 256]))
        return ''.join(out)

    >>> RC4('test','1234567890')
    >>> '\xbeI\x86u'
    >>> RC4('\xbeI\x86u', '1234567890')
    >>> 'test'
{% endhighlight %}

Ref:<br />
[RC加密解密算法C源代码](http://wzgyantai.blogbus.com/logs/31867065.html)<br />
[Encryption between Python and C#](http://entitycrisis.blogspot.com/2011/04/encryption-between-python-and-c.html)

[RC4]: http://en.wikipedia.org/wiki/RC4

