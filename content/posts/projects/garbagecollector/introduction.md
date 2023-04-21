---
title: Introduction
summary: What is a garbage collector anyway?
draft: false
showToc: true
tocopen: true
tags: ["garbage collector", "C", "low level", "operating system"]
date: "2023-04-12"
weight: 1
---
Garbage collection is a programming concept that you might have heard about. It's important to know about because it can help you write better code by removing some of the burden of memory management from your shoulders.

> Garbage collection is the process by which a program automatically reclaims unused memory and frees up resources for other programs to use.

When you write code in any language, **there are two types of data: live and dead (or "garbage")**:

- **Live data** refers to anything being **used by the program** at any given time; for example, if an object has been created but hasn't been destroyed yet then that object would be considered live.

- **Dead objects** are those **no longer being used** by any part of your program; these include variables whose scope has ended or references that point nowhere anymore (like null pointers).

Garbage collection works by scanning through all these objects periodically looking for ones that aren't referenced anywhere else anymore; when one is found then it gets deleted so its resources can be freed up again for reuse later on down the line. This tecnique is called **mark and sweep** and that's what we are going to use later.

## Benefits of Garbage Collection

Garbage collection is a useful feature of many programming languages. It allows the programmer to **focus on what they want to do, rather than having to worry about memory management**.

**Garbage collection is an alternative to manual memory management** (for example, using `malloc()` and `free()` in C/C++). This means that if you write some code that allocates some memory and then calls another function with that same piece of data in it, **there's no need for you as a programmer to clean up after yourself**: the language will do it automatically! This **reduces the likelihood of memory leaks** occurring because there's no longer any possibility for dangling pointers or other bugs related to incorrect freeing of allocated resources. This makes programs **safer and easier to write** correctly in general (and thus more reliable).

## Shortcomings of Garbage Collection

Garbage collection is not without its **drawbacks**. For one thing, it can introduce a certain amount of **overhead into your program**. The collector has to run periodically to identify objects that are no longer being used and reclaim their memory for use by other objects. This means that **if you have lots of short-lived objects, this process may be more expensive than simply freeing up those resources manually** (and then reusing them later).
Secondly, **garbage collection reduces your control over memory allocation**; you don't know exactly where your data will end up being stored in the heap or stack because it depends on how much space is available at any given time. In contrast with **manual allocation which allows us complete freedom** over where our variables are stored in memory!

## Real-World Applications

**Garbage collection is a big part of many popular applications today**. In fact, you may be using garbage collected languages right now without even knowing it!
**Java and C# are both garbage-collected languages** that are used for **web and app development**. This means that when you visit a website built with one of these languages or use an app on your phone you are using a garbage collector!
