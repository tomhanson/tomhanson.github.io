---
layout:     post
title:      "The Outcome"
subtitle:   "Category: Minesweeper"
date:       2016-04-27 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Its the home straight - the outcome. I've scratched my head a little bit with this and decided to actually split this 3 ways. Outcome, win &amp; loss. The reason for this is that actually they are 3 different methods. Outcome simply checks for an outcome while win/loss actually perform the action of winning/losing.

I haven't decided just yet what I want to happen on win or loss so I left them as just an alert. 

Now back to that outcome prototype...

The first thing I did was make this into an event listener. It needs to listen to the board and check for an 
outcome every click. I tried my hardest to get this event listener as a method on the space object, 
but I couldn't out how to do it because ultimately spaces have no knowledge of each other whereas the Board object 
has a reference to every space stored in it.

So I checked for a left click only as there is no point running my event on a right click

<pre>Board.prototype.outcome = function() {
    var self = this;
    self.minefield.addEventListener('mouseup', function(e) {
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
                        return;
                    } else if (self.spaces[wl].revealed === true &amp;&amp; self.spaces[wl].b !== true) {
                        //+ 1 to a variable if the space is revealed but is not a bomb
                        spacesNowRevealed++;
                    }
                }
                //after the above loop has finished check that the space revealed variable is equal to spaces that need to be revealed(width * height)
                if (spacesNowRevealed === self.spacesToReveal ) {
                    //if it is then you win!
                    self.win();
                }
            }
        }, 30);
    });
};</pre>
As simple as that I think! Onwards...