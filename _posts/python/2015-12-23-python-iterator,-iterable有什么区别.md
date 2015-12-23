---
layout: post
title: "Python iterator, iterable有什么区别"
category: python
duoshuo: true
mydate : 2015-12-23-23-08-03
description: "Python iterator, iterable有什么区别"
tags: [python]
---
An ITERABLE is:   

- anything that can be looped over (i.e. you can loop over a string or file)    
- anything that can appear on the right-side of a for-loop:    

	for x in iterable: ...   
    
- anything you can call with `iter()` have it return an ITERATOR:     

	iter(obj)

- an object that defines `__iter__` that returns a fresh ITERATOR, or it may have a `__getitem__` method suitable for indexed lookup.     


An ITERATOR is:   

- an object with state that remembers where it is during iteration   
- an object with a `__next__` method (Python 3; `next` before) that:   
    - returns the next value in the iteration   
     - updates the state to point at the next value    
     - signals when it is done by raising `StopIteration`   
- an object that is self-iterable (meaning that it has an `__iter__` method that returns self).     


The builtin function next calls the __next__ (Python 3) or next (Python 2) method on the object passed to it.

For example:

>>> s = 'cat'      # s is an ITERABLE
                   # s is a str object that is immutable
                   # s has no state
                   # s has a __getitem__() method 

>>> t = iter(s)    # t is an ITERATOR
                   # t has state (it starts by pointing at the "c"
                   # t has a next() method and an __iter__() method

>>> next(t)        # the next() function returns the next value and advances the state
'c'
>>> next(t)        # the next() function returns the next value and advances
'a'
>>> next(t)        # the next() function returns the next value and advances
't'
>>> next(t)        # next() raises StopIteration to signal that iteration is complete
Traceback (most recent call last):
...
StopIteration

>>> iter(t) is t   # the iterator is self-iterable

shareimprove this answer
