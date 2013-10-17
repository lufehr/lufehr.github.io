---
layout: post
title:  C++ Algorithm accumulate
date:   2011-05-14 14:00:00
tags:   C++, STL, Algorithms
assets: 
header: 3
comments: true
---

#Beschreibung

Alle in einem Bereich enthaltene Elemente werden aufsummiert. An Stelle der Addition können auch andere Operationen wie Funktoren eingesetzt werden.

#Algorithmus

{% highlight cpp %}
// addition
template <class InputIterator, class T>
T accumulate (InputIterator first, InputIterator last, T init)
{
  while (first!=last) init = init + *first++;
  return init;
}
 
// general operation
template <class InputIterator, class T, Class binaryOperation>
T accumulate (InputIterator first, InputIterator last, T init, binaryOperation)
{
  while (first!=last) init = op(init, *first++);
  return init;
}
{% endhighlight %}

#Code Example

{% highlight cpp %}
#include <numeric>    	// for accumulate
#include <vector>		// for vector
#include <functional>	// for multiplies
#include <iostream>   	// for cout
 
int main()
{
    std::vector<int> v;
    v.push_back(3);
    v.push_back(2);
    v.push_back(4);
 
    // addition
    int sum = accumulate(v.begin(), v.end(), 0);
    std::cout << "Summe: " << sum << std::endl; // Ausgabe => Summe: 9
 
    // Operation -> std::multiplies<int>()
    int prod = accumulate(v.begin(), v.end(), 1, std::multiplies<int>());
    std::cout << "Produkt: " << prod << std::endl; // Ausgabe => Produkt: 24
}
{% endhighlight %}