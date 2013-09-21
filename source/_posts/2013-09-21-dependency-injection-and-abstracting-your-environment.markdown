---
layout: post
title: "Dependency injection and abstracting your environment"
date: 2013-09-21 11:37
comments: true
categories: Tech 
---

[Dependency injection](http://en.wikipedia.org/wiki/Dependency_injection) is one of the most useful software design pattern.

Although explanations on the subject can become long-winded, the core idea behind dependency injection is one of the most simple and natural, not just in programming, but also in life.

To me, it's all about abstracting the environment.

## Recognizing patterns in your environment makes you more adaptable

Let's say that, pining for a change of air, you decide to move to China (or anywhere that is the most exotic place you can imagine).

Obviously, there's a lot of things that will confuse you. But you will still be able to survive. How so?

By going upwards in the levels of abstraction. You can't understand anything particuliar, but you still understand the general things, and that's really good enough for you. You don't know the words, but you know people around you speak a language, you can realize someone is talking to you, act in consequence, use body language. You don't know what the food is made of, or what kind it is, but you can be pretty sure it's food.

You don't reason in terms of familiar, accurate, specific words anymore, but in terms of general sensations and concepts.


## Abstraction is not a vague mathematical concept, it's a survival tool

What is abstraction anyway, at a fundamental level, how do we understand it, as human beings? It's recognizing similar things or processes, identifying generic patterns, that unite things that are superficially dissimilar, but can be treated in the same way for certain applications.

For example, you can eat chinese food (specific) or any sort of food, once you recognize it is indeed food (the more abstract concept).

Abstraction is a very powerful concept, probably the most useful in programming, where all you do all day is choose the right level of abstraction and implement all the specific layers from there.

It's not that obvious, animals are said to be incapable of abstract reasoning. And even with a human brain it's something you can't really form abstract thoughts until you're around 11 years old.

Animals are too strongly tied to their environment, it never changes that much, and when it does, the species just goes extinct. It's the equivalent of a code that simply hardcodes environment values instead of reading them from the environment, and breaks when the environment changes.

## In programming, the environment should be abstracted too

To make your code more resilient to all the changes occurring around it (especially when your code is part of a library that will be used in various kinds of projects), your classes should depend on interfaces (how things around the class *generally* are or behave) rather than one specific class.

If you want your code to be as adaptable as humanity, it means to make the least possible amount of assumptions on what is immediately available to it.

Because if you don't, and your code travels to China, it won't recognize the local food and will just die an evitable death. Get it?

## What about dependency injection, then?

Animals often require their environment to be finely tuned to them, while humans only require their environment to conform to generic expectations (water, food, stuff to build things from).

And it's easier to satisfy generic expectations, not to mention more things will satisfy them, so your piece of code will be useful in more situations than you previously thought was possible, the same way humans can survive in nuclear submarines, in the desert or on the arctic circle.

Dependency injection means that, instead of your class requiring something particular that you need to actively seek out immediately (at compile time), you require something more abstract and you don't have it yet (because it's abstract, it's not "a thing", it's a concept). But the environment can then inject the dependency, the "real" object, at runtime.

Without dependency injection, your code NEEDS the environment to be perfectly accomodating of its own needs or it will break. With dependency injection, the environment can try to throw anything at your code, only when it needs to, as long as it's within some accepted rules, it will work.

Understanding dependency injection is one of the most spectacular ways to improve the quality and reusability of your code.




