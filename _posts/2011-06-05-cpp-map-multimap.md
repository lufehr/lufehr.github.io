---
layout: post
title:  C++ Map/Multimap
date:   2011-06-05 14:00:00
tags:   C++, Map, Multimap
assets: 
header: 3
comments: true
---

Mittels <code>std::map<K,V></code> kann eine Nachschlagtabelle für key-value Paare definiert werden. Diese verhält sich analog zu java.util.Map in Java, jedoch sortiert. Die Datenstruktur wird auch als “assoziatives Array” oder Dictionary bezeichnet.
 
#Eigenschaften
 
- Jeder Key darf nur einmal enthalten sein (Ausnahme bei <code>std::multimap<K,V></code>) 
- Die map wird nach Keys sortiert 
- Wenn nach dem Key gesucht wird, kann ein effizienter Suchzugriff erzielt werden 
- Zugriff wie bei einem Array über [key] möglich 
- Values können modifiziert werden, Keys hingegen nicht (lediglich über delete/create)
 
#Iterator für std::map
 
Jedes Element einer map repräsentiert ein Paar aus einem Key und einem Value. Für diesen Zweck gibt es in C++ eine Standard Datenstruktur.

{% highlight cpp %}
std::pair<Key,Value>
{% endhighlight %}

Ein <code>map::iterator</code> verweist deshalb nicht direkt auf einen Key oder Value der map-Einträge, sondern auf diese Paare.
 
##Zugriff über Iterator

{% highlight cpp %}
map<string,int> m;
map<string,int>::iterator it = m.begin();  // Zugriff auf Key
(*it).first
// oder
it->first  // Zugriff auf Value
(*it).second
// oder
it->second
{% endhighlight %}

#Initialisieren mit boost::assign
 
Wie auch für den <code>vector<></code> gibt es für maps eine elegantere Möglichkeit um die Initialisierung durchzuführen als die folgende:

{% highlight cpp %}
map<string, int> m;
m["Hans"]=60;
m["Fritz"]=23;
m["Sepp"]=78;
{% endhighlight %}

nämlich ebenso mit <code>boost::assign</code> der boost Library.

{% highlight cpp %}
#include <boost/assign.hpp>
using namespace boost::assign;
 
// ...
 
insert(m) ("Hans", 60) ("Fritz", 23) ("Sepp", 78);
{% endhighlight %}