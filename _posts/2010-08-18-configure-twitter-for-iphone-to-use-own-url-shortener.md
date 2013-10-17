---
layout: post
title:  Configure Twitter for iPhone to use own URL Shortener
date:   2010-08-18 14:00:00
tags:   YOURLS, iPhone, Twitter, URL-Shortener
assets: 2010-08-18-configure-twitter-for-iphone-to-use-own-url-shortener
header: 3
comments: true
---

Wie in einem vorherigen Post erwähnt kann in Twitter for iPhone ein eigener URL Shortener konfiguriert werden. Folgende Schritte sind dafür notwendig, vorausgesetzt YOURLS ist bereits installiert und lauffähig.

**Well, let’s start.**

Twitter for iPhone starten und *Accounts & Settings – Settings* auswählen.

![]({{ site.url }}/assets/{{page.assets}}/configure-iphone-to-use-own-url-shortener-1.png)

![]({{ site.url }}/assets/{{page.assets}}/configure-iphone-to-use-own-url-shortener-2.png)

Im Settings Screen *Services* auswählen.

![]({{ site.url }}/assets/{{page.assets}}/configure-iphone-to-use-own-url-shortener-3.png)

In den Services unter *URL Shortening* den Service auf *Custom* umstellen.

![]({{ site.url }}/assets/{{page.assets}}/configure-iphone-to-use-own-url-shortener-4.png)

Nun wird man aufgefordert die URL zum URL Shortener Dienst einzutragen. Angenommen YOURLS wurde wie im Post *Eigener URL Shortener mit YOURLS* installiert und ist über http://url.mydomain.comerreichbar, muss folgende Service-URL eingetragen werden.

{% highlight bash %}
http://url.mydomain.com/yourls-api.php?username=myUsername&password=myPassword&format=simple&action=shorturl&url=%@
{% endhighlight %}

The rest is magic… Neuer Tweet mit Full-URL – Shrink URLs – Voilà.

![]({{ site.url }}/assets/{{page.assets}}/configure-iphone-to-use-own-url-shortener-5.png)

Have Fun with shrink’n’tweet!