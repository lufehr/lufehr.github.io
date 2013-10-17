---
layout: post
title:  C++ Set/Multiset
date:   2011-06-05 14:00:00
tags:   Cpp, Set, Multiset
assets: 
header: 3
comments: true
---

In C++ gibt es nebst <code>vector<></code> und string auch Standardklassen für Mengen von Elementen, <code>set<></code> und <code>multiset<></code>.

#std::set<T>

##Eigenschaflten

- Jedes Element kann nur einmal vorkommen (keine Duplikate) 
- Automatisch sortiert

##Funktionen

Nebst den Standard-Container Member-Funktionen stehen Funktionen zur Verfügung welche mit Element-Werten anstatt mit Iteratoren arbeiten.

{% highlight cpp %}
s.find(Element e) 
s.insert(Element e) 
s.erase(Element e)
{% endhighlight %}

#std::multiset<T>

##Eigenschaften

- Elemente können mehrfach vorkommen (Duplikate erlaubt) 
- Automatisch sortiert

**Hinweis:** Für den Element Typ muss wegen der Sortierung der <code><</code> Operator definiert sein (für int, string, etc. ist dies automatisch der Fall).

{% highlight cpp %}
#include <set>
#include <string>
 
using namespace std;
 
// ...
 
set<string> setStr;
string s = "A";
 
setStr.insert(s);
s = "B";
setStr.insert(s);
s = "C";
setStr.insert(s);
 
for(set<string>::const_iterator it = setStr.begin(); it != setStr.end(); ++it)
{
    cout << *it << endl;
}
{% endhighlight %}