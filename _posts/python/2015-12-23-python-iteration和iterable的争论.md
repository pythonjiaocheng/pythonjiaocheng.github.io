---
layout: post
title: "python iteration和iterable的争论"
category: python
duoshuo: true
mydate : 2015-12-23-23-38-41
description: "python iteration和iterable的争论"
tags: [python]
---
In natural language,   

> iteration is the process of taking one element at a time in a row of elements.   

In Python,   

> iterable is an object that is, well, iterable, which simply put, means that it can be used in iteration, e.g. with a `for` loop. How? By using `iterator`.     

... while iterator is an object that defines how to actually do the iteration--specifically what is the next element. That's why it must have next() method. Iterator is not iterable, though.

So what does Python interpreter think when it sees for x in obj: statement?

    Look, a for loop. Looks like a job for an iterator... Let's get one. ... There's this obj guy, so let's ask him.

    "Mr. obj, do you have your iterator?" (... calls iter(obj), which calls obj.__iter__(), which happily hands out a shiny new iterator _i.)

    OK, that was easy... Let's start iterating then. (x = _i.next() ... x = _i.next()...)

Since Mr. obj succeeded in this test (by having certain method returning a valid iterator), we reward him with adjective: you can now call him "iterable Mr. obj".

However, in simple cases, you don't normally benefit from having iterator and iterable separately. So you define only one object, which is also its own iterator. (Python does not really care that _i handed out by obj wasn't all that shiny, but just the obj itself.)

This is why in most examples I've seen (and what had been confusing me over and over), you can see:

class IterableExample(object):

    def __iter__(self):
        return self

    def next(self):
        pass

instead of

class Iterator(object):
    def next(self):
        pass

class Iterable(object):
    def __iter__(self):
        return Iterator()

There are cases, though, when you can benefit from having iterator separated from the iterable, such as when you want to have one row of items, but more "cursors". For example when you want to work with "current" and "forthcoming" elements, you can have separate iterators for both. Or multiple threads pulling from a huge list: each can have its own iterator to traverse over all items. See @Raymond's and @glglgl's answers above.

Imagine what you could do:

class SmartIterableExample(object):

    def create_iterator(self):
        # An amazingly powerful yet simple way to create arbitrary
        # iterator, utilizing object state (or not, if you are fan
        # of functional), magic and nuclear waste--no kittens hurt.
        pass    # don't forget to add the next() method

    def __iter__(self):
        return self.create_iterator()

Notes:

    I'll repeat again: iterator is not iterable. Iterator cannot be used as a "source" in for loop. What for loop primarily needs is __iter__() (that returns something with next()).

    Of course, for is not the only iteration loop, so above applies to some other constructs as well (while...).

    Iterator's next() can throw StopIteration to stop iteration. Does not have to, though, it can iterate forever or use other means.

    In the above "thought process", _i does not really exist. I've made up that name.

    There's a small change in Python 3.x: next() method (not the built-in) now must be called __next__(). Yes, it should have been like that all along.

    You can also think of it like this: iterable has the data, iterator pulls the next item

Disclaimer: I'm not a developer of any Python interpreter, so I don't really know what the interpreter "thinks". The musings above are solely demonstration of how I understand the topic from other explanations, experiments and real-life experience of a Python newbie.
shareimprove this answer
	
edited Jun 19 at 9:21
Rick-777
3,1401330
	
answered Sep 11 '13 at 14:59
Alois Mahdal
2,89922349
	
11 	 
	
I think all iterators are themselves iterable. When the built-in function iteris called on an iterable, it adds both __iter__ and next methods on the object that it returns. Can you give an example of an object which is an iterator but not iterable? – ajay Oct 24 '13 at 7:18
6 	 
	
I like this answer for its conversational tone, clear explanations and the examples. One correction: an iterator is an iterable. From the PEP on iterators: "a class that wants to be an iterator should implement two methods: a next() method...and an __iter__() method that returns self." An iterator is iterable because it return a valid iterator when iter() is called on it - in this case, the iterator returns itself, and we know it is a valid iterator because it defines the next() method. – crennie Jan 10 '14 at 19:33
   	 
	
@crennie yes, as ajay pointed out, I realize that my answer is incorrect at that point. I tried to fix it but could not come up with something without me breaking it so I decided to let it be and vote up ajay's comment hoping that it will be come one of that 2-digit comments that everyone will read anyway :) – Alois Mahdal Jan 11 '14 at 0:26
1 	 
	
@udarH3 You do define the iterator code next(), or __next__() but the actual iterator is an object that will be created each time something wants to iterate over the object. That way you can have eg. two passes over the same list at the same time without interfering with each other; each will have new iterator intitially set to first item, then returning it and moving one item further, etc... (In fact, those items may not really exist in the object; they may be fetched or created on the go.) – Alois Mahdal Oct 25 at 0:45
1 	 
	
@udarH3 Actually you can try it with any simple list: lst = ['a', 'b', 'c']; we want to iterate? let's get an iterator: ir1 = iter(lst)... first item: next(ir1) will be 'a', calling next(ir1) gives 'b' etc. This is what python does behind the scenes with each for. At any point, though, you can always get a new iterator: ir2 = iter(lst) that will start from scratch: next(ir2) gives you 'a' again when next(ir1) already gives 'c'. – Alois Mahdal Oct 25 at 0:55 
