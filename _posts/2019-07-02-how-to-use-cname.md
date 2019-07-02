---
layout: post
title:  "关于CNAME!"
date:   2019-07-02 09:55:00 +0800
categories: network
tag: network
---

* content
{:toc}

# 关于CNAME解析

今早看到订阅更新了，有一个网友这样问到：
能不能在地址栏用 B.com 完全替代 A.com 呢？这个有一个前置条件就是不能更改各自的解析IP

1. 首先想到的是NGINX 做代理，再B 域名的服务器中搭建ng服务，然后做转发。这个是一定可以完成。
2. 最近一直再玩家里的路由器，一直有用到域名解析。之前一直用的是A解析，那如果用CNAME解析呢？ 是不是可以完成这个需求呢？
---
开始测试
1. CNAME 到 www.baidu.com 失败
2. CNAME 该域名下面的另外一个域名  成功
3. CNAME 到其他各种网站 失败
   
---
做到这个时候，我ping了一下域名，发现域名的解析地址统统的都跑cname解析目标域名的ip上去了。也就是说我离题了。当然测试之后才识真知。

---
特地检索了一下A解析和CNAME解析的区别及CNAME存在的意义
>Differences Between A and CNAME Records
The A and CNAME records are the two common ways to map a host name (“name”) to one or more IP addresses. There are important differences between these two records.
>The A record points a name to a specific IP. If you want blog.dnsimple.com to point to the server 185.31.17.133 you’ll configure:
`blog.dnsimple.com.     A        185.31.17.133`
>The CNAME record points a name to another name instead of to an IP. The CNAME source represents an alias for the target name and inherits its entire resolution chain.
>Let’s use our blog as an example:
```
blog.dnsimple.com.      CNAME   aetrion.github.io.
aetrion.github.io.      CNAME   github.map.fastly.net.
github.map.fastly.net.  A       185.31.17.133
```
>We use GitHub Pages and we set blog.dnsimple.com as a CNAME of aetrion.github.io, which is a CNAME of github.map.fastly.net, which is an A record pointing to 185.31.17.133. This means blog.dnsimple.com resolves to 185.31.17.133.
>An A record points a name to an IP. A CNAME record can point a name to another CNAME or to an A record.

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
