---
layout: post
title:  User Interfaces Basics
date:   2011-09-21 14:00:00
tags:   Design, User Interfaces, Software Engineering
assets: 2011-09-21-user-interfaces-basics
header: 3
comments: true
---

User Interfaces sind Schnittstellen zwischen dem Informationssystem und dem Benutzer dessen. Dabei wird zwischen dem internen und dem externen Design unterschieden.

![node-adt-01]({{ site.url }}/assets/{{page.assets}}/user-interfaces-basics-1.png)

#Externes vs. Internes Design

Externes Design: Design von Presentation- & Action Language
Internes Design: Design der Implementierung

##Qualitätskriterien für internes Design

**Functionality / Funktionalität:**
Korrektheit, Angemessenheit, Interoperabilität, Ordnungsmässigkeit, Sicherheit

**Reliability / Zuverlässigkeit:**
Reife, Fehlertoleranz, Wiederherstellbarkeit

**Usability / Benutzbarkeit:**
Verständlichkeit, Bedienbarkeit, Erlernbarkeit, Robustheit

**Efficiency / Effizienz:**
Wirtschaftlichkeit, Zeitverhalten, Verbrauchsverhalten

**Maintainability / Wartbarkeit:**
Analysierbarkeit, Änderbarkeit, Stabilität, Testbarkeit

**Portability / Übertragbarkeit:**
Anpassbarkeit, Installierbarkeit, Konformität, Austauschbarkeit

##Kriterien für externes Design

Die zu bauenden Systeme sollten folgende Eigenschaften aufweisen:

**Effektivität:**
Benutzer können ihre Ziele erreichen

**Effizienz:**
Benutzer können diese Ziele mit angemessenem Aufwand erreichen

**Zufriedenheit:** 
Benutzer werden in ihrer Zufriedenheit nicht beeinträchtigt

#User Centered Design

Beim User Centered Design wird der Benutzer in den Analyse und Design Prozess miteinbezogen. Hierfür ist es notwendig, sich das benötigte Wissen über Vision, Umfeld, Benutzer und die Aufgabe & das System anzueignen.

Dabei sind in den folgenden Phasen jeweilige Aktionen auszuführen, resp. jeweilige Ergebnisse werden zurückgeliefert.

##Analyse:
**Aktivitäten:** Benutzer Beobachtung / Workshops

**Modellieren:**
*Ergebnisse:* Personas / Szenarios / Low-Fi GUI Design

**Spezifikation:**
*Ergebnisse:* Use Cases, Features / Requirement Documentation / Detail External Design

**Realisierung:**
*Ergebnis:* Internal Design Evaluation

Beim User Centered Desing wird oft die Methodik vom Paper Prototyping für Präsentations- und Interaktionsdesign angewednet. Hierbei geschieht der erste Schritt auf Papier, danach werden die Ergebnisse detailierter in Tools (zum Beispiel Balsamiq) übertragen und zum Schluss in SW-Tools (GUI-Tools) umgesetzt.