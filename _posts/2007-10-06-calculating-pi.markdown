--- 
layout: post
title: Calculating PI
wordpress_id: 100
wordpress_url: http://nibrahim.net.in/journal/?p=100
---
Here's a simple statistical way to calculate PI using an iterative procedure. 
<br />
Imagine a dartboard with the following figure on it. A square of side 2 inscribed by a circle of radius 1. The area of the square is 4 and that of the circle PI. If we were to throw random darts at this dartboard, the ratio of number of darts inside the circle to the number of darts inside the square would start approximating the ratio of their areas which is PI/4. ie.
<br />
( no. of darts inside circle / no. of darts inside square ) = (area of circle / area of square) = (PI/4)
<br />
So, the value of PI would be 4 times the ratio of the number of darts inside the circle to the number of darts inside the square.
<br />
I wrote a short program to try this out. 
<pre>
#!/usr/bin/python

import math
import random

noc = nc = 0.0

for i in range(1,5000001):
    x = random.random()*2 - 1
    y = random.random()*2 - 1
    distance = math.sqrt(x**2 + y**2)
    if distance > 1:
        noc += 1
    else:
        nc += 1
    if i%500000 == 0:
        print "%d  %f"%(i,(4 * nc/(noc+nc)))
</pre>
It throws 5x10<sup>6</sup> darts and prints the result every 5x10<sup>5</sup> iterations (10 times). The result of a typical run is like this
<pre>
500000  3.138040
1000000  3.139720
1500000  3.140701
2000000  3.141336
2500000  3.141488
3000000  3.141023
3500000  3.140557
4000000  3.141085
4500000  3.141009
5000000  3.141120
</pre>
