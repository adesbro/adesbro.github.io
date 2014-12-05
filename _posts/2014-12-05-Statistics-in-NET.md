---
layout: post
title: Statistics in .NET
---

My development environment of choice is Visual Studio and C#. I have a lot of experience with it and I *feel* more productive than other environments. I have used Java (with Eclipse) in the past and some elements of my work require me to use Python and R but I always come back to old familiar.

[![Normal Distribution (CDF)](http://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Normal_Distribution_CDF.svg/512px-Normal_Distribution_CDF.svg.png)](http://commons.wikimedia.org/wiki/File%3ANormal_Distribution_CDF.svg "By Inductiveload (self-made, Mathematica, Inkscape) [Public domain], via Wikimedia Commons")

My field of research requires an element of statistical calculation so I've recently been working on some prototypes that require the use of frequency tables, cumulative distribution and pseudo-random sampling. So, my first thought was, "there must be heaps of libraries out there for this, lets have a look on GitHub." Initially I found nothing written in C# (or any .NET language), but plenty for R, Java, C and Python. A bit of googling yielded some libraries out there, but they seemed large and cumbersome and I wanted something simple for some testing. Some required payment. 

My experience as a developer on commercial products was telling me that I should find some candidate libraries, perform a quick evaluation on them, make a decision on which to use and move on. Then I thought, "Hang on, the maths here is not just a means to an end. It is important and I need to really understand it."

> What better way to understand something as a developer than by writing an implementation from scratch?

So I created a [repository on GitHub called MathStat](https://github.com/adesbro/MathStat) that would be a home for these statistical classes. I learned a lot creating these and it was an enjoyable experience (I know, I need to get out more). It is one of the things that enticed me to my new role in research; the ability to spend more time learning and exploring (within reason, of course).

It was a bit later that I discovered the [Math.NET project](http://www.mathdotnet.com/). 

> Math.NET is an opensource initiative to build and maintain toolkits covering fundamental mathematics, targetting advanced but also every day needs of .Net developers.

I don't know why I didn't stumble across this before, but Math.NET appears to have everything I need. Plus, it is all available as a series of [NuGet Packages](https://www.nuget.org/profiles/mathnet/) and is actively developed. Win.

**Bottom line:** If you want a library to do maths and statistics in .NET, seriously consider something like [Math.NET](http://www.mathdotnet.com/). However, the process of trying something for yourself is an invaluable learning experience.

