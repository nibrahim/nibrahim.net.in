---
layout: post
title: "Calligraphic rulings"
---

Calligraphy is a hobby of mine and I spend a few hours every week on
it. I've tried arabic but it's too hard to do without precise
instruments and a teacher. English is slightly easier and the results
look somewhat pleasing even if one is not very skilful. 

Calligraphic letter proportions are specified using multiples of the
width of the nib of the pen being used. Following is an example of the
proportions of various parts of a letter in the
"[Foundational hand](http://web.me.com/yukimia/Yukimi_A/Foundational_Hand.html)".

![Foundational hand (from thecalligraphypen.com)] (/images/foundational-hand.JPG) 

(from [http://thecalligraphypen.com](http://thecalligraphypen.com))

As you can see, the body is 4 nibs high and the ascenders and
descenders are 3 nibs high each. 

The only calligraphic pens I've seen in India are the ones from
[Sheaffer](http://www.sheaffer.com/calligraphy/). They're not bad and
usually come as a set of 3 nibs. The fine nib is 1mm wide, the medium
one is 1.5mm wide and the broad one is 2mm wide. Here is a picture of
my broad nibbed pen

![Sheaffer calligraphy pen (broad nib - 2mm)] (/images/calligraphy_pen.jpg) 

One of the things that are necessary to write properly is a
[ruling sheet](http://www.monkeysee.com/play/8106-ruling-the-page-for-calligraphy). These
are similar to the "cursive writing" practice books you must have seen
when in kindergarten. The sheets specify the height of the main body,
the height of the ascenders and descenders and sometimes the
inclination of the letters. They serve as guides when one is writing.

To manually create such a ruling is
quite hard and error prone. It's tedious to actually measure multiples
of nib and making sure that all the lines are straight necessitates
some good drawing equipment. To save myself some trouble, I wrote a
[little script](https://github.com/nibrahim/Calligraphic-Rulings)
which takes a bunch of parameters and creates an A4 PDF with the
rulings. The only "documentation" right now is the help message which
is pretty sufficient really. 

{% highlight text %}
    Usage: ruling.py [options] output_file
    
    Options:
      -h, --help            show this help message and exit
      -n NIB_WIDTH, --nib-width=NIB_WIDTH
                            Width of the nib specified in millimeters. All other
                            measurements are multiples of this.
      -p PARTITIONS, --partitions=PARTITIONS
                            Comma separated list of partitions in each line
                            (specified in nib widths)
      -g GAP, --gap=GAP     gap between lines (specified in nib widths)
      --top-margin=TOP_MARGIN
                            Top margin (specified in nib widths). Default is 2
      -r RULINGS, --rulings=RULINGS
                            How many rulings to draw. Default is 10
      -a ANGLES, --angle=ANGLES
                            Comma separated list of angles (in degrees) for which
                            to draw lines on the page (for pen angle, serifs etc.)
      -t TITLE, --title=TITLE
                            A title for this ruling (usually the font name)
{% endhighlight %}

Invoking it like so `python ruling.py -g2 -p3,4,3 -n2 -r25  -a30
-t"Foundational Hand" foundational_ruling_sheet.pdf` produces
[this](/images/foundational_ruling_sheet.pdf) which works pretty
decently. 

I hope you find it useful. 
