---
layout:     post
title:      "Timing the game (and building some dynamic HTML)"
subtitle:   "Category: Minesweeper"
date:       2016-04-25 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Ok so i told a small porky in the title, I'm not going to create the dynamic HTML just yet because I only had a small amount of free time to add a timing function. I made this into a prototype of the 
board and gave it a few properties of its own. They hold the containers that will hold the seconds/minutes being counted as well as the main container. I also gave both seconds and minutes a variable to hold the second or minute count.

The last step here was to simply start a timer when there is a click. It starts an interval which ups the seconds by 1 every 1 second until 59, then ups the minutes and sets the seconds back to 0(as 
per normal timers!). I'm not sure if there is just a default way of doing this but I decided to try and do this without google to see what I could do and it seems to work. I did at first have a bit of trouble 
with the click being started every time I clicked on the board so I removed the event listener after starting the timer. I also set a property of started to true, just for reference more than anything else. 

The code I wrote is below:

<pre>Board.prototype.timer = function() {
    //These elements should be made dynamically and have an ID added rather than relying on on already made.
    var self = this;
    self.container = document.getElementById('minefield');
    self.seconds = 0;
    self.secondsContainer = document.getElementById('js-seconds');
    self.minutes = 0;
    self.minutesContainer = document.getElementById('js-minutes');

    self.startTimer = function() {
        setInterval(function() {
            if(self.seconds &lt; 59 ) {
                self.seconds +=1;
                self.secondsContainer.innerHTML = self.seconds;
            } else {
                self.minutes +=1;
                self.seconds = 0;
                self.minutesContainer.innerHTML = self.minutes;
                self.secondsContainer.innerHTML = self.seconds;
            }
        }, 1100);
        self.started = true;
        self.container.removeEventListener("click", self.startTimer);
    };
    self.container.addEventListener("click", self.startTimer);

};</pre>