---
layout: post
title:  SQLSM 2008 Collation Bug
date:   2010-08-17 14:00:00
tags:   MSSQL, SQL, Bug
assets: 2010-08-17-sqlsm-2008-collation-bug
header: 3
comments: true
---

Seit kurzem habe ich meinen Entwicklungsserver auf Microsoft Windows Server 2008 mit IIS7, SQL Server Express 2008 und dem darin enthaltenen Microsoft SQL Server Management Studio (SSMS) umgestellt.

Der Funktionsumfang und die teils neue und zu Beginn etwas gewöhnungsbedürftige Verwaltung ist wirklich gelungen und es macht richtig Spass in einer solchen Umgebung zu entwickeln.

Leider jedoch, gibt es im neuen SSMS einen kleinen Bug, nichts grosses, aber ärgerlich wenn man danach suchen muss… wie immer halt ;)

#Situation

Viele Hosting-Provider stellen SQL Server zur Verfügung. Natürlich steht nicht für jeden Kunden eine eigene SQL Instanz zur Verfügung, was bedeutet, dass X Kunden den gleichen Server verwenden. Dies hat zur Folge, dass beim Verbinden auf den SQL Server alle darauf eingerichteten Datenbanken ersichtlich sind. Selbstverständlich hat man keinen Zugriff darauf, es werden jedoch die DB-Namen gelistet (db1_KundeAB, db3_KundeXY). Man muss also zuerst alle DBs durchbrowsen (eingedeutscht? :D ) um seine eigene zu finden.

In der alten Version von SSMS, in Verbindung mit dem SQL Server Express 2005 ging dies ohne Probleme, die DBs wurden aufgelistet und man konnte seine eigene auswählen.Im Neuen SSMS mit dem SQL Server Express 2008 erhält man jedoch bei diesem Vorhaben eineneFehlermeldung.

**Error:** The server principal "User_KundeAB" is not able to access the database "Database_KundeXY" under the current security context. (Microsoft SQL Server, Error: 916)

![]({{ site.url }}/assets/{{page.assets}}/SQLSSMSCollationBug1.png)

Die Meldung sagt soviel aus wie, der User des Kunden AB hat keinen Zugriff auf die Datenbank des Kunden XY, was ja auch richtig ist. Nun auf die DB anderer Kunden möchte ich ja auch gar keinen Zugriff, lediglich auf meine :D.

#Lösung

Die Lösung ist folgendermassen. Sobald die Verbindung zum Server steht, öffnet man mit F7 den Objekt-Explorer. In dessen Tabellenkopf über das Kontextmenü hat man die Möglichkeit die anzuzeigenden Spalten auszuwählen. Per Default ist die Spalte “Collation” ausgewählt. Dieser Hacken entfernen und die Ansicht aktualisieren. Nun werden die Datenbanken wie gewohnt aufgelistet.

![]({{ site.url }}/assets/{{page.assets}}/SQLSSMSCollationBug2.png)