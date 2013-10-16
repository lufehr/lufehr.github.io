---
layout: post
title:  C++ Algorithm copy
date:   2011-10-15 14:00:00
tags:	C++, STL, Algorithms, copy
assets: 
header: 2
---

#Beschreibung

<kbd>copy()</kbd> bietet die Möglichkeit, aus einer Sequenz eine zweite zu erzeugen. Beim lesen einer Sequenz wird eine Beschreibung benötigt wo begonnen und wo beendet werden soll. Um zu Schreiben wird lediglich ein Iterator benötigt, welcher das Ziel definiert. Es muss jedoch darauf geachtet werden, dass nicht hinter das Ende geschrieben wird (beispielsweise bei einem Vector).

#Algorithmus

{% highlight cpp %}
template <class InputIterator, class OutputIterator>
OutputIterator copy (InputIterator first, InputIterator last, OutputIterator result)
{
	while (first!=last) *result++ = *first++;
	return result;
}
{% endhighlight %}

#Code Example

{% highlight cpp %}
#include <algorithm> 	// for copy
#include <vector> 		// for vector
#include <iterator> 		// for iterator
#include <iostream> 		// for cout

int main ()
{
	std::vector<int> v;
	v.push_back(3);
	v.push_back(2);
	v.push_back(4);

	// Ausgabe: Standard Output (Console) => 3, 2, 4,
	copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, ", "));

	// Inhalt von vs nach copy => 3 2 4 0 0
	std::vector<int> vs(5);

	// Koennte das Ende von vs ueberschreiben
	copy(v.begin(), v.end(), vs.begin()); 

	// Inhalt von vi nach copy => 3 2 4
	std::vector<int> vi;

	// Elem. von v an vi hinten anfuegen
	copy(v.begin(), v.end(), back_inserter(vi)); 

	return 0;
}
{% endhighlight %}