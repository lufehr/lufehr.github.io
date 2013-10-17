---
layout: post
title:  Trees ADT - Traversals
date:   2011-10-03 14:00:00
tags:   ADT, Trees, Traversals, Algorithms
assets: 2011-10-03-trees-adt-traversals
header: 3
comments: true
---

Bäume sind eine der wichtigsten Datenstrukturen. Um sich in Bäumen zu bewegen ist es wichtig zu wissen, wie diese geordnet sind.

#Pre-Order Traversierung

Bei der Pre-Order Traversierung wird bei der Wurzel gestartet, diese wird direkt ausgegeben, danach wenn ein Left Child vorhanden ist wird dieser besucht und ausgegeben. Sobald kein Left Child mehr vorhanden ist wird entsprechend der Right Child besucht und ausgegeben. Das Ganze läuft natürlich so lange, bis alle Knoten besucht wurden.

{% highlight csharp %}
Visit(Node current) {
    if(current ==  null) {
        return;
    }
 
    Process(current.Value);
    Visit(current.Left);
    Visit(current.Right);
}
{% endhighlight %}

Der folgende Baum wird per Pre-Order Traversierung durchlaufen:

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-1.png)

und erzeugt somit folgenden Output:

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-2.png)

#In-Order Traversierung

Bei der In-Order Traversierung wird jeweils auch zuerst der Left Child besucht, der Current Node wird jedoch erst bei der ‘Rückkehr’ ausgegeben. Am Schluss wird dann noch der Right Child besucht.

{% highlight csharp %}
Visit(Node current) {
    if(current == null) {
        return;
    }
 
    Visit(current.Left);
    Process(current.Value);
    Visit(current.Right);
}
{% endhighlight %}

Derselbe Baum wird per In-Order Traversierung durchlaufen:

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-3.png)

und erzeugt folgenden Output:

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-4.png)

#Post-Order Traversierung

Und zum Schluss bei der Post-Order Traversierung werden zuerst die beiden Childrens besucht (Left dann Right), bevor der Current Node ausgegeben wird.

{% highlight csharp %}
Visit(Node current) {
    if(current == null) {
        return;
    }
 
    Visit(current.Left);
    Visit(current.Right);
    Process(current.Value);
}
{% endhighlight %}

Dies erzeugt wiederum beim selben Baum

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-5.png)

den folgenden Output:

![]({{ site.url }}/assets/{{page.assets}}/trees-traversals-6.png)

