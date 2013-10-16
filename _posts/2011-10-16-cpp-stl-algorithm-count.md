---
layout: post
title:  C++ Algorithm count
date:   2011-10-16 14:00:00
tags:	C++, STL, Algorithms, count
assets: 
header: 2
---

#Beschreibung

<kbd>count()</kbd> liefert die Anzahl Elemente einer Sequenz mit dem Wert *value*. 

#Algorithmus

{% highlight cpp %}
template <class InputIterator, class T>
iterator_traits<InputIterator>::difference_type
count (InputIterator first, InputIterator last, const T& value)

// Das Verhalten dieser Funktion ist vergleichbar mit:
template <class InputIterator, class T>
ptrdiff_t count (InputIterator first, InputIterator last, const T& value)
{
	ptrdiff_t ret=0;
	while (first != last) if (*first++ == value) ++ret;
	return ret;
}
{% endhighlight %}

#Code Example

{% highlight cpp %}
#include <algorithm>	// for count
#include <string>		// for string
#include <iostream>		// for cout

int main ()
{
	std::string foo = "Foobar";
	int n = count(foo.begin(), foo.end(), ’o’);

	// Ausgabe => Anzahl o: 2
	std::cout << "Anzahl o: " << n << std::endl; // Ausgabe => Anzahl o: 2

	return 0;
}
{% endhighlight %}