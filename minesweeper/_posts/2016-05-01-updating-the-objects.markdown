---
layout:     post
title:      "Updating the Objects"
subtitle:   "Category: Minesweeper"
date:       2016-05-01 13:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

As I thought I was coming to the end of the game I suddenly realised that none of my methods had a way of stopping when the game was finished. Because of this I've had to add a few more methods in. They are the following:
<ul>
 	<li>resetOutcome</li>
 	<li>outcome(I just ammended that)</li>
 	<li>resetTimer</li>
 	<li>resetBombCounter</li>
</ul>
First of all I tackled the outcome. As mentioned above I ammended the outcome method. I changed it to this:
<pre>Board.prototype.outcome = function() {
    var self = this;
    self.outcome = function(e) {
        //set the spaces revealaed back to 0 ready for the next click;
        var spacesNowRevealed = 0;
        //for loop variable hoisted
        var wl;
        //set a small timeout to allow for the space to be revealed
        setTimeout(function(){
            //if a left click
            if(e.which === 1 ) {
                //run through each space
                for (wl = 0; wl &lt; self.spaces.length; wl++) {
                    //check if its any of the bombs have been revealed
                    if (self.spaces[wl].revealed === true &amp;&amp; self.spaces[wl].b === true) {
                        //bomb revealed means you have lost!
                        self.loss();
                        return false;
                    } else if (self.spaces[wl].revealed === true &amp;&amp; self.spaces[wl].b !== true) {
                        //+ 1 to a variable if the space is revealed but is not a bomb
                        spacesNowRevealed++;
                    }
                }
                //after the above loop has finished check with the space revealed variable is equal to spaces that need to be revealed(width * height)
                if (spacesNowRevealed === self.spacesToReveal ) {
                    //if it is then you win!
                    self.win();
                }
            }
        }, 30);
    };
    self.minefield.addEventListener('mouseup', this.outcome );
};</pre>
I tried to keep this as just a function which was called with the adding and removing of the event listeners on seperate prototypes but I can't get it to work that way at the moment so I have just left the event listener at the bottom and it seems to be working!  Its a bit of a mess at the moment so I will have to tidy it up at some point.

Oddly enough removing the event listener using its own prototype works perfectly.
<pre>Board.prototype.resetOutcome = function() {
    self.minefield.removeEventListener('mouseup', this.outcome );
};</pre>
Fixing the bomb counter to reset properly was the simplest of the lot so I have quickly done that. It went from:
<pre>Board.prototype.bombCounter = function() {
    this.containerElement = document.getElementById('js-bomb-container');
    var bomb = this.bombs.toString();
    //this.bombNumber = document.createTextNode(bomb);
    this.containerElement.appendChild(this.bombNumber);
};</pre>
to:
<pre>Board.prototype.bombCounter = function() {
    this.containerElement = document.getElementById('js-bomb-container');
    this.containerElement.innerHTML = this.bombs;
};</pre>
Looking at that now, I'm not sure why I didn't just do that in the first place! The final thing is the timer. This will be slightly more complicated so thats for another day.

Oh....I also did this as a way of resetting the game when I need to:
<pre>function init(width, height, bombs) {
    new Board(width, height, bombs);
}</pre>
This gives me a way to initialise the board by simply calling a function.
