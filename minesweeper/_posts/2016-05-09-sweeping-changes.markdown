---
layout:     post
title:      "Sweeping changes( no pun intended)"
subtitle:   "Category: Minesweeper"
date:       2016-05-09 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Something I thought would would be really easy has turned into a bit of a mammoth challenge. The problem is this...

When the page loads the board initialises. What I wanted was for it to initialise on a button click. Simple I thought, add a click handler which initialises everything...wrong!

The timing function is here:
<pre>//start the timer
Board.prototype.timer = function() {
 var self = this;
 self.container = document.getElementById('minefield');
 self.seconds = 0;
 self.secondsContainer = document.getElementById('js-seconds');
 self.secondsContainer.innerHTML = self.seconds;
 self.minutes = 0;
 self.minutesContainer = document.getElementById('js-minutes');
 self.minutesContainer.innerHTML = self.minutes;
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
 }, 1100);
 self.started = true;
 self.minefield.removeEventListener("click", self.timerGo);
};</pre>
and I initialised it like this:
<pre>Board.prototype.startTimer = function() {
 this.minefield.addEventListener("click", this.timer );
};</pre>
If you thought that may work, then you would be under the same mindset that I was! fast forward about 3 hours and I figured out the problem.<em> Now this is the important bit:</em>

When I set an event listener 'this' changes and it now refers to whatever the event handler is attached too. In this case that would be the actual minefield from this.minefield.

Because of the above(which I did think was a strange mechanic) It meant that my click handler as far as I am aware had to actually be initialised on the object itself(and the object needs a variable of this for it to reference. I duly obliged like so:
<pre>//timer properties
//set a variable for this
var self = this;
this.seconds = 0;
this.secondsContainer = document.getElementById('js-seconds');
this.secondsContainer.innerHTML = this.seconds;
this.minutes = 0;
this.minutesContainer = document.getElementById('js-minutes');
this.minutesContainer.innerHTML = this.minutes;
//use button for initialisation
this.startButton = document.getElementById('js-start');
this.startButton.addEventListener('click', function() {
    //create the board prototype init
    self.createBoard();
    //add bombs prototype init
    self.addBombs();
    //creation of timer in its own prototype
    self.timer();
    //build the bomb counter
    self.bombCounter();
    //event listener on the board for win/loss
    self.outcome();
    //remove event listener on start button
    self.html.classList.add('animation');
    self.startButton.removeEventListener('click', arguments.callee);
});</pre>

Oh and arguments.callee...something I've never used before and to be honest I wasn't really that clear about how it works and will be looking into it further when I get the chance. For the time being I think MDN explains it best [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/callee).

Ok, so now it all works properly on a button click! This is another one where I am not sure whether this is the best way to do this(and it probably isn't) but I can't think of another way right now.

You may notice as well that I moved the properties of my timer prototype to the actual object. Its another case of not sure if its the right thing to do but it feels like it is so I'm going to stick with it!

One small change that effects nothing but I may as well update you on is this:
<pre>self.html.classList.add('animation');</pre>
I added this upon initialisation(self.html just targets the html tag) and removed it upon an outcome. All this does is allow the board to do its 3d effect every time it starts up.