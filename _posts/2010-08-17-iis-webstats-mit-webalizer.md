---
layout: post
title:  IIS WebStats mit Webalizer
date:   2010-08-17 14:00:00
tags:   IIS, WebStats, Webalizer, Hosting
assets: 
header: 3
comments: true
---

Auf Anfrage erstellt Microsoft’s IIS wunderbare LogFiles für WebStats, sofern man das entsprechende Werkzeug zur Auswertung hat… Mit genügend Budget ist dieses Werkzeug auch rasch beschafft und installiert. Was aber, wenn das Budget halt eben nicht vorhanden ist?

Hierfür ist der Webalizer resp. dessen Weiterentwicklung von Stone Steps Inc. eine Alternative. Mit ein wenig Konfigurations-/Programmieraufwand ist das Tool in Kürze einsatzbereit.

Um das Tool mit dem IIS einzurichten sind ein paar Einstellungen vorzunehmen. Zu Beginn sollte das Logging aktiviert werden. Im IIS 5/6 unter Website – Eigenschaften.

Nachdem das Tool heruntergeladen und das .zip File entpackt wurde, steht mit <kbd>samples.conf</kbd> eine Default Konfigurationsdatei zur Verfügung, welche nach <kbd>webalizer.conf</kbd> umbenannt werden muss. In diesem File können alle gewünschten Einstellungen vorgenommen werden. Um den Report mittels CSS formatieren zu können ist zusätzlich folgende Zeile zu ergänzen.

{% highlight bash %}
# HTML Css Path  Pfad zum CSS File
HTMLCssPath C:\Tools\Webalizer\
{% endhighlight %}

Der HTMLCssPath gibt den relativen/absoluten Pfad zum CSS File an, dessen Formatierungen für die Reports verwendet werden.

Um die Auswertungen der Log Files zu automatisieren ist es sinnvoll ein Batch-Script zb. alle 12 Stunden durchlaufen zu lassen. Dieses Batch-Script sieht folgendermassen aus (eine Zeile).

{% highlight bash %}
for %%i
IN (C:\Inetpub\IISLogs\W3xxx\*.log)
do webalizer.exe -F iis -n <a href="http://www.domain.ch/">www.domain.ch</a>
-o C:\Inetpub\wwwroot\domain.ch\webstats
%%i
{% endhighlight %}

Das Script durchläuft das Verzeichnis <kbd>C:\Inetpub\IISLogs\W3xxx\ </kbd> resp. alle darin enthaltenen .log Files, und speichert den Report ins Output-Verzeichnis <kbd>C:\Inetpub\wwwroot\domain.ch\webstats</kbd>. Der mitübergebene Domainname wird für die Verlinkung im Report verwendet, sollte deshalb zwingend mitgeliefert werden. Das Batch-Script sollte im Webalizer Verzeichnis abgelegt werden, ansonsten muss der Pfad zu <kbd>webalizer.exe</kbd> angepasst werden.