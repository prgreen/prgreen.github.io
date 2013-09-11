---
layout: post
title: "True hackers need math"
date: 2013-09-11 04:26
comments: true
categories: 
---

# (and an intuitive comprehension of what's happening in their code)

More and more software developers in the start-up world are the product of necessity. While their technical knowledge of programming is usually good, their understanding of mathematics is sometimes really disappointing, and is a real handicap in their job.

Now, don't get me wrong, I'm not *particularly* good at math, and I'm an engineer so I should feel bad about it, but when I can constantly notice the flaws in my colleagues' or friends' work or reasoning, that makes it all the more alarming.

Math is indisputably at the heart of programming, the same way it's the heart of physics, or engineering, or anything that allows us to understand or manipulate the evolution of reality in quantifiable ways.

From what I've seen, "genius hacker college dropout kids" seem to be the most affected by the lack of mathematical knowledge. To them programming is mastering the specificities of a technology and use it to create cool stuff fast, or as a means to disrupt society's power balance in their favor. Since it's a valuable skill after all, they obtain sufficient gratification from their coding skills alone, and they never get around to learning real math to help them progress past a certain critical point, which is a handicap in all but the most trivial problems a programmer has to face. So they end up plateauing pretty hard despite a promising start, and prefer abandoning coding altogether and just doing business. Only the more "mathematical" geniuses can continue enjoying programming for years in a somewhat autotelic fashion (I'm thinking John Carmack and his recent tech-heavy speech at QuakeCon 2013). 

## A few tales of mathematical ignorance

I promise those people are actually brilliant professional coders otherwise, but not understanding the math behind what they do is definitely a source of problems.

-----

**Example 1**: Implementing the [Glicko](http://en.wikipedia.org/wiki/Glicko_rating_system) rating system (to assess players' strength in an online game)

"Can you implement this formula?"

=> "Wait, what's a logarithm?"

(and then not understanding the difference between natural logarithm of ten in base e, and logarithm in base ten)

--------

**Example 2**: 

"If your background image is too heavy for our website, you can try drawing more uniformly colored areas instead of varying the shades everywhere."

=> "I don't get it, if the image size doesn't change, it has the same number of pixels so the download size remains the same for the users."

No. Dude. No. Not even close. Images displayed on a website are in a compressed format! Try comparing a big image with white only (compression works perfectly, 1kB) and a an image with the same size and every single pixel set to a random value (compression becomes useless, your image now weighs a few MB...).

Information entropy, fast fourier transform, are complicated but useful notions.

------

**Example 3**:

Not a dialog, but I see a lot of programmers who like to obsess over time and space complexities (using a fragile understanding of big O notations), saying "I'll just use quicksort because it has the best time complexity and I want it to be fast".

Actually when sorting an input they should consider all the aspects: 

- in some cases the size of the input will remain low enough that more "complex" but simpler to understand algorithms with more favourable multiplicative constants are faster.

- if the size of the input is high but there are very few possible values, bucket sort beats quicksort.

- circumstances can justify using other algorithms, for example if the input is received by chunks over the network and needs to be constantly sorted, heap sort might be more adapted. 

## "Google coding"

Applying formulas or following recipes is no substitute for an intuitive understanding of what's happening inside your code.

The general spirit of not wanting to think or do math but just "have fun" or "make something awesome" results in an annoying trend: the so-called "Google coders", people who will use Google extensively to solve their problems, copy-paste and adapt some code without fully understanding what they're doing, not really checking the internals, or wondering why it should work.

It's OK to do that sometimes, and tapping into the internet hivemind might feel like a decent way to increase your productivity instantly. But mostly it's just laziness.

There are indeed a few glaring flaws with this approach:

- You end up trying to connect black boxes of code together, with potential impedance mismatch or lack of general consistency in your project.

- How do you test code you don't understand? How do you handle memory, errors, exceptions, tests, that were not in the original code because the author wanted to keep things simple?

- You don't learn anything that you can reapply in the future in similar circumstances using principles extracted from your intuitive comprehension of the problem.

- If you don't understand exactly what you need in your specific problem, your solution won't exactly answer your problem.

- No progress or breakthrough in technology was ever made by people who sacrifice for comfort the need to think. 


Many programmers have the right idea when they try to roll their own piece of software (like, coding your blog platform or yet another content management system), and only then use all the available tools and libraries made by others and refined by years of use, understanding the intent behind every design decision and API call.

One immense advantage is that, when something breaks, you already know or imagine how the internals are made, and you can easily go fix it yourself (instead of waiting for the maintainer to manifest himself).  

I would only use "google coding" in emergency situations, for fun coding marathons, where I would be convinced the code quality and maintainability matters less than getting something out in record time.

## Worse than "Google coders": "Microsoft black magic coders"

I'm not talking about people who work for Microsoft itself, who, I'm sure, are mostly brilliant people with a true passion for programming, for which I have a lot of respect.

I'm talking about people who believe they can get all the complicated parts of programming out of programming.

I talk to some of my friends who work and code for very big companies and are forced to use Microsoft products everywhere. 

Microsoft products are great and make everything easier, except when they break, and they break all the time. I'm not blaming Microsoft particularly, everything breaks all the time, it's called entropy, and it's an inevitable part of the physical universe. Everyone can realize that programming is inherently hard, and it's inevitable that sometimes, people will use your program in unforeseeable ways.

What I could blame Microsoft for, however, is that trying to hide all the implementation details and making programming simpler for a generation of subpar brainwashed programmers makes it harder to fix the inevitable problems one will encounter.

When something breaks in a fully open source stack, it's relatively easy to identify which part, where, to correct the problem, to submit a fix to the maintainers, etc. It's a very good feeling for a programmer, and it trains a generation of people who understand what happens at every level in their code, which is very sound and satisfying.

Microsoft deprives people from this feeling, by making the disputable (but economically justified) strategy choice to make everything "their way", close-sourced and with voluntary divergence from internationally accepted norms or open interoperable formats.

Every programmer who uses Microsoft products is treated like a child, maintained in a state of voluntary ignorance, brainwashed into accepting a mix of archaic constructs and new idiomatically named features, given comparatively lower powers for fear that they could break things, and when (inevitably) something does break, no one has any idea what's actually happening, and they are forced to call "experts" to unblock the situation.

I like to call them "black magic coders" because they see programming as summoning the powers of the dark side and pray that they consent to help. If Microsoft doesn't provide any specific magical way to do what they want, they are distraught and helpless because they have no idea how to reimplement the basics by themselves.

I've talked to a software engineer with years of experience working with .Net and C#, who had no idea whether the huge web app they were making used "regular" HTTP calls or Ajax. Everything had always been abstracted away to him, to the point he had trouble making the connection between Microsoft mumbo jumbo and the actual underlying technologies used. 

They memorize acronyms, standardized solutions, common answers good enough to pass their Microsoft certificate, and have a Pavlovian relationship to programming. 

It's fine to abstract away the implementation details, but only if you know what's behind them in case you need to know, which happens often with those hard-to-find bugs where you hit a limitation or implementation quirk of the underlying technology.

