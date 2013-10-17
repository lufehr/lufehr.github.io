---
layout: post
title:  C++ Vector
date:   2011-06-05 14:00:00
tags:   C++, Vector
assets: 2011-06-05-cpp-vector
header: 3
comments: true
---

<code>std::vector<></code> ist der Standardcontainer für die Sammlung von gleichartigen Elementen.

#Eigenschaften

- Grösse wird automatisch angepasst 
- Hinten anfügen möglich ( <code>push_back()</code> ) 
- Initialisierung über Iteratoren möglich 
- Zugriff über Index möglich ( <code>v[5]</code> ) 
- Indizes von 0 bis max-1 
- keine Exceptions bei Zugriff ausserhalb / falsche Indizes (Ausnahme bei <code>.at()</code> Funktion)

#Verwendung von vector

{% highlight cpp %}
#include <vector>// Definition
std::vector<int> v(12);					// Laenge
std::cout << v.size() << std::endl;		// Zugriff
v[3] = 20;
std::cout << v[3] << std::endl;			// Element anhaengen
v.push_back(40);						// Letztes Element loeschen
v.pop_back();							// Alle Elemente loeschen
v.clear();								// Anzahl Elemente ermitteln
std::cout << v.size();
{% endhighlight %}

#vector<> mit einer Schleife abfüllen

{% highlight cpp %}
#include <iostream>
#include <vector>   
 
using namespace std;
 
int main()
{
    vector<int> v;
    int input;
    while(cin)
    {
        cin >> input;
        v.push_back(input);
    }
}
{% endhighlight %}

#vector<> Iteratoren

<table class="table table striped">
	<thead>
		<tr>
			<th>
				vector<>::iterator
			</th>
			<th>
				Iterator-Typ
			</th>
		</tr>
	</thead>
	<tbody>
		</tr>
		<tr>
			<td>
				<code>begin()</code>, <code>end()</code>
			</td>
			<td>
				Iteratoren zur Benutzung
			</td>
		</tr>
	</tbody>
</table>

![]({{ site.url }}/assets/{{page.assets}}/cpp-std-vector-1.png)

<code>end()</code> ist das Ende des Containers (hinter dem Ende), darauf darf nicht zugegriffen werden (keine Überprüfung –> undefiniertes Verhalten).

**vector<>::reverse_iterator**

<code>std::vector<></code> sowie auch <code>std::string</code> definieren auch Iteratoren welche vom Ende her iterieren. Die Iteratoren zur Benutzung heissen dabei <code>rbegin()</code> und <code>rend()</code>. Der Iterator-Typ ist <code>std::vector<>::reverse_iterator</code>. Hierbei ist der Zugriff auf <code>rend()</code> nicht erlaubt (Undefiniertes Verhalten).

{% highlight cpp %}
for(vector<int>::reverse_iterator it = v.rbegin(); it != v.rend(); ++it)
{
  cout << *it << endl;
}
{% endhighlight %}

**vector<> mit istream_iterator abfüllen**

<code>std::istream_iterator<Typ></code> zum Einlesen von Elementen des definierten Typs. Mittels Spezialobjekt ohne Stream wird ein EOF Marker markiert.

{% highlight cpp %}
istream_iterator<int> eof;
vector<int> v(istream_iterator<int>(cin),eof);
{% endhighlight %}

**Hinweis:** “whitespace” (Leerzeichen, Tabs, Zeilentrenner) wird überlesen. Alternative: <code>std::istreambuf_iterator<Typ></code>

#vector<> und Algorithmen

In C++ gibt es eine vielzahl an nützlichen Algorithmen welche natürlich auch im Zusammenhang mit <code>vector<></code> ihre Verwendung finden.

<table class="table">
	<tr>
		<td>
			<code>copy</code>
		</td>
		<td>
			Kopiert einen Bereich
		</td>
	</tr>
	<tr>
		<td>
			<code>find</code>
		</td>
		<td>
			Element suchen
		</td>
	</tr>
	<tr>
		<td>
			<code>count</code>
		</td>
		<td>
			Elemente zählen
		</td>
	</tr>
	<tr>
		<td>
			<code>accumulate</code>
		</td>
		<td>
			Elemente summieren
		</td>
	</tr>
</table>

**Suchen mit find()**

Mit dem Algorithmus <code>find</code> wird über Iteratoren nach einem Wert gesucht. Dabei wird ein Iterator als Ergebnis zurückgeliefert welcher entweder auf das gefundene Element, oder auf das Ende des Bereichs zeigt, falls kein Element gefunden wurde. Für die Verwendung des Algorithmus ist der Include von “algorithm” nötig.

{% highlight cpp %}
#include <algorithm> 
 
// ... 
istream_iterator<int> eof;
vector<int> v(istream_iterator<int>(cin),eof);
if(find(v.begin(), v.end(), 14) != v.end())
{
    count << "Element 14 gefunden!" << endl;
}
else
{
    cout << "kein Element gefunden!" << endl;
}
{% endhighlight %}

**Zählen mit count()**

<code>count</code> zählt wie oft ein Wert in einem bestimmten Bereich enthalten ist. Für die Verwendung des Algorithmus ist der Include von “algorithm” nötig.

{% highlight cpp %}
#include <algorithm>  
 
// ...
istream_iterator<int> eof;
vector<int> v(istream_iterator<int>(cin), eof);
cout << "Anzahl der Ziffer 4 im Bereich: " << count(v.begin(), v.end(), 4) << endl;
{% endhighlight %}

**Summieren mit accumulate()**

accumulate summiert Werte aus einem bestimmten Bereich auf. Dabei wird intern der + Operator verwendet. Für die Verwendung des Algorithmus ist der Include von “numeric” nötig.

{% highlight cpp %}
#include <numeric> 
 
// ...
istream_iterator<int> eof;
vector<int> v(istream_iterator<int>(cin), eof);
cout << "Summe: " << accumulate(v.begin(), v.end(), 0) << endl;
{% endhighlight %}

**Iterator-Funktion distance()**

distance() zählt die Elemente, welche zwischen zwei Iteratoren liegen.

{% highlight cpp %}
distance(v.begin(), v.end()) == v.size()
{% endhighlight %}

#vector<> mit boost::assign

Die Initialisierung von vector ohne Iteratoren ist aufwändig und mühsam (push_back). Abhilfe schafft boost::assign aus der boost Library. Für die Verwendung ist der Include “boost/assign.hpp” nötig.

{% highlight cpp %}
#include <boost/assign.hpp>
using namespace boost::assign; 
 
// ...
v += 12,15,20,80
{% endhighlight %}