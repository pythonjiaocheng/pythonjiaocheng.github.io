---
layout: post
title: "Dive Into Python3 Chapter3 Comprehensions"
category: "Dive Into Python3"
duoshuo: true
mydate : 2015-12-20-23-00-56
description: "Dive Into Python3 Chapter3 Comprehensions"
tags: [Dive Into Python3]
---
❝ Our imagination(n. [心理] 想象力；空想；幻想物) is stretched(v. 伸直，伸展；舒展) to the utmost(n. 极限；最大可能), not, as in fiction, to imagine things which are not really there, but just to comprehend(vt. 理解；包含；由…组成) those things which are. ❞    
— Richard Feynman     

##Diving In


Every programming language has that one feature, a complicated thing intentionally(adv. 故意地，有意地) made simple. If you’re coming from another language, you could easily miss it, because your old language didn’t make that thing simple (because it was busy making something else simple instead). This chapter will teach you about list comprehensions, dictionary comprehensions, and set comprehensions: three related concepts centered around one very powerful technique. But first, I want to take a little detour(vt. 使…绕道而行) into two modules that will help you navigate your local file system.          


##Working With Files And Directories   

Python 3 comes with a module called os, which stands for “operating system.” The `os` module contains a plethora(n. 过多；过剩；[医] 多血症) of functions to get information on — and in some cases, to manipulate — local directories, files, processes, and environment variables. Python does its best to offer a unified API across all supported operating systems so your programs can run on any computer with as little platform-specific code as possible.             

###The Current Working Directory   

When you’re just getting started with Python, you’re going to spend a lot of time in the Python Shell. Throughout this book, you will see examples that go like this:    

- Import one of the modules in the `examples` folder                   
- Call a function in that module               
- Explain the result    

If you don’t know about the current working directory, step 1 will probably fail with an `ImportError`. Why? Because Python will look for the `example` module in the import search path, but it won’t find it because the `examples` folder isn’t one of the directories in the search path. To get past this, you can do one of two things:    

- Add the `examples` folder to the import search path   
- Change the current working directory to the `examples` folder    

The current working directory is an invisible property that Python holds in memory at all times. There is always a current working directory, whether you’re in the Python Shell, running your own Python script from the command line, or running a Python CGI script on a web server somewhere.     

The `os` module contains two functions to deal with the current working directory.     



