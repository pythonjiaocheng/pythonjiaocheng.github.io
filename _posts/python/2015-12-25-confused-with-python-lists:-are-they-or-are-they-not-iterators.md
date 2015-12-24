---
layout: post
title: "Confused with python lists: are they or are they not iterators"
category: python
duoshuo: true
mydate : 2015-12-25-02-25-47
description: "Confused with python lists: are they or are they not iterators"
tags: [python]
---
I am studying Alex Marteli's Python in a Nutshell and the book suggests that any object that has a `next()` method is (or at least can be used as) an iterator. It also suggests that most iterators are built by implicit or explicit calls to a method called `iter`.    

After reading this in the book, I felt the urge to try it. I fired up a python 2.7.3 interpreter and did this:    

	>>> x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	>>> for number in range(0, 10):
	...     print x.next()

However the result was this:    

	Traceback (most recent call last):
	  File "<stdin>", line 2, in <module>
	AttributeError: 'list' object has no attribute 'next'

In confusion, I tried to study the structure of the x object via `dir(x)` and I noticed that it had a `__iter__` function object. So I figured out that it can be used as an iterator, so long as it supports that type of interface.                            

So when I tried again, this time slightly differently, attempting to do this:                    

	>>> _temp_iter = next(x)

I got this error:    

	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: list object is not an iterator

But how can a list NOT be an iterator, since it appears to support this interface, and can be certainly used as one in the following context:    

	>>> for number in x:
	...     print x

	

They are iterable, but they are not iterators. They can be passed to `iter()` to get an iterator for them either implicitly (e.g. via for) or explicitly, but they are not iterators in and of themselves.               

Note that all iterators (which are well-behaved) are also iterable -- their `next` simply returns `self`, so you can call `iter(iter(iter(iter(x))))` and get the same thing as `iter(x)`. This is why for works with both iterables and iterators without type sniffing (well, disregarding performance optimizations).                 

	

You need to convert list to an iterator first using `iter()`:                             

	In [7]: x = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

	In [8]: it=iter(x)

	In [9]: for i in range(10):
	    it.next()
	   ....:     
	   ....:     
	Out[10]: 0
	Out[10]: 1
	Out[10]: 2
	Out[10]: 3
	Out[10]: 4
	Out[10]: 5
	Out[10]: 6
	Out[10]: 7
	Out[10]: 8
	Out[10]: 9

	In [12]: 'next' in dir(it)
	Out[12]: True

	In [13]: 'next' in dir(x)
	Out[13]: False

	checking whether an object is iterator or not:

	In [17]: isinstance(x,collections.Iterator)
	Out[17]: False

	In [18]: isinstance(x,collections.Iterable)
	Out[18]: True

	In [19]: isinstance(it,collections.Iterable) 
	Out[19]: True

	In [20]: isinstance(it,collections.Iterator)
	Out[20]: True

		
	answered Oct 24 '12 at 17:03
	Ashwini Chaudhary
	119k13160235
		
	add a comment
	up vote
	9
	down vote
		

Just in case you are confused about what the difference between iterables and iterators is. An iterator is an object representing a stream of data. It implements the iterator protocol:                 

	    __iter__ method
	    next method

Repeated calls to the iterator’s `next()` method return successive items in the stream. When no more data is available the iterator object is exhausted and any further calls to its `next()` method just raise `StopIteration` again.   

On the other side iterable objects implement the `__iter__` method that when called returns an iterator, which allows for multiple passes over their data. Iterable objects are reusable, once exhausted they can be iterated over again. They can be converted to iterators using the `iter` function.   

So if you have a list (iterable) you can do:                                

	>>> l = [1,2,3,4]
	>>> for i in l:
	...     print i,
	1 2 3 4
	>>> for i in l:
	...     print i,
	 1 2 3 4

If you convert your list into an iterator:                      

	>>> il = l.__iter__()  # equivalent to iter(l)
	>>> for i in il:
	...     print i,
	 1 2 3 4
	>>> for i in il:
	...     print i,
	>>> 

