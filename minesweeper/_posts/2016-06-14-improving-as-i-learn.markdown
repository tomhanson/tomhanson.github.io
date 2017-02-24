---
layout:     post
title:      "Improving as I learn"
subtitle:   "Category: Minesweeper"
date:       2016-06-14 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Currently I am reading a book series called 'You don't know JS'. Im finding it really useful and learning lots. As I am learning I am trying to apply it to my minesweeper game if applicable. One thing I've learned today is arrow functions. They are an ES6 feature but they seem to be really handy for sharing lexical scope. Heres how I used it:

I went from:
<pre>Board.prototype.startTimer = function() {
    var self = this;
    self.timerGo = setInterval(function() {
        if(self.seconds &lt; 59 ) {
            self.seconds +=1;
            if(self.seconds &lt; 10 ) {
                self.secondsContainer.innerHTML = '0' + self.seconds;
            } else {
                self.secondsContainer.innerHTML = self.seconds;
            }

        } else {
            self.minutes +=1;
            self.seconds = 0;
            self.minutesContainer.innerHTML = self.minutes;
            self.secondsContainer.innerHTML = self.seconds;
        }
    }, 1000);
    self.started = true;
    self.minefield.removeEventListener("click", self.timerGo);
};</pre>
to:
<pre>Board.prototype.startTimer = function() {

    this.timerGo = setInterval( () =&gt; {
        if(this.seconds &lt; 59 ) {
            this.seconds +=1;
            if(this.seconds &lt; 10 ) {
                this.secondsContainer.innerHTML = '0' + this.seconds;
            } else {
                this.secondsContainer.innerHTML = this.seconds;
            }

        } else {
            this.minutes +=1;
            this.seconds = 0;
            this.minutesContainer.innerHTML = this.minutes;
            this.secondsContainer.innerHTML = this.seconds;
        }
    }, 1000);
    this.started = true;
    this.minefield.removeEventListener("click", this.timerGo);
};</pre>
What this did was allow me to remove the 'self = this' variable and force the setTimeout function to use 'this from the overall function instead. Now to apply this everywhere else in my script...!
