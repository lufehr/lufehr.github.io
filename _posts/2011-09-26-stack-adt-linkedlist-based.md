---
layout: post
title:  Stack ADT (LinkedList Based)
date:   2011-09-26 14:00:00
tags:   ADT, Stack, LinkedList, Java
assets: 
header: 3
comments: true
---

Im Post Stack ADT (Array-Based) Java wurde bereits ein auf einem Array basiertem Stack implementiert. Nun dasselbe mit einer Generic Linked List als Basis.

Nochmals kurz zur erläuterung, ein Stack bietet folgende Basis-Methoden:

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

#Exception Class

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

#Node Class

Implementiert wird das Ganze mit einem Node:

{% highlight java %}
public class Node<T>
{
    private T element;
    private Node<T> next;
 
    /**
     * Creates a node with null references to its element and next node
     */
    public Node()
    {
        this(null, null);
    }
 
    /**
     * Creates a node with the given element and next node
     */
    public Node(T e, Node<T> n)
    {
        element = e;
        next = n;
    }
 
    /**
     * Accessor methods
     */
    public T getElement()
    {
        return element;
    }
 
    public Node<T> getNext()
    {
        return next;
    }
 
    /**
     * Modifier methods
     */
    public void setElement(T newElem)
    {
        element = newElem;
    }
 
    public void setNext(Node<T> newNext)
    {
        next = newNext;
    }
}
{% endhighlight %}

#Implementation

{% highlight java %}
public class NodeStack<T> implements Stack<T>
{
    protected Node<T> top;    // reference to the head node
    protected int size;      // number of elements in stack
 
    public NodeStack()
    {
        top = null;
        size = 0;
    }
 
    @Override
    public int size() {
        return size;
    }
 
    @Override
    public boolean isEmpty() {
        if(top == null)
            return true;
        return false;
    }
 
    @Override
    public T top() throws EmptyStackException {
        if(isEmpty())
            throw new EmptyStackException("Stack is empty.");
        return top.getElement();
    }
 
    @Override
    public void push(T element) {
        Node<T> v = new Node<T>(element, top);
        top = v;
        size++;
    }
 
    @Override
    public T pop() throws EmptyStackException {
        if(isEmpty())
            throw new EmptyStackException("Stack is empty.");
        T temp = top.getElement();
        top = top.getNext();
        size--;
        return temp;
    }
}
{% endhighlight %}