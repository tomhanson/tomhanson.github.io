---
layout:     post
title:      "Win Loss Function"
subtitle:   "Category: Minesweeper"
date:       2016-05-01 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Disaster has struck - but thankfully it's nothing too serious!

The problem goes like this: When you win or lose the board needs to reset. Simple enough I thought...but everything needs to be reset and I didn't really think of that(although in hindsight I probably should have).

So what does this mean? I think it means I have to split some methods. So for example, where I have a timer function, I'm going to need a timer start and timer reset functions.

I have taken a second to add an initialisation function too:
<pre>Board.prototype.init = function() {
    if(this.minefield.hasChildNodes() ) {
        while ( this.minefield.firstChild ) this.minefield.removeChild( this.minefield.firstChild );
    }
    //create the board prototype init
    this.createBoard();
    //add bombs prototype init
    this.addBombs();
    //creation of timer in its own prototype
    this.timer();
    //build the bomb counter
    this.bombCounter();
    //event listener on the board for win/loss
    this.outcome();
};</pre>
This will hopefully allow me to easily restart the board in one call when necessary!