--- 
layout: post
title: Seal tutorial
wordpress_id: 50
wordpress_url: http://nibrahim.net.in/journal/?p=50
---
My first attempt to write some kind of a GIMP + Inkscape tutorial.

The plan is to create something that looks like a seal made with a rubber stamp like this
<img id="image39" alt="seal-final.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-final.thumbnail.png" />

<!--more-->

You need two tools to do this. The vector graphics tool called Inkscape and the raster graphics program called the GIMP.
<ol>
	<li>Start off with inkscape and draw two concentric circles. This can be done using the circle tool from the toolbar on the left. Hold ctrl when you draw the circles to make sure that they're proper circles. Use Ctrl-Shift-F to set their inner fill to empty and their stroke thickness to something like 15px. Once you draw two of them, you can use the "arrange and distribute" tool (using ctrl-shift-A) to make their centers coincide. Use the align vertical and horizontal center buttons shown here.
<img id="image40" alt="seal-tutorial-align-and-distribute.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-align-and-distribute.thumbnail.png" /></li>
	<li>Now select the inner ring, make a copy of it (ctrl-c ctrl-v), enlarge it a little and move it aside(let's call this circle <em>circle2</em>). Use the text tool and a bold variant of a typewriter like font (I used courier) to write the text you want to appear on the top. Click on the text, then hold shift and click on <em>circle2</em> to select them both. Then select Text->Put on Path from the menus on top. If you did it right, you should get something like this.
<img id="image42" alt="seal-tutorial-step2.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-step2.thumbnail.png" /></li>
	<li>Click on the circle and then shift-click on the text to select both of them. Use Ctrl-G to group them into a single entity and then double click on the group using the selection tool (the arrow icon). Rotate the whole thing till the text comes neatly on top. It should look like this now
<img id="image44" alt="seal-tutorial-step3.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-step3.thumbnail.png" /></li>
	<li>Okay. Now take this group and position it such that the text appears in between the concentric circles you drew in step 1. After that, select this group and use ctrl-U to ungroup them. Select the circle on which the text lies and use ctrl-shift-F to get the fill/stroke dialog. Select "no stroke" for the circle. The end result should be something like this. You might have to make some small adjustments to get the spacing proper. Select all the elements here and group them together using Ctrl-G.
<img id="image45" alt="seal-tutorial-step4.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-step4.thumbnail.png" /></li>
	<li>Now use a bold heavy font to put some text inside the inner circle slightly towards the top (I used Arial Black). Draw a line below the text and use the font you used for the top arching text to write something more. You can also use the star tool to draw a star or two to give the seal some personality. Use centrally aligned text and make sure that all the fonts are bold. Verically align all the stuff you drew. Group everything together and then click twice on the group using the selection tool to get the rotation handles. Rotate it about 45 degrees. The result will be something like this.
<img id="image48" alt="seal-tutorial-step52.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-step52.thumbnail.png" /></li>
	<li>Okay. Now, select this group and use ctrl-shift-E to export the drawing as a .png. Then open it with the GIMP.</li>
	<li>In the GIMP use Layer->Transparency->Alpha to selection to select the drawn part. Use Script-Fu->Selection->Distress selection to make the selection a little uneven (you might have to play with the parameters to get it right). Use Selection->Invert and then then Edit->Clear to remove some of the colour. You'll get an slightly distorted image which is your final product.
<img id="image49" alt="seal-tutorial-step7.png" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-step7.thumbnail.png" /></li>
	<li>To get higher quality images, use large size images for everything and finally scale your results to the appropriate size.</li>
</ol>
You can vary the shape, fonts and items used to get some interesting effects. Here is another one that I made.

<img alt="seal-tutorial-sample1.png" id="image51" src="http://nibrahim.net.in/journal/wp-content/uploads/2007/08/seal-tutorial-sample1.thumbnail.png" />
