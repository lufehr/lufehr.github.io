---
layout: post
title:  Smells & Refactorings - Naming
date:   2014-02-18 17:30:00
tags:	Smells, Code Smells, Refactoring, Naming
assets: 
header: 1
comments: true
---

Ich wollte bereits seit längerer Zeit über Code Smells und Refactoring schreiben, kam jedoch nie dazu. Nun nehme ich einen erneuten Anlauf und starte eine kleine **Smells & Refactoring Serie**, denn es schadet nicht, sich diese Dinge ab und an mal wieder ins Gedächnis zu rufen.

#Smell: Naming allgemein (Smells betreffend Namensgebung)

Ein guter Name für Bezeichner (Klassen, Methoden, Variablen, etc.) machen viele Kommentare überflüssig, deshalb sollte darauf geachtet werden, dass sie stets aussagekräftig sind.

Folgendes sind Smells schlechter Namensgebung:

- Type Embedded in Name
- Uncommunicative Name
- Inconsistent Name

##Refactorings
Die entsprechenden Refactorings sind:

- Rename Method
- Rename Field
- Rename Class 

#Smell: Type Embedded in Name

{% highlight csharp %}
// Name zusammengesetzt aus Wort und Argumenttyp
addLoan(Loan l);

// Typ in Namensprefix
int iSum;

// Variablename reflektiert Typ statt Zweck
Class aClass;
{% endhighlight %}

#Smell: Uncommunicative Name

{% highlight csharp %}
// Namen aus 1 oder 2 Zeichen
i;

// Namen ohne Vokale
Flgzg;

// Nummerierte Variable
sum1; sum2;

// Gesuchte Abkuerzungen
Flgzgnr;

// Irrefuehrende Namen
string average;
{% endhighlight %}

**Regel:** Wähle Namen, der Zweck wiederspiegelt.

#Smell: Inconsistent Names

{% highlight csharp %}
// Unterschiedliche Namen fuer das gleiche Vorhaben
add(); store(); insert(); put();
{% endhighlight %}

Konsistente Namen erzwingen zum Beispiel durch führen eines Projektglossars.

#Refactoring: Rename Method (Field/Class)
Das entsprechende Refactoring gemäss Martin Fowler's Catalog of Refactoring ist [Rename Method](http://www.refactoring.com/catalog/renameMethod.html)

#Quellen & Links

- [Catalog of Refactorings](http://www.refactoring.com/catalog)