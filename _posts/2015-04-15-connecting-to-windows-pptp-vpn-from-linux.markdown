--- 
name: connecting-to-windows-pptp-vpn-from-linux
layout: post
title: Connecting to a Windows PPTP VPN from Linux (a problem solved)
time: 2015-04-15 10:45:00 +00:00
categories:
    - Linux
    - VPN
---

I'd been struggling to connect to one of our client's Windows VPNs from my Linux Mint (Cinnamon) OS for a little while but not had any urgent need for it so put off solving it. Today, the urgency arose and so I went about fixing the issue which was thankfully pretty painless.<!--more-->

## The problem

The VPN was configured using the built in network manager UI in my version of Linux Mint:

> Network connections -> Add -> Type: PPTP

I entered the gateway address and credentials as supplied and left all other options as default. Attempting to connect to the VPN would fail, without any message explaining the failure.

## Getting an error message

The first job was to find out what was going wrong. After a little [Duck-Duck-Go-ing](http://www.duckduckgo.com), I discovered that the vital logs were being written to `/var/log/syslog`. I tailed this while trying to connect and discovered this message that was causing an authentication failure:

{% highlight sh %}
pppd[3321]: EAP: unknown authentication type 26; Naking
{% endhighlight %}

## Fixing the problem

A little searching later gave me the solution. Some Windows VPNs will not accept a request that will attempt multiple Authentication schemes and will use MPPE for the connection. I fixed my problem by enabling MPPE and disabling all authentication methods with the exception of MSCHAPv2. The settings dialog for me could be found at:

> Network connections -> Edit (my connection) -> Advanced...


Hopefully that helps someone else out (or indeed me, when I forget).

D




