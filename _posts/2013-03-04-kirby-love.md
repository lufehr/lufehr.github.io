---
layout: post
title:  Kirby ( &#9829; )
date:   2013-03-04 14:00:00
tags:	Java, Threads, Concurrency
assets: 2013-03-04-kirby-love
header: 3
comments: true
---

Seit einiger Zeit nun verfolge ich die Entwicklung des file-based CMS (link: http://getkirby.com text:Kirby) und eines kann ich sagen, je länger man sich damit beschäftigt, desto mehr Freude hat man daran. Einige mögen jetzt sagen, "file-based, was? Ohne Datenbank läuft gar nichts.", ok, aber mal ganz ehrlich, Wordpress, TYPO3, whatever in Ehren, wie oft waren solche Installationen im Vergleich zur Projektgrösse einfach nur Overhead? Oft.

Ich will hier keineswegs behaupten, Kirby sei die eierlegende Wollmilchsau im CMS Dschungel. Auch will ich andere Systeme wie z.B. Wordpress, Drupal oder TYPO3, welche ich auch öfters bei Projekten einsetze oder eingesetzt habe, nicht abwerten, aber für gewisse Projekte ist Kirby definitiv die eleganteste und effizienteste Lösung, die Wollmilchsau.

# ANALYSE / TREND
CMS Systeme mit einem fancy-pancy UI und tausenden von Konfigurations-Optionen sind cool, für den Benutzer, bei der ersten Präsentation. Sobald der Enduser mit dem System arbeiten will/muss kommen Fragen auf, welche entweder auf das eben zu fancy-pancy, oder auf ein zu kompliziertes Interface zurückzuführen sind. Klar, dieser Punkt kann mit einem geringen bis mittleren Aufwand an Schulung wett gemacht werden, aber lohnt sich das auch? Nehmen wir als Beispiel ein KMU in der Handwerksbranche mit einer kleinen Website welche alle drei Monate von einem Mitarbeiter der Firma aktualisiert wird. Nebst der Infrastruktur, Installation des CMS, Design, Adaption des Designs ans System und weiteren Arbeiten ist zusätzlich eine Schulung für mindestens einen Mitarbeiter nötig um die Website zu aktualisieren. Da diese Aktualisierung nur sporadisch stattfindet, besteht die Möglichkeit, dass das gelernte bei der Schulung bereits wieder vergessen ist, oder ein Update stellt die Standardprozesse wieder auf den Kopf stellt. Bald finden es die Benutzer nicht mehr so interessant, sich nebst ihren eigentlichen Tätigkeiten auch noch um die Website zu kümmern, die Aktualisierungen werden nicht mehr so oft durchgeführt und nach ein paar Monaten heisst es: "Macht ihr das für uns, wir haben keine Zeit dafür.".

Dieser Trend stelle ich vermehrt fest. Die Website ist wichtig, keine Frage und dem sind sich auch die meisten bewusst, aber sie wollens sich nicht mit noch mehr Systemen herumschlagen müssen, keine Zeit verlieren um "lediglich" die Website zu aktualisieren. Ein Standpunkt welcher absolut vertretbar ist. Hier kommt Kirby ins Spiel.

# DIE RETTUNG. MANCHMAL.
Wie gesagt, Kirby ist nicht die Lösung für alle Probleme und wir sprechen hier auch nicht von Grossprojekten, aber für eben gerade die kleineren bis mittelgrossen Projekte kann Kirby definitiv die Rettung sein. Und dies sowohl für den Kunden, den Redakteur, als auch den Entwickler.

## DESIGNER
Ich bin kein Profi bezüglich CSS, trotzdem mache ich die Dinge gerne selbst. Auch wenn ich dabei manchmal etwas zu viel Zeit verliere, mit Kirby hält sich diese definitiv in Grenzen. Es gibt keinerlei Einschränkungen, keine Plugins oder Widgets welche von Haus aus irgendwelche Styles definieren. Nichts.

Will man mit (link:http://twitter.github.com/bootstrap/ text:Twitter Bootstrap) arbeiten, arbeitet man damit. (link: http://foundation.zurb.com/ text: Foundation), kein Problem. Man hat die volle Kontrolle, ohne Restriktionen. Für diesen Blog habe ich (link: http://stuffandnonsense.co.uk/projects/320andup/ text: 320 and Up) verwendet, herunterladen, verlinken, fertig.

## ENTWICKLER
Auch für den Entwickler ist Kirby wunderbar. Das System baut auf dem gleichnamigen (link:http://toolkit.getkirby.com/ text: Kirby Toolkit) auf und glänzt durch seine Einfachheit und Eleganz. Templates sowie Content sind in einer Filestruktur organisiert und folgen dem Prinzip **convention over configuration**. Templates sind in Rekordzeit erstellt, Plugins implementiert. Die Installation fällt ohne Datenbank gänzlich weg, lediglich ein paar Konfigurationseinstellungen müssen je nach Betriebsart vorgenommen werden. Diese file-based Lösung ist für den Entwickler zudem interessant, da es auch den Deployment-Prozess massiv vereinfachen kann. Da keine Database-Migrations o.ä. notwendig sind könnte der Prozess zum Beispiel folgendermassen aussehen: 

**Git Checkout &rarr; Modification &rarr; Git push &rarr; Auto-Deploy to Webserver**

Keine Migrations, kein FTP, ja nicht mal ein Backup vorweg ist mehr notwendig. Im Fehlerfall wird einfach ein früherer Stand zurückgeholt (ok, hier ist ein SSH Login nötig ;)).

Und wenn für die Aktualisierung auch zum alt bekannten FTP Tool gegriffen wird, das ist keine Schande und ist auch mit nur wenig Aufwand verbunden.

![Kirby]({{ site.url }}/assets/{{page.assets}}/01.jpg)

## KUNDE / REDAKTEUR
Nun soll das Ganze ja aber auch für den Kunden / Redakteur einfach und benutzerfreundlich sein, falls dieser die Aktualisierungen selbst durchführen möchte. Auch hier gibt es mehrere Ansätze. Entweder ist der Enduser technisch versiert und er weiss mit FTP und einem Markdown Editor umzugehen, dann sind keine weiteren Schritte mehr nötig, auch die Schulung dafür hält sich in Grenzen. Eine andere Möglichkeit, vor allem für technisch nicht so versierte Enduser, ist das (link: http://getkirby.com/docs/panel text:Kirby Panel), ein simples, schlichtes und komplett an die Bedürfnisse des Kunden anpassbares User Interface.

![Kirby]({{ site.url }}/assets/{{page.assets}}/02.jpg)

# KOSTEN
Wie immer in der heutigen Zeit, spielt natürlich auch die Kostenfrage eine grosse Rolle, gerade auch für kleine Firmen. Hierzu lässt sich sagen, Kirby ist nicht gratis, es ist geschenkt. **Hä?** - Ja, ernsthaft, die Lizenz Pro "Installation" kostet $39, den Benefit welcher sich durch die Vereinfachung der Entwicklung und dadurch Verkürzung der Umsetzungszeit ergibt, ist ein Vielfaches der $39.

# FAZIT
Lange ist es her, als ich das letzte Mal solch Spass hatte mit einem System zu arbeiten. Es ist simpel, es ist schnell, es ist elegant. Es ist GROSSARTIG.

# QUELLEN & LINKS

- [Kirby Website](http://getkirby.com)
- [Kirby Panel](http://getkirby.com/docs/panel)
- Bilder stammen von der offiziellen Website von Kirby CMS
