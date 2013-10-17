---
layout: post
title:  Continous Integration - Hudson with VisualSVN Server and Visual Studio
date:   2010-09-10 14:00:00
tags:   Continous-Integration, Tools, Build-Server, VSS, SVN, Hudson
assets: 2010-09-10-ci-hudson-with-visualsvn-server-and-visual-studio
header: 3
comments: true
---

Gestern hatte ich bereits über dieses Thema gebloggt – Continuous Integration. Dabei ging es spezifisch um den CruiseControl.NET Server in Verbindung mit VisualSVN Server und Visual Studio. Nun habe ich heute einen zweiten Buildserver, Hudson, aufgesetzt und getestet.

#Ziel

Das Ziel hierbei ist es nun den Hudson Buildserver in Verbindung mit VisualSVN und Visual Studio aufzusetzen. Der Prozess sieht gleichermassen aus wie unter Einsatz von CruiseControl.NET. Der Entwickler checkt seine Änderungen am Projekt ein, das Versionierungssystem aktualisiert das Repository, der Hudson Server checkt das Projekt automatisch aus (Trigger) und führt den Build durch. Nach Abschluss erhält der Entwickler entsprechende Statusmeldungen über den Verlauf des Buildprozesses.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-1.png)

#Setup

##Downloads

Hudson Server &rarr; [Download](http://hudson-ci.org/) 

VisualSVN Server &rarr; [Download](http://www.visualsvn.com/server/download/) 

JAVA Runtime Environment &rarr; [Download](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

##VisualSVN Server

Die Installation von VisualSVN Server ist identisch dem Post Continous Integration: CruiseControl.NET with VisualSVN Server & Visual Studio.

##Setup Hudson on Windows

Voraussetzung für die Installation ist die JAVA Runtime Environment, mehr wird nicht benötigt.
Die Installation unter Windows ist schnell und leicht gemacht. Nach dem Download von Hudson (hudson.zip), dieses in einen gewünschten Zielordner kopieren und nach hudson.war umbenennen. Anschliessend mittels CLI in das entsprechende Verzeichnis wechseln und den folgenden Befehl ausführen.

{% highlight bash %}
java -jar hudson.war
{% endhighlight %}

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-2.png)

Nun kann Hudson bereits das erste Mal ausgeführt werden, über die URL http://localhost:8080.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-3.png)

Hudson als Windows Service installieren

Damit der Server nicht immer über das CLI von Hand gestartet werden muss gibt es die Möglichkeit Hudson als Windows Dienst zu installieren. Dies kann ganz einfach über das WebUI gemacht werden. Unter ***Hudson verwalten – Als Windows-Dienst installieren*** auswählen,

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-4.png)

Installationspfad angeben und installieren.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-5.png)

Nach Abschluss wird im Command Prompt eine Abschlussmeldung ausgegeben.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-6.png)

und der Dienst ist unter den Windows Services (services.msc) gelistet.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-7.png)

Plugins installieren

Hudson bringt von Grund auf eine Vielzahl von Plugins mit sich, welche mit wenigen Klicks über ***Hudson verwalten – Plugins verwalten*** installieren und verwalten lassen.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-8.png)

ch habe mich für folgende Plugins entschieden:

- MSTest Plugin 
- NUnig Plugin 
- xUnit Plugin 
- NAnt Plugin 
- Static Code Analysis Plugin 
- Task Scanner Plugin 
- Violations PowerShell Plugin 
- Warning Plugin 
- MSBuild Plugin 
- Green Balls Plugin

Nach der Installation wird der Hudson neu gestaret und die Plugins sind direkt verfügbar.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-9.png)

Konfiguration

Im Gegensatz zu CruiseControl ist die Konfiguration in Hudson ein Leichtes. Es kann alles über das WebUI konfiguriert werden, ohne endloslange XML Konfigurationsfiles zu editieren.

Über ***Hudson verwalten – System konfigurieren*** die Serverkonfiguration aufrufen.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-10.png)

Ich habe mehr oder weniger alles beim Standard belassen, lediglich die Einstellungen zu MSBuild und NAnt müssen angepasst werden (sofern diese Builder verwendet werden).

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-11.png)

Neuer Job

Das System ist nun soweit vobrereitet, dass der erste Build-Job erstellt werden kann. ***Neuen Job anlegen*** auswählen, um den Wizard zu starten. Job Name eintragen und Projektart auswählen.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-12.png)

Im nächsten Schritt wird zuerst das Source-Code-Management konfiguriert. Hier werden nun die Subversion Daten (VisualSVN Server/Repository) eingetragen.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-13.png)

Von der Fehlermeldung bezüglich fehlendem Zugriff sollte man sich nicht beirren lassen, das Repository erfordert eine Authentifizierung, welche direkt über den angezeigten Link ausgeführt werden kann.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-14.png)

Als nächstes kann definiert werden, in welchem Zeitintervall Hudson den SVN Server nach Aktualisierungen überprüfen soll, um den Buildvorgang automatisch anzustossen. Die Eingabe von * * * * * definiert den minütlichen Intervall.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-15.png)

Last but not least sind noch die Angaben zum Buildverfahren nötig. Für das TestProject1 wird MSBuild unter Angabe des Visual Studio Solution Files (TestProject1.sln) verwendet.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-16.png)

Auf die zusätzlichen Settings bez. Reporting, Statusmeldungen etc. gehe ich in diesem Post nicht ein.

Nach Abschluss des Wizards ist das Projekt im Dashboard ersichtlich und der erste Buildvorgang kann gestartet werden.

![]({{ site.url }}/assets/{{page.assets}}/ci-hudson-with-visualsvn-server-and-visual-studio-17.png)

#Fazit

Hudson ist sehr leicht und schnell zu installieren. Das WebUI ist übersichtlich und einfach zu bedienen. Im Vergleich zu CruiseControl ist es vor allem der Konfigurationsaufwand, welcher bei den ersten Tests massiv geringer ist. Ich bin gespannt wie sich das Ganze bei Live-Projekten verhalten wird.