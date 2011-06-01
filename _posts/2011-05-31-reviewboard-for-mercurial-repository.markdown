---
layout: post 
title: 在Review Board中使用mercurial(hg)库
---

在 [Review Board][] 里使用mercurial时候，填了repository的url，
但是在上传diff文件的时候 始终抱找不到文件的错误 
The file was not found in the repository. 

看了下源码，发现连接的时候返回是个404错误，
进一步看了下scmtools/hg.py里的HgWebClient，找到问题了，
原来这里请求url路径里没有包含hgweb.cgi
而我这边的mercurial repo server里没有把hgweb.cgi在路径里隐藏，
在apache里改了下就好了。

至于怎么改，可以参考[Publishing the CGI script directly](http://mercurial.selenic.com/wiki/PublishingRepositories)


[Review Board]: http://code.google.com/p/reviewboard/

