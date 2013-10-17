---
layout: post
title:  Markus Fehr + CO - A "One Pager"
date:   2013-03-18 14:00:00
tags:	Kirby, Website
assets: 2013-03-18-markus-fehr-co-a-one-pager
header: 2
comments: true
---


Für das Holzbauunternehmen [Markus Fehr + CO](http://markusfehr.ch) hat [fireup Software Solutions](http://fireup.ch) ein Redesign Entwickelt. Dabei sind wir für ein KMU Handwerksbetrieb aus der Schweiz einen etwas unüblichen Weg gegangen, mit einem "One-Pager".

Für die Umsetzung wurden die üblichen Technologien & Methoden (HTML5/CSS3, jQuery, etc.) eingesetzt. Und Kirby. Ich habe ja bereits in einem vorherigen Post von Kirby geschwärmt, nun möchte ich auch mal ein konkretes Beispiel dokumentieren.

Grundsätzlich ist eine mit Kirby aufgebaute Site so strukturiert, dass ein Verzeichnis jeweils einer effektiven Seite entspricht. Mit dem kleinsten Aufwand lässt sich aber auch ohne Probleme einen One-Pager abbilden.

#ONE-PAGER

Ein One-Pager ist grundsätzlich eine "normale" Website. Der Unterschied zu einer herkömmlichen Site ist, dass die Inhalte auf einer einzigen Seite dargestellt werden und beim Laden einer "Seite" resp. einer Section kein Wechsel auf eine neue Seite und damit kein neuer HTTP Request stattfindet. Dabei ist gemeint, dass die grundlegende Struktur nicht immer und immer wieder neu aufgebaut wird, natürlich können Inhalte mittels AJAX Aufrufen geladen werden. Meistens (so auch bei diesem Projekt) erfolgt die Navigation zu einer Section mittels vertikalem Scrolling, kann aber beliebig definiert werden (als Beispiel sei hier die Präsentations-Library [impress.js](http://bartaz.github.com/impress.js) erwähnt).

#MARKUSFEHR.CH

##SEITENAUFBAU

Die Struktur der Site wurde wie im folgenden Bild definiert. Die Navigation wird beim Scrollen zu einer Section (About, Services, Work, etc.) jeweils am oberen Rand fixiert, somit hat der Benutzer zu jedem Zeitpunkt die Navigation im Blickfeld.

![]({{ site.url }}/assets/{{page.assets}}/one-pager.png)

##FILESTRUKTUR

An der Filestruktur des Content Verzeichnisses ändert sich bei einem One-Pager nicht und sieht bei diesem Projekt in etwa folgendermassen aus.

![]({{ site.url }}/assets/{{page.assets}}/01.png)

##TEMPLATES

Als nächstes werden die Templates definiert, in diesem Falle nur ein (1) einziges, home.php.

{% highlight php %}
<?php 

snippet('header');

foreach($pages->visible() as $section) {
  snippet($section->uid(), array('data' => $section));
}

snippet('footer');

?>
{% endhighlight %}

Die Snippets header.php und footer.php werden auf herkömmliche Weise geladen. In der foreach Schleife in der Mitte wird für jede sichtbare Seite ein entsprechendes Snippet mit gleichem Namen geladen.

{% highlight xml %}
01-section1     =>      snippets/section1.php
02-section2     =>      snippets/section2.php
03-section3     =>      snippets/section3.php
04-section4     =>      snippets/section4.php
05-section5     =>      snippets/section5.php
{% endhighlight %}

##SNIPPETS

Die Snippets repräsentieren nun eine Section auf der Seite.

{% highlight html %}
<div id="container-section4">
    <h1>HOLZ</h1>
    <h2>Nachhaltig. Modern.</h2>
</div>
{% endhighlight %}

Um auf die Content-Daten zuzugreifen steht die Variable <code>$data</code> zur Verfügung welche beim laden der Section mit <code>array('data' => $section)</code> übergeben wurde. Somit können die im Content File definierten Felder ausgelesen werden.

{% highlight php %}
<?php echo html($data->title()) ?>
<?php echo kirbytext($data->text()) ?>
{% endhighlight %}

#QUELLEN & LINKS

(*) Dieser Post wurde auf Basis von "How to build a "one-pager" with Kirby" von Bastian Allgeier geschrieben (Konzept / Struktur). Der Inhalt ist spezifisch für das durchgeführte Projekt. 

- [Markus Fehr + CO Website](http://markusfehr.ch)
- [Kirby Website](http://getkirby.com)
- [How to build a "one pager" with Kirby by Bastian Allgeier](http://getkirby.com/blog/one-pager) (*)