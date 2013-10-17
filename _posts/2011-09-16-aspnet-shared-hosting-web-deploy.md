---
layout: post
title:  ASP.NET Shared Hosting - Web Deploy
date:   2011-09-16 14:00:00
tags:   Hosting, ASP.NET, WebDeploy
assets: 
header: 2
comments: true
---

Ich beschäftige mich derzeit mit dem Opensource CMS [Orchard](http://www.orchardproject.net/) von Microsoft und werde in Zukunft sicherlich noch den ein oder anderen Post darüber verfassen. Hier geht’s nun aber ums Deployment auf ein ASP.NET Shared Hosting, mit welchem ich mich ein wenig herumschlagen musste.

Dabei habe ich zwei verschiedene Provider getestet. Zum einen [Gearhost](http://www.gearhost.com/) und zum anderen [discountASP.net](http://www.discountasp.net/).

Die beiden Provider bieten grundsätzlich einen ähnlichen Funktionsumfang. IIS Remote Administration, zusätzliches ControlPanel für die Domainverwaltung, Datenbankverwaltung, etc. Auch preislich sind beide bei $10/Mt. was meiner Meinung nach vollkommen in Ordnung ist. Die Tests mit Gearhost verliefen zunächst vielversprechend, bis es einige Probleme mit den Berechtigungen gab und weitere Deployment-Versuche verhinderte. Leider ist es derzeit noch nicht möglich die Website oder die ApplicationPools selbst neuzustarten, hierfür muss ein Support Ticket eröffnet werden, was etwas ärgerlich sein kann mit >10h Zeitverschiebung :). Gemäss den Technikern sollte dies aber in der nächsten Version des Services möglich sein. Zudem war die Performance nicht allzu berauschend, was jedoch sicherlich auch am Standort des Datacenters (USA) liegt.

Im Gegenzug bietet discountASP.net die Möglichkeit das Hosting in einem UK Datacenter zu eröffnen. Auch die Funktionen wie Neustarten der Website, Recycle des AppPools sind hier bereits implementiert, was mich dazu bewegte vorerst auf diesen Provider zu setzen.

#Entwicklung / Deployment

Die Applikation (Website) habe ich in Visual Studio erstellt und danach per “One Click Publish” von [Web Deploy](http://www.iis.net/download/WebDeploy) auf meinen Entwicklungsserver deployed. Danach habe ich per msdeploy das Ganze zum Provider geschickt.

CMD Prompt öffnen und ins Verzeichnis <em>C:\Program Files\IIS\Microsoft Web Deploy V2</em> wechseln. Nun den folgenden Befehl ausführen, wobei die einzelnen Parameter natürlich auf die Applikation und den Hosting Account angepasst werden müssen.

{% highlight bash %}
msdeploy.exe -verb:sync
-source:iisApp=mydomain.com
-dest:iisApp=mydomain.com, computerName=https://webxxx.discountasp.net:8172/msdeploy.axd?site=mydomain.com,
userName=cpUsername, password=cpPassword, authtype=basic
-allowUntrusted
{% endhighlight %}

Je nach Umfang des Projektes dauert dies beim ersten Mal eine Zeit, jede weitere Synchronisierung geschieht jedoch inkrementell, es werden lediglich die geänderten Daten neu deployed.