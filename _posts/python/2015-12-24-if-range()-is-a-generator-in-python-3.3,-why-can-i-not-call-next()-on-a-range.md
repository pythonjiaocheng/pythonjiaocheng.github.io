---
layout: post
title: "If range() is a generator in Python 3.3, why can I not call next() on a range"
category: python
duoshuo: true
mydate : 2015-12-24-00-41-47
description: "If range() is a generator in Python 3.3, why can I not call next() on a range"
tags: [python]
---

Iterables are objects that `iter` can be used on to obtain an iterator. Iterators are objects that can be iterated through using `next`. Generators is a category of iterators (generator functions and generator expressions).    

`range` is a class of immutable iterable objects. Their iteration behavior can be compared to lists: you can't call `next` directly on them; you have to get an iterator by using `iter`.                        
So no, `range` is not a generator.    
You may be thinking, "why didn't they make it directly iterable"? Well, `range`s have some useful properties that wouldn't be possible that way:             

- They are immutable, so they can be used as dictionary keys.   
- They have the `start`, `stop` and `step` attributes (since Python 3.3), `count` and `index` methods and they support `in`, `len` and `__getitem__` operations.    
- You can iterate over the same range multiple times.   

	>>> myrange = range(1, 21, 2)
	>>> myrange.start
	1
	>>> myrange.step
	2
	>>> myrange.index(17)
	8
	>>> myrange.index(18)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	ValueError: 18 is not in range
	>>> it = iter(myrange)
	>>> it
	<range_iterator object at 0x7f504a9be960>
	>>> next(it)
	1
	>>> next(it)
	3
	>>> next(it)
	5

Another nice feature of `range` objects is that they have a `__contains__` method which can be used to test whether a value is in a range: `5 in range(10) => True`    
