---
layout:     post
title:      "I've done a few minor tweaks"
subtitle:   "Category: Minesweeper"
date:       2016-05-13 07:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

More finesse, but only a few minor tweaks. First up, and after a couple of days of thinking about it I've moved the initialisation click event out of the object. its now just in the JS file as follows:
<pre>var st = document.getElementById('js-start');
st.addEventListener('click', function() {
    init(10, 10, 1);
});</pre>
I had to remove the init function from the win and loss function as well, not sure why I even put them there but I only want this game initialised on a click so now its all good!

I also got to remove this:
<pre>self.startButton.removeEventListener('click', arguments.callee);</pre>
which is good because I think arguments.callee is deprecated and I wasn't exactly sure how it works!

Because I have updated this I also decided to update the win function.
<pre>//win function
Board.prototype.win = function() {
   alert('winner');
    this.spaces.length = 0;
    this.minefield.innerHTML = '';
    this.bestTime = document.getElementById('js-best-time');
    this.time = this.minutesContainer.innerHTML + ':' + this.secondsContainer.innerHTML;
    var currentBest = this.bestTime.innerHTML;
    if(currentBest &gt;= this.time || currentBest == false) {
        this.bestTime.innerHTML = this.time;
    }
    this.resetOutcome();
    this.resetTimer();
};</pre>
I updated the destroying of the actual objects by using this.spaces.length = 0. This is just my attempt to clear out memory a bit if someone was to play a few(hundred) times in a row( does this work? I have no idea at this moment!)

I also originally used a while loop to destroyed the html inside the minefield so its ready to receive a whole new game. I changed that slightly in this update by instead just setting the innerHTML to nothing:
<pre>this.minefield.innerHTML = '';</pre>
The final bit was just a visual representation of what the players best score is. All I did here was concatenate the html of the minute and second containers into one variable, then ran a quick check to see if it beat the best score, if it did then I updated it. This part:
<pre>|| currentBest == false</pre>
was just for the first time you played as a top score hadn't been set yet!