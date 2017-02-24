---
layout:     post
title:      "The Bomb Counter"
subtitle:   "Category: Minesweeper"
date:       2016-04-25 13:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Following on from the timer I just wrote I decided to strike while the iron was hot and also do the bomb counter. Although I called this a bomb counter it is essentially counting how many flags you have left to use. I had to do this in two stages. 

Firstly a new prototype on the board object which found the html element(again I was a bit rush for time so didn't actually create this element dynamically) and populated it with the amount of bombs there were. The code ended up like this
<pre>Board.prototype.bombCounter = function() {
    this.containerElement = document.getElementById('js-bomb-container');
    var bomb = this.bombs.toString();
    this.bombNumber = document.createTextNode(bomb);
    this.containerElement.appendChild(this.bombNumber);
};</pre>
Next I had to dynamically change what the number was. This had to be done on the space object because the board object does not keep track of how many spaces have been flagged. The click event was ammended slightly and now looks like this:
<pre>Space.prototype.clickEvent = function() {
    var self = this;
    var cover = this.ol;
    //add an event listener to each space
    self.sp.addEventListener("mouseup", function(e) {
        //get the bomb container so it can be updated
        var bombContainer = document.getElementById('js-bomb-container');
        //parse whatever the contents of that bomb container are into an integer so they can be read and re written
        var bombsLeft = parseInt(bombContainer.innerHTML);
        //check for a right click
        e = e || window.event;
        if(e.which === 1 ) {
            if( self.ol.classList.contains('flagged') ) {
                return false;
            } else {
                if(self.revealed !== true &amp;&amp; self.bombsNear === 0 &amp;&amp; self.b !== true ) {
                    self.spaceCheck(self);
                } else {
                    self.ol.classList.remove('cover');
                    self.revealed = true;
                    return false;
                }
            }
        } else if (e.which === 3) {
            if( self.revealed !== true ) {
                if( self.ol.classList.contains('flagged')) {
                    self.ol.classList.remove('flagged');
                    bombContainer.innerHTML = bombsLeft + 1;
                    return false;
                } else if( bombsLeft &gt; 0 ) {
                    self.ol.classList.add('flagged');
                    bombContainer.innerHTML = bombsLeft - 1;
                    return false;
                }
            }
        }
    });
};</pre>
99% of the change is in the final part of the above code. If the click is a right click (e.which === 3 ) check if the space has already been revealed, if it has then obviously we don't want to 
attempt to flag it, however, if it hadn't then I need to  add the flag to the space by means of a html class and remove 1 from the bomb container number(and re populate the actual html). If it was already flagged then 
I simply reverse that by removing the class and adding 1 to the bomb container number.

Its also worth noting that I had to parse the contents of the bombContainer div into an integer in order to add or subtract from that number. It was a lesser used but incredibly simple parseInt() function:
<pre>var bombContainer = document.getElementById('js-bomb-container');
var bombsLeft = parseInt(bombContainer.innerHTML);</pre>
After a few weeks of trial and error to get to where I am, there is actually light at the end of the tunnel. I think I just need to make the outcome work correctly(for example the timer currently wont ever stop running) and then I am ready to make it look nice with css.

Hopefully that will be done really soon and I will have to start having a think about what my next project may be!