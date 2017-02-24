---
layout:     post
title:      "An update to the timing mechanics"
subtitle:   "Category: Minesweeper"
date:       2016-05-03 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

I did need to split the timer up into a few prototypes so now it looks like this:
<pre>//start the timer
Board.prototype.timer = function() {
    var self = this;
    self.container = document.getElementById('minefield');
    self.seconds = 0;
    self.secondsContainer = document.getElementById('js-seconds');
    self.minutes = 0;
    self.minutesContainer = document.getElementById('js-minutes');
    self.timerGo = setInterval(function() {
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
    self.minefield.removeEventListener("click", self.startTimer);
};
//reset timer
Board.prototype.startTimer = function() {
    this.minefield.addEventListener("click", this.timer );
};
Board.prototype.resetTimer = function() {
    clearInterval(this.timerGo);
    this.seconds = 0;
    this.secondsContainer.innerHTML = this.seconds;
    this.minutes = 0;
    this.minutesContainer.innerHTML = this.minutes;
};</pre>
I started by making the timer prototype into the actual interval function. The works exactly the same as before, including removing the click handler once it has been initialised.

I added a startTimer method which simply calls the interval function upon click and then I made a resetTimer prototype. This just clears the interval and then resets the seconds and minutes back to 0. It is initialised when a game is either won or lost. Simple really...Don't know what I was worried about!

And I believe that is that! Time to make it all look nice and try and tidy my own code up a little!