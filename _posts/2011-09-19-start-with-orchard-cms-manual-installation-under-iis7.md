---
layout: post
title:  Start with Orchard CMS - Manual Installation under IIS7
date:   2011-09-19 14:00:00
tags:   Orchard, CMS, ASP.NET, IIS
assets: 2011-09-19-start-with-orchard-cms-manual-installation-under-iis7
header: 2
comments: true
---

Seit einigen Wochen beschäftige ich mich mit dem Opensource CMS [Orchard](http://www.orchardproject.net/) von Microsoft. In diesem Post möchte ich die Installation kurz beschreiben, welche die Installation/Konfiguration des IIS Webservers sowie des CMS selbst umfasst.

#Infrastruktur

Server, Microsoft Windows Server 2008R2 WebEdition x64, ist bereits aufgesetzt.

Download & Run Microsoft [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx)

Unter *Products – Server “IIS7 Recommended Configuration”*, und unter *Products – Frameworks “ASP.NET MVC3”* auswählen und installieren. Diese Konfiguration sollte vorerst ausreichen, bei Bedarf können selbstverständlich noch weitere Module, oder der SQL Server Express installiert werden. Abhängigkeiten werden automatisch ermittelt und installiert.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-1.png)

Nach Abschluss der Installation über *Run – inetmgr* den Internet Information Services (IIS) Manager starten und eine neue Website erstellen.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-2.png)

Wichtig, der Application Pool muss auf .NET Framework v4.xx umgestellt werden.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-3.png)

Hinweis: Für den Test mit der Domain mydomain.com habe ich einen Windows Host File (<kbd>C:\Windows\System32\drivers\etc\hosts</kbd>) Eintrag erstellt.

{% highlight bash %}
127.0.0.1   www.mydomain.com
127.0.0.1   mydomain.com
{% endhighlight %}

#Installation Orchard CMS

Es gibt verschiedene Varianten Orchard zu installieren. Per WebMatrix, Visual Studio und WebDeploy oder wie hier beschrieben händisch mit der Konfiguration von IIS und dem ZIP-File. Das [Package](http://orchard.codeplex.com/) kann von CodePlex heruntergeladen werden. ZIP-File entpacken und den Inhalt des Verzeichnises Orchard nach <kbd>C:\inetpub\wwwroot\mydomain.com</kbd> kopieren.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-4.png)

Das Verzeichnis *App_Data* sollte noch auf die richtigen Berechtigungen überprüft werden, *IIS_IUSRS* benötigt read/write Rechte. Auch die Verzeichnisse *Modules* und *Themes* benötigen spezielle Berechtigungen, sollten diese Fehlen wird jedoch beim Installationsprozess darauf hingewiesen.

Nun kann im Browser bereits die entsprechende URL http://www.mydomain.com aufgerufen werden und der Setup Prozess wird gestartet.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-5.png)

Die auszufüllenden Felder sollten selbsterklärend sein. Ich habe als Data Storage den built-in data storage (SQL Server Compact) verwendet. Bei der Auswahl von SQL Express werden entsprechende Hinweise bezüglich ConnectionString angezeigt (falls SQL Server Express mit der Standardkonfiguration verwendet wird, kann als sqlServerName (local)\SQLEXPRESS verwendet werden). Weiter mit Finish Setup - nun wird gekocht!

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-6.png)

Fertig!

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-7.png)

Das Backend kann über den Link im Footer (Dashboard), oder mit /admin (http://www.mydomain.com/admin) aufgerufen werden.

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-8.png)

![]({{ site.url }}/assets/{{page.assets}}/start-with-orchardcms-manual-installation-under-iis7-9.png)

#Fazit

Das System gefällt mir sehr gut, auch nach weiterführenden Tests. Der Installationsprozess selbst ist sehr einfach und übersichtlich gestaltet und ich bin bislang auf keine Probleme gestossen. Bin gespannt auf die weitere Entwicklung.