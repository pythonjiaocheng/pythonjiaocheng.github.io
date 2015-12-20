---
layout: post
title: "how to make class iterable?.md"
category: python
duoshuo: true
mydate : 2015-12-21-02-02-16
description: "how to make class iterable?.md"
tags: [python]
---
`Iterator` objects in python conform to the iterator protocol, which basically means they provide two methods: `__iter__()` and `next()`. The `__iter__` returns the iterator object and is implicitly called at the start of loops. The `next()` method returns the next value and is implicitly called at each loop increment. `next()` raises a `StopIteration` exception when there are no more value to return, which is implicitly captured by looping constructs to stop iterating.    
Here's a simple example of a counter:    

	class Counter:
	    def __init__(self, low, high):
		self.current = low
		self.high = high

	    def __iter__(self):
		return self

	    def next(self):
		if self.current > self.high:
		    raise StopIteration
		else:
		    self.current += 1
		    return self.current - 1

	for c in Counter(3, 8):
	    print c


This will print:    

	[test@Master python]$ python counter.py
	3
	4
	5
	6
	7
	8

python3的版本:   

	class Counter:
	    def __init__(self, low, high):
		self.current = low
		self.high = high

	    def __iter__(self):
		return self

	    def __next__(self):
		if self.current > self.high:
		    raise StopIteration
		else:
		    self.current += 1
		    return self.current - 1

	for c in Counter(3, 8):
	    print(c)

运行结果:   

	[test@Master python]$ python3 counter.py
	3
	4
	5
	6
	7
	8

上面这个类虽然已经可以遍历了，但是却不能重复遍历  


	class Counter:
	    def __init__(self, low, high):
		self.low = low
		self.high = high

	    def __iter__(self):
		self.current = self.low
		return self

	    def __next__(self):
		if self.current > self.high:
		    raise StopIteration
		else:
		    self.current += 1
		    return self.current - 1

	counter = Counter(3, 8)

	for c in counter:
	    print(c)

	print('==========')

	for c in counter:
	    print(c)



This is easier to write using a generator:            

def counter(low, high):
    current = low
    while current <= high:
        yield current
        current += 1

for c in counter(3, 8):
    print c

The printed output will be the same. Under the hood, the generator object supports the iterator protocol and does something roughly similar to the class Counter.

David Mertz's article, Iterators and Simple Generators, is a pretty good introduction.
shareimprove this answer
