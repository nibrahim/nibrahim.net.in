--- 
layout: post
title: Approximations
wordpress_id: 79
wordpress_url: http://nibrahim.net.in/journal/?p=79
---
While reading Jon Bentley's excellent book <a href="http://www.cs.bell-labs.com/cm/cs/pearls/">Programming pearls</a>, I came across something that I had read back at college. The idea is to cultivate an ability to make reasonably accurate approximations for various large numbers. It's an interesting exercise. It's fun to try to "guess" the number of cars in a parking lot, the number of windows on one side of a building etc.
People are quite good are making guesses of a small number of things. If you see a pile of coins, you can usually say how many are there withing a small margin of error. As the numbers grow larger, it's harder to say though. Suppose you were to guess the number of people in Bangalore (say), you'd find it harder. It's definitely more than 10, more than 100, more than 1000, more than 10000, more than 1,00,000, more than 10,00,000. How about 10,00,00,00,000. Don't be silly. It's definitely lesser than that. So we have an upper and lower limit. What do you think the <a href="http://www.google.co.in/search?q=Population+of+Bangalore&ie=utf-8&oe=utf-8&aq=t&rls=com.ubuntu:en-US:official&client=firefox-a">answer</a> is? It's interesting to try this out for various things. What's the distance from Bangalore to Chennai? What's the circumference of the earth? These are quite easily verifiable.

I think however that being able to make estimates like this is dependant on what you're looking at. Most of the examples above are numbers and lengths. Weights is different. How much does a maruti 800 car weigh? A ton? Half a ton? Quarter a ton? Check your <a href="http://www.maruti800.com/specifications.asp">answer</a>.

Why is this useful? Jon Bentley introduces the topic when he discusses making back of the envelope calculations of memory usage and other program parameters.  This is under appreciated these days but it's a useful skill I think. The physicist Enrico Fermi was famous for this sort of thing and problems designed to teach people to make quick approximations are called Fermi problems. If you're interested in some such problems, check <a href="http://www.physics.umd.edu/perg/fermi/fermi.htm">this</a> and <a href="http://iws.ccccd.edu/mbrooks/demos/fermi_questions.htm">this</a>.

If you're a regular blogger, try this "how many posts does my blog have?" :)
