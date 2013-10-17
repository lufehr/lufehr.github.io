---
layout: post
title:  Continous Integration - CruiseControl.NET with VisualSVN Server and Visual Studio
date:   2010-09-09 14:00:00
tags:   Continous-Integration, Tools, Build-Server, VSS, SVN
assets: 2010-09-09-ci-cruisecontrolnet-with-visualsvn-server-and-visual-studio
header: 3
comments: true
---

An der Embedded Computing Conference in Winterthur habe ich einen Vortrag über Continous Integration (CI) für Embedded Systeme gehört. Bei der Demo wurde das Open Source Projekt Hudson eingesetzt, was sehr interessant aussah. Deshalb habe ich mir heute einmal CruiseControl.NET angeschaut resp. aufgesetzt. Da ich selbst noch keine Erfahrungen mit solchen Systemen habe ist dies sicherlich noch verbesserungswürdig :).

#Ziel

Ziel ist es CruiseControl.NET in Verbindung mit VisualSVN und Visual Studio einzusetzen, dabei soll der Prozess folendermassen aussehen. Nach dem der Entwickler seine Änderungen am Projekt commitet und das Versionierungssystem das Repository aktualisiert hat, holt sich CruiseControl von einem Trigger angestossen automatisch die neuste Version und erstellt den Build in Verbindung mit Visual Studio’s devenv.com.

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-1.png)

#Setup

##Downloads

CruiseControl.NET Server, CCTray, Tools &rarr; [Downlaod](http://confluence.public.thoughtworks.org/display/CCNET/Download)

VisualSVN Server &rarr; [Download](http://www.visualsvn.com/server/download/)

##Setup CruiseControl.NET

Voraussetzung für die Installation ist das Microsoft .NET Framework (min. Version 2.0) und ein ASP.NET fähiger Webserver, vorzugsweise Microsoft IIS.

Die Installation ist simpel, CruiseControl.NET Setup ausführen und mit den Default-Werten bestätigen. Dasselbe mit den beiden anderen Setups CCTray und den Tools.

Zum Schluss des Setups wird gefragt, ob für das Dashboard ein Virtuelles Directory im IIS installiert werden soll. Trotz dem Aktivieren der Checkbox hat dies bei mir nicht geklappt, was jedoch nicht allzu schlimm ist, das Dashboard kann in ein paar Klicks manuell “erstellt” werden.

Dashboard manuell erstellen
Dazu muss zuerst der Internet Information Services (IIS) Manager (inetmgr) aufgerufen werden. Unter der Default Web Site eine neue Applikation hinzufügen und der Physical path zum webdashboard (ex. <kbd>C:\Program Files (x86)\CruiseControl.NET\webdashboard</kbd>) eintragen. That’s it.

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-2.png)

Das Dashboard ist nun über die URL http://localhost/CCNet aufrufbar. Sollte die Meldung *“No connection could be made because the target machine actively refused it …”* erscheinen muss CruiseControl.NET noch gestartet werden.

So weit so gut, auf die Konfiguration gehen wir später näher ein, nun wird zuerst der VisualSVN Server eingerichtet.

##Setup VisualSVN Server

Das Setup des VisualSVN Servers ist genauso simpel wie jenes von CruiseControl. Setup kann grundsätzlich auch mit den Default-Werten durchgeklickt werden, wenn der Pfad zu den Repositories und die Portangaben nicht explizit geändert werden möchten.

Nach Abschluss des Setups den VisualSVN Server Manager starten und ein neues Repository und ein neuer User erfassen.

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-3.png)

Nun kann mittels eines SVN Clients ein Projekt dem Repository hinzugefügt und commited werden. (zum Beispiel mittels VisualSVNfür Visual Studio)

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-4.png)

Nun stehen alle Daten bereit um dem Buildserver zu übergeben, dieser weiss jedoch noch nichts von dem soeben eingecheckten Projekt. Somit weiter zur Konfiguration.

###CruiseControl.NET Konfiguration

Um dem Buildserver mitzuteilen, dass er unser TestProject1 automatisch laden und builden soll ist etwas Handarbeit nötig. Unter <kbd>C:\Program Files (x86)\CruiseControl.NET\server</kbd> liegt das Config-File <kbd>ccnet.config</kbd>, welches erweitert werden muss.

{% highlight xml %}
<cruisecontrol xmlns:cb="urn:ccnet.config.builder">
  <project name="TestProject1">
    <sourcecontrol type="svn">
      <trunkUrl>https://localhost:8088/svn/TestProject1/trunk</trunkUrl>
      <workingDirectory>
        C:\Development\CCNet\TestProject1
      </workingDirectory>
      <username>username</username>
      <password>password</password>
    </sourcecontrol>
    <triggers>
      <intervalTrigger name="Subversion" seconds="30" />
    </triggers>
    <tasks>
      <devenv>
        <solutionfile>C:\Development\CCNet\TestProject1\TestProject1.sln</solutionfile>
        <configuration>Debug</configuration>
        <buildtype>Build</buildtype>
        <executable>C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\devenv.com</executable>
        <buildTimeoutSeconds>60</buildTimeoutSeconds>
      </devenv>
    </tasks>
  </project>
</cruisecontrol>
{% endhighlight %}

Diese Konfiguration bewirkt, dass der Buildserver alle 30 Sekunden das SVN Repository auf Aktualisierungen prüft. Sollte es Aktualisierungen geben, wird die Working Copy upgedatet und ein Build erstellt. Der Buildprozess stösst das Visual Studio eigene devenv.com mit der Übergabe des Solution-Files an.

Das Projekt ist nun im Dashboard ersichtlich und kann verwaltet und überwacht werden.

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-5.png)

###CCTray

Ein weiteres Tool für das Monitoring ist CCTray (bereits installiert). Über die Settings unter Build Projects muss das gewünschte Projekt hinzugefügt werden, danach wird man mittels Bubbles stets über die Aktivitäten und Fortschritte informiert.

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-6.png)

![]({{ site.url }}/assets/{{page.assets}}/ci-cruisecontrol.net-with-visualsvn-server-and-visual-studio-7.png)

#Fazit

Wie bereits zu Beginn erwähnt habe ich noch kenerlei Erfahrungen mit CI Systemen. CruiseControl.NET sieht interessant aus und scheint, wenn die Installation-/Konfiguration sorgfältig erstellt wurde, recht anständig zu laufen. Ich werde nun mal einige Tests durchführen und ggf. einen weiteren Artikel verfassen.