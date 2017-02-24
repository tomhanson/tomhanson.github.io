---
layout:     post
title:      "More Tidying"
subtitle:   "Category: Minesweeper"
date:       2016-06-09 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

I am trying to tidy up my code now so I have adjusted a few things.

Firstly, I have removed the start Timer prototype, After changing the board initialisation to work from a button this was no longer needed. Once the button is clicked the timer starts, I have however changed the timer prototype to 'startTimer' instead so it makes it easier for me to know exactly whats going on here.

I also adjusted the resetBombCounter prototype so that it just removes the bombs from that box.
<pre>Board.prototype.resetBombCounter = function() {
    this.containerElement = document.getElementById('js-bomb-container');
    this.containerElement.innerHTML = ' ';
};</pre>
One other thing I had noticed was clicking the start button multiple times initiated multiple minefields. It now runs a quick check like so:
<pre>var html = document.querySelector('html');
if( !html.classList.contains('game-in-progress') ) {
    init(10, 10, 15);
}</pre>
I also added this when the board is created
<pre>this.html.classList.add('game-in-progress');</pre>
and this when an outcome is reached
<pre>this.html.classList.remove('game-in-progress');</pre>
Bit of a jumpy update but just lots of little things all updated in one sitting!