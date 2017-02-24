---
layout:     post
title:      "Another Challenge"
subtitle:   "Category: Minesweeper"
date:       2016-04-22 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Its starting to come down to finesse now and I decided to tackle the click functionality fully.  Heres what I have had a crack at this time round.
<ul>
	<li>make the right click completely disable context menu(frowned upon)</li>
	<li>make the right click add and then remove the flag on alternate clicks</li>
	<li>Add a counter for the bombs so you know how many flags you have left to place.  The extra thing to add to this is that I will also limit the amount of flags placeable to the amount of bombs in the board.</li>
</ul>
So I started with disabling the context menu. Unsure as ever I took to my old mentor Google. I got almost the same result on every post I saw which also came with the same warning - "<strong>Do not do this on a website as it is not good for user experience".</strong>

I had a think and decided I could live without it for this experiment so I went ahead and disabled it. Its relatively simple code to do to as it happens:
<pre>document.oncontextmenu = document.body.oncontextmenu = function() {return false;};</pre>
'.oncontextmenu' is what how javascript targets that right click menu so returning false just means it does nothing. Aside from one small caveat that you have to left click first before the context menu stops working....simple. And I am pretty sure that is down to the fact I only fire that function upon the first click - I'll look at that another time though.

Next I moved on to making the right click add or remove a flag depending on its current state. Easy I thought...

And it was, It was just the added bug that came up with my previous space checking function that proved slightly more difficult but I will come to that later.

As I mentioned, this functionality was relatively simple. Add a check on a right click(within a space) to see if:
<ol>
	<li>there was already a flag.</li>
	<li>If there wasn't a flag I had some flags left to place.</li>
</ol>
If it was a left click then I returned false so the function ends. If it was a right click I just simply checked if the space has a flag, if it did I removed it, if It didn't I added it. I also returned false at the end just to make sure nothing else ran.

All done and dusted I thought. Here was how the code looked at the end:
<pre>self.sp.addEventListener("mouseup", function(e) {
    document.oncontextmenu = document.body.oncontextmenu = function() {return false;};
    //check for a right click
    e = e || window.event;
    if(e.which === 1 ) {
        if(self.ol.classList.contains('flagged') ) {
            return false;
        } else {
            if(self.revealed !== true &amp;&amp; self.bombsNear === 0 &amp;&amp; self.b !== true ) {
                spaceCheck(self);
            } else {
                self.ol.classList.remove('cover');
                return false;
            }
        }
    } else if (e.which === 3) {
        if( self.ol.classList.contains('flagged')) {
            self.ol.classList.remove('flagged');
            return false;
        } else {
            self.ol.classList.add('flagged');
            return false;
        }
    }
});</pre>
Curveball alert: When the space check function ran it didn't account for whether there was a flag so it just removed spaces regardless of their flag status. Relatively simple to fix. I ran a check on the space checker function to see if it was flagged and only continue if it wasn't.

I haven't quite got round to making the box which tells me how many bombs(and flags) there are so I will have to put that on my to do list.

Up Next - configuring the the timing function(and creating some more html dynamically), then get the bomb counter created.
