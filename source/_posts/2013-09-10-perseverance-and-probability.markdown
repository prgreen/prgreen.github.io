---
layout: post
title: "Perseverance and probability"
date: 2013-09-10 14:47
comments: true
categories: Math
---

**Problem: Trying n times something that has a 1 in n chance of success.**

For example, n = 2 means having 2 tries to guess the result of a coin flip.

For n = 6, you have 6 tries to guess the result of rolling a standard die.

The probability of success P can also be expressed as "one minus the probability of every try failing".
$$
P(n) = 1 - \Big(1-\frac{1}{n}\Big)^n
$$

A surprisingly high number of people have a very flawed understanding of probabilities, even with mathematical backgrounds, and will just assume without thinking that P is always 1 or converges towards 1 as n goes to infinity. None of these propositions are true. 

## What is the limit as n goes to infinity?

If you try sufficiently hard something that has a very low chance of success, you will always succeed (your probability of success converges towards 1), so most people would be tempted to say that P converges towards 1. But that's not the question.

The question is: if you try _exactly_ n times something that has a 1 in n chance of success, and n grows very big, how good are your chances? Let's call P the probability of success.


$$
lim_{n\to\infty}P(n) = 1 - lim_{n\to\infty}\Big(1-\frac{1}{n}\Big)^n
$$

Numerically if you tried a few values of n you would realize that the right expression converges quickly towards a number that is neither 0 nor 1.

Remembering one of the definitions of **e**, the Euler number:

$$
e = lim_{n\to\infty}\Big(1+\frac{1}{n}\Big)^n
$$

it becomes easy to realize that

$$
lim_{n\to\infty}P(n) = 1 - e ^{-1} \approx 0.63212
$$

A *63% chance of success* means you're a winner in the long term if you're used to *trying a million times* something that has a *one in a million chance* of success.

## Wacky logic: "Les Shadoks" 

{% img center /images/shadok.jpg %}

> "If there is one in a million chance to succeed, rush failing the 999,999 first tries."

During my childhood in France in the 90s, an old animated TV series from the late 60s called "Les Shadoks" was still running, that had been a significant cultural phenomenon among older generations. I remember it being very funny, and you should definitely check it out on Youtube if you understand French, and are prepared to feel overwhelmed with utter confusion and culture shock. Some of the ridiculously wrong and illogical Shadok mottos are still heard today in France, although since then end of the show less and less people know where they come from ("Pourquoi faire simple quand on peut faire compliqué ?" => "Why do it the easy way when you can do it the hard way?").

{% img center /images/shadok2.jpg %}

> "Better to pump even if nothing happens than to risk something worse happening by not pumping"

It had the most bizarre, absurd and eerie stories, and an unhealthy obsession with useless and endless pumping, but more importantly for our current discussion, the Shadoks, stupid and ruthless bird-like creatures, keep dying in atrocious circumstances because of how flawed their "logic" is.

One example that particularly struck me as a child: the Shadoks want to build a rocket to travel from their dysfunctional home planet to the Earth. Their most renowned mathematician calculates that their plan has a one in a million chance of success, then claims that all they have to do is rebuild the same rocket over and over again until it inevitably works when they reach 1 million attempts.

> "When one tries continuously, one ends up succeeding. Thus, the more one fails, the greater the chance that it will work."

Even 7 year-old me knew it couldn't be right (and anyone who understands basic probabilities is warned specifically against this fallacy), but it took me years to figure out the real probability of success.

Approximately 63% is actually a pretty good chance for the Shadoks, given their general incompetence.

{% img center /images/shadok3.jpeg %}

> The Shadok brain can only memorize 4 words, so they count using a base 4 number system. They were my first introduction to non-decimal bases, at age 7, or BU MEU. 

{% img center /images/shadok4.gif %}

Here's a link to a video explaining Shadok counting (in French): [Shadok counting](http://www.youtube.com/watch?v=nm0cw6b1PMA)


## Applied probabilities: picking up women (or men)

What are the odds that a girl you meet randomly in public agrees immediately to date you? The chances are pretty slim (although you'd be surprised how much blunt honesty works), especially now that I know you'd rather spend your free time reading through my ramblings instead of going outside making friends or something.

But the point is: the chances are small, but non-negligible, let's say 1%. Ask every woman you meet during the day, in public transportation, on the streets, and more often than not (63% if you ask 100 girls out), you will end up dating someone today with little effort (probably someone really easy or mentally unstable, but that's your problem now).

No need for sophisticated matching algorithms, loneliness in our contemporary societies is easily solved by brute force[^1]!

[^1]: I am not liable for tragic misunderstandings of this sentence.




