---
layout: post
title:  IIS ASP.NET Handle Error
date:   2011-09-16 14:00:00
tags:   IIS, ASP.NET, Webserver
assets: 
header: 3
comments: true
---

Hatte heute das Problem, dass nach der Installation von IIS mit der recommended configuration über den [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx) ASP.NET nicht richtig geladen wurden. Dies resultierte in einem Error (Handler Error) beim Seitenaufruf.

Um die Registrierung von Hand vorzunehmen ins Verzeichnis <kbd>C:\Windows\Microsoft.NET\Framework64\v4.0.30319</kbd> wechseln und den folgenden Befehl ausführen.

{% highlight bash %}
aspnet_regiis.exe -i
{% endhighlight %}

Nach einem Reload der Website oder allenfalls Restart des IIS sollte der Fehler nicht mehr auftreten.