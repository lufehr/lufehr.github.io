---
layout: post
title:  Webdev Git Workflow
date:   2013-03-19 14:00:00
tags:	Git, Development
assets:
header: 2
comments: true
---

Seit einiger Zeit bin ich auf der Suche nach meinem perfekten Workflow für die Entwicklung von Webprojekten. Bislang wurde noch vieles von Hand ausgeführt, was gerade bei CMS Installationen fast unumgänglich war. Mit der Zeit und mit immer mehr verschiedenen Projekten und damit verbunden Zugangsdaten, Konfigurationen, etc. etc. wurde dies aber stets mühsamer. Zeit für eine Veränderung!

Da ich nun vermehrt [Kirby](http://getkirby.com) als mein CMS Nr. 1 einsetze, werden Stimmen für ein Git-Deployment-Workflow laut.

Da einer meiner Hosting Provider [cyon][ln-cyon] Git auf ihren Shared Hostings anbietet, habe ich mich für die folgende Konfiguration entschieden.

#Server

Auf dem Server werden zwei Repositories geführt, das eine dient als Hub (Bare Repo), das andere repräsentiert die Live Site.

##Hub

Der Hub fungiert als Schnittstelle zwischen der Live Site und der Development Version (Client). Dabei wird auf dem Client der Hub als Remote Repo hinzugefügt. Nach einem Commit/Push vom Client wird mittels Hook von der Live-Site einen Pull vom Hub ausgeführt. Im umgekehrten Fall, sollte es Updates direkt an der Live-Site geben (direkt durch Kunde o.ä.), wird auf dem Live Repo ein Add / Commit / Push (zum Hub) ausgeführt. Der Client kann anschliessend wiederum ein Pull (vom Hub) ausführen und ist wieder auf dem aktuellen Stand für die Entwicklung. 

###Repository

{% highlight bash %}
mkdir hub.git
cd hub.git
git init --bare
{% endhighlight %}

Es sollte darauf geachtet werden, dass das Repository nicht öffentlich zugänglich ist.

###Hook

Anschliessend wird der <code>post-update</code> Hook angelegt (mv post-update.sample post-update) und mit folgenden Zeilen ergänzt.

{% highlight bash %}
echo "Pulling changes into live (post update hook)"
cd ~/www/site.ch/live || exit
unset GIT_DIR
git pull hub master
{% endhighlight %}

##Live Site

Die Live-Site ist das eigentliche Root Directory des Webservers, resp. des Virtual Hosts, hier werden die effektiven Daten für die Ausführung der Site bereitgestellt.

###Repository

{% highlight bash %}
mkdir live
cd live
git init
git remote add hub ~/www/site.ch/hub.git
{% endhighlight %}

Auch dieses Repo sollte gegen Zugriff von Aussen geschützt werden, beispielsweise mittels .htaccess.

#Client (Development)

##Repository

Auf dem Client wird ebenfalls ein Repository angelegt und der Hub als Remote eingetragen. Von hier werden zum ersten mal Daten committed / gepushed.

{% highlight bash %}
cd dev/site.ch
git init
git remote add hub ssh://user@server.ch/~/www/site.ch/hub.git
git add .
git commit -m "Initial Commit"
git push hub master
{% endhighlight %}

Nach dem Push zum Hub wird nun der Git post-update Hook ausgeführt, welcher die Live-Site aktualisiert.

#Fazit

Mir gefällt dieser Ansatz, denn es ist nur noch in wenigen Fällen ein SSH Login auf den Server nötig. Zudem ist kein Drittanbieter (Github, Bitbucket, o.ä.) mehr notwendig und somit auch keine zusätzlichen Deployment-Scripts. Ich denke ich werde nun mal eine Weile mit dieser Lösung arbeiten und schauen wie es sich bewährt. Gerade für statische oder filebased-CMS Seiten ist dies aber sicherlich ein eleganter weg.

#Quellen & Links

- [Git Website][git-website]

[git-website]: http://git-scm.com
[ln-cyon]: http://cyon.ch
