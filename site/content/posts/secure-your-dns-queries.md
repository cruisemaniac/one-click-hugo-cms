---
comments: true
date: "2018-02-22T00:00:00Z"
tags:
- internet
- freedom
- dns
- dnscrypt
title: Secure your DNS Queries
---

As part of my home server setup (which warrants a post very soon), I've setup a DNSCrypt resolver. This was heavily inspired by Abhay ([Nemo][1]) who's setup a DNSCrypt resolver on Digital Ocean in Bangalore.

My server takes the second number for DNSCrypt resolvers in Bangalore, INDIA.

Here's the configuration for the tech guys and hence the `TLDR;`:

```
ProviderKey A6FA:B764:A3D7:A1C7:208D:11DA:0184:8602:81A5:0887:BC42:3DDE:FD33:CF77:0B7A:EFFF
ResolverAddress 106.51.128.78:4434
ProviderName 2.dnscrypt-cert.qag.me
```

The server does not respond to insecure DNS requests on port 53. This means you'll need to setup either Unbound / dnscrypt-proxy to get this going. I'll put up a post on how to set this up shortly.

The server does not log any DNS queries. The only metrics I get out of the server are these:
```
total.num.queries=2357
total.num.queries_ip_ratelimited=0
total.num.cachehits=894
total.num.cachemiss=1463
total.num.prefetch=606
total.num.zero_ttl=2
total.num.recursivereplies=1463
total.requestlist.avg=4.02465
total.requestlist.max=31
total.requestlist.overwritten=0
total.requestlist.exceeded=0
total.requestlist.current.all=0
total.requestlist.current.user=0
total.recursion.time.avg=0.645631
total.recursion.time.median=0.889858
total.tcpusage=0
time.now=1519313244.596154
time.up=4252.237591
time.elapsed=4252.237591
```

What you get out of using a DNSCrypt based resolver is more important - No one knows what websites you're asking for. Google does not need to know what you're up to! They mine ONE LESS user's data. The internet is a decentralised place! Lets try to keep it that way!

Since this is a DNS server, downtime at my end can break your internet. While all attempts will be made to keep this rolling strong, I have setup a ([status page][2]) to let you know if it does go down!

To an internet without the devils!

[1]: https://captnemo.in
[2]: https://status.flatty.co