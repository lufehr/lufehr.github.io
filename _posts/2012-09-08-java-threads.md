---
layout: post
title:  Java Threads
date:   2012-09-08 14:00:00
tags:	Java, Threads, Concurrency
assets: 
header: 2
---

In Java gibt es zwei verschiedene Möglichkeiten einen Thread zu erstellen. 

#Variante: Klasse erweitern

Die erste Möglichkeit besteht darin, die gewünschte Klasse von der Klasse Thread abzuleiten und die Methode run() zu überschreiben:

{% highlight java %}
public class MyExtendedThread extends Thread {
    public MyExtendedThread(String name) {
        super(name);
    }
    
    public void run() {
        // do some stuff
    }
}
{% endhighlight %}

{% highlight java %}
public class MyExtendedThreadTest {
    public static void main(String[] args) {
        new MyExtendedThread("Thread 1").start();
    }
}
{% endhighlight %}

Der Nachteil bei dieser Variante ist, dass Java nur Single Inheritance unterstützt, falls die Klasse also bereits von einer anderen ableitet, kann diese Variante nicht gewählt werden.

#Variante: Interface implementieren

Um trotzdem nicht auf Threads verzichten zu müssen kann als zweite Variante das Interface *Runnable* implementiert werden.

{% highlight java %}
public class MyRunnableThread implements Runnable {
    public void run() {
        // do some stuff
    }
}
{% endhighlight %}

{% highlight java %}
public static void main(String[] args) {
    MyRunnableThread t = new MyRunnableThread();
    new Thread(t, "Thread 1").start();
}
{% endhighlight %}

Der Nachteil hierbei ist die etwas "aufwändigere" Implementierung.

#Variante: Runnable mit Anonymous Inner Class

Eine weitere Möglichkeit besteht darin, einen Thread mittels einer Anonymen Inneren Klasse zu implementieren. Hierbei spart man sich etwas Schreibarbeit auf Kosten der Wiederverwendbarkeit.

{% highlight java %}
public class MyAnonymousClassTest {
    public static void main(String[] args) {
        Runnable t = new Runnable() {
            public void run() {
                // do some stuff
            }
        };
 
        new Thread(t, "Thread 1").start();
    }
}
{% endhighlight %}