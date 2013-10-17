---
layout: post
title:  Stack ADT (Array Based)
date:   2011-09-25 14:00:00
tags:   ADT, Stack, Java
assets: 
header: 2
comments: true
---
Der Stack ist die simpelste aller Datenstrukturen, trotzdem aber eine der wichtigsten. Er operiert nach dem last-in-first-out (LIFO) Prinzip. Der Stack ADT unterstützt die folgenden Basis-Methoden:

<table class="table">
	<tr>
		<td>
			<kbd>push(e)</kbd>
		</td>
		<td>
			Insert element e, to be the top of the stack
		</td>
	</tr>
	<tr>
		<td>
			<kbd>pop()</kbd>
		</td>
		<td>
			Remove from the stack and return the top element on the stack; an error occurs if the stack is empty
		</td>
	</tr>
	<tr>
		<td>
			<kbd>size()</kbd>
		</td>
		<td>
			Return the number of elements in the stack
		</td>
	</tr>
	<tr>
		<td>
			<kbd>isEmpty()</kbd>
		</td>
		<td>
			Return a Boolean indicating if the stack is empty
		</td>
	</tr>
	<tr>
		<td>
			<kbd>top()</kbd>
		</td>
		<td>
			Return the top element in the stack, without removing it; an error occurs if the stack is empty
		</td>
	</tr>
</table>

#Exception Classes

{% highlight java %}
/**
 * Runtime exception thrown when one tries to perform operation
 * top or pop on an empty stack
 */
public class EmptyStackException extends RuntimeException
{
    public EmptyStackException(String e)
    {
        super(e);
    }
}
{% endhighlight %}

{% highlight java %}
/**
 * Runtime exception thrown when one tries to perform operation
 * push on a full stack
 */
public class FullStackException extends RuntimeException
{
    public FullStackException(String e)
    {
        super(e);
    }
}
{% endhighlight %}

#Interface

{% highlight java %}
public interface Stack<T>
{
    /**
     * Return the number of elements in the stack.
     */
    public int size();
 
    /**
     * Return true if the stack is empty, false otherwise.
     */
    public boolean isEmpty();
 
    /**
     * Return element at the top of the stack.
     * Throws EmptyStackException if the stack is empty.
     */
    public T top()
        throws EmptyStackException;
 
    /**
     * Insert an element at the top of the stack.
     */
    public void push(T element);
 
    /**
     * Remove the top element from the stack.
     * Throws EmptyStackException if the stack is empty.
     */
    public T pop()
        throws EmptyStackException;
}
{% endhighlight %}

#Implementation

Eine simple Array-Based Implementation:

{% highlight java %}
public class ArrayStack<T> implements Stack<T> {
 
    protected int capacity; // The actual capacity of the stack array
    public static final int CAPACITY = 1000;    // Default array capacity
    protected T S[];    // Generic array to implement the stack
    protected int top = -1; // Index for the top of the stack
 
    public ArrayStack(int cap)
    {
        capacity = cap;
        S = (T[]) new Object[capacity];
    }
 
    @Override
    public int size()
    {
        return (top + 1);
    }
 
    @Override
    public boolean isEmpty()
    {
        return (top < 0);
    }
 
    @Override
    public T top() throws EmptyStackException
    {
        if(isEmpty())
            throw new EmptyStackException("Stack is empty.");
        return S[top];
    }
 
    @Override
    public void push(T element) throws FullStackException
    {
        if(size() == capacity)
            throw new FullStackException("Stack is full.");
        S[++top] = element;
    }
 
    @Override
    public T pop() throws EmptyStackException {
        T element;
        if(isEmpty())
            throw new EmptyStackException("Stack is empty.");
        element = S[top];
        S[top--] = null; // Dereferences S[top] for garbage collector
        return element;
    }
}
{% endhighlight %}