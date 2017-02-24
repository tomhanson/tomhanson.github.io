---
layout:     post
title:      "Adding a Click Event"
subtitle:   "Category: Minesweeper"
date:       2016-04-20 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

I think I'm moving into the latter stages of making this game now, so I am going to have a quick recap of where I am at so far.

I have a board object and a space object(well numerous space objects). My board will create the correct spaces based on parameters and each space will build itself and put itself inside the minefield container. 
Each space also has awareness of all of its neighbour spaces as well as if they are a bomb and how many bomb spaces are neighbouring it too.

I've decided to tackle the click event. At the moment I have taken the win/lose scenario out of the equation and I am just focusing on the click and the what will happen upon clicking a square. This is what I am hoping for:
<ul>
 	<li>A right click to add a flag to the bomb(symbolised by a color change to red at this stage)</li>
 	<li>A left click to reveal a space unless that space is flagged</li>
 	<li>If I click a space and it has no bombs around it, then it should continually check spaces around it, opening them all out until it comes to spaces which do have bombs next to it.(This is what happens when you click the board and a large chunk of spaces are revealed).</li>
 	<li>Some sort of visual representation of how many flags are left. A note here is that the total amount of flags is always exactly the same as the amount of bombs.</li>
</ul>
I started simple. A click event, or to be precise a mouse down event. I don't know why I chose mouse down but as far as I know it doesn't really matter. I guess I just fancied a change!

I made the click handler as a prototype of the space object so that every space object could access it. It looks like this:
<pre>Space.prototype.clickEvent = function() {
    var self = this;
    var cover = this.ol;
    //add an event listener to each space
    self.sp.addEventListener("mouseup", function(e) {
        //check for a right click
        e = e || window.event;
        if(e.which === 1 ) {
            if( self.ol.classList.contains('flagged') ) {
                return false;
            } else {
                if(self.revealed !== true &amp;&amp; self.bombsNear === 0 &amp;&amp; self.b !== true ) {
                   //check other spaces
                } else {
                    self.ol.classList.remove('cover');
                    return false;
                }
            }
        } else if (e.which === 3) {
                self.ol.classList.add('flagged');
            }
        }
    });
};</pre>
You may recall this.sp is a property of the createSpace prototype and it represents the actual space html div.

My first check was for a right click. This had to be the starting point as the two possible outcomes here made a world of difference, and its just an if statement anyway.

The next part was a small bit of cross browser trickery, setting the event to be a variable 'e' regardless of how the browser returns it. Next up I checked what that event actually returned and if it 
returned 1(left click) then I had to check that the space hadn't been flagged, if it had been flagged then the function ended there. If it hadn't I moved on to my next check.

Its worth pointing out here as well that I have tried quite a number of different orders for these conditional checks but in the end had to split them up quite a bit and order them this way due to the many different outcomes of the actions taken.

Moving on though..my next check was to see if the space had been revealed, that it didn't have any bombs near it and of course that it wasn't a bomb itself. If that was the case then I needed to start checking its neighbours spaces as 
mentioned earlier on. If any of those criteria were not met then it was safe to just reveal the space and set that spaces 'revealed' property to true.

The final section of the function is just saying that if the click event returns 3(right click) then add the flag to that space. Flags can go on any space that isn't revealed so this is relatively easy to check for. It isn't perfect 
but I've done the majority of what I set out to do. There are still a few things I will be trying to tackle next though to tidy this up:
<ul>
 	<li>I need to check the space I right click isn't revealed</li>
 	<li>I still need to prevent the context menu appearing</li>
 	<li>I need to be able to remove flags</li>
 	<li>I need flagged spaces to not be clickable</li>
 	<li>I need a visual representation of the bombs left for the user to be able to see.</li>
</ul>
Out of time now though so it will have to be next time.

&nbsp;

