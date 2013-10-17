---
layout: post
title:  C++ Iterators
date:   2011-06-14 14:00:00
tags:   C++, Iterators
assets: 
header: 3
comments: true
---

In C++ setzen praktisch alle Algorithmen der STL auf Iteratoren auf. Diese Iteratoren legen zum Beispiel folgende Positionen fest:

Begin und End Insert- und Remove-Position Search-Position

Dabei ist ein Range ein Paar solcher Iteratoren. Zum Beispiel <code>[begin(), end()]</code> - Im Gegensatz zum Anfang gehört das Ende des Ranges nicht mehr zum Range hinzu (<code>end()</code> ist undefiniert).

#Verhalten

Die im folgenden aufgelisteten Operationen sind für alle Iteratoren definiert.

{% highlight cpp %}
T& iterator::operator*() 
{% endhighlight %}

Liefert jenes Element, an dessen Position sich der Itartor befindet. Wenn das Element enthaltene Komponenten hat, kann mit dem <code>-></code> Operator darauf zugegriffen werden.

{% highlight cpp %}
iterator::operator++()
{% endhighlight %}

Setzt den Iterator ein Element weiter. Hierbei ist zu beachten, dass stets die Präinkrement Variante verwendet werden sollte.

{% highlight cpp %}
iterator::operator==() und iterator::operator!=()
{% endhighlight %}

Vergleichsoperatoren

#Kategorien

##Input-Iteratoren
Einmaliger Durchlauf ist möglich/wird gefordert Read Zugriff via <code>operator*()</code> 
Modell: <code>istream_iterator<T></code>

##Output-Iterator
Einmaliger Durchlauf ist möglich/wird gefordert nur write Zugriff via <code>operator*()</code>
Modell: <code>ostream_iterator<T></code>

##Forward-Iterator
mehrmaliger Durchlauf ist möglich/wird gefordert Zugriff via <code>operator*()</code> (wenn <code>const_iterator</code>, nur read Zugriff)

##Bidirectional-Iterator
Zusätzlich ist der <code>operator--()</code> definiert vorwärts und rückwärts möglich 
Modell: <code>list<T>::iterator</code>

##Random Access-Iterator
zusätzlich ist der <code>operator[]()</code> definiert Möglichkeit zur Integer-Addition Subtraktion zusammengehörender Iteratoren für Ermittlung der Distanz 
Modell: <code>vector<T>::iterator</code>, Zeiger auf Array
