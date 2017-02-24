---
layout:     post
title:      "Making the board"
subtitle:   "Category: Minesweeper"
date:       2016-04-13 11:30:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

I decided to make the actual spaces using javascript as well, seeing as this is supposed to be a way of improving my knowledge! Heres how it went:
<pre>Space.prototype.createSpace = function() {
    //create divs to hold info
    this.sp = document.createElement("div");
    this.ol = document.createElement("div");
    this.sp.classList.add('space');
    this.sp.classList.add(this.b);
    this.ol.classList.add('cover');
    //reference to the container
    this.wrapper = document.getElementById("minefield");
    //variables for how many bombs are nearby
    if(this.b !== true &amp;&amp; this.bombsNear !== 0 ) {
        //if it is not a bomb space then print out the number of bombs around that space
        this.ap = document.createElement('p');
        var text = this.bombsNear.toString();
        this.a = document.createTextNode(text);
        this.ap.innerHTML = text;
        this.sp.appendChild(this.ap);
    } else if (this.b === true ) {
        //if it is a bomb space put an SVG bomb in
        this.sp.innerHTML = '&lt;svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="512px" height="512px" viewBox="0 0 512 512" enable-background="new 0 0 512 512" xml:space="preserve"&gt;\
        &lt;g&gt;\
            &lt;path d="M218.255,131.197c16.958,0,33.379,2.285,48.987,6.542V91.448h-92.995v45.008 C188.357,133.027,203.091,131.197,218.255,131.197z"/&gt;\
            &lt;circle fill="#020202" cx="218.255" cy="316.947" r="171.502"/&gt;\
            &lt;path fill="none" stroke="#000000" stroke-width="10" stroke-miterlimit="10" d="M218.255,108.849c0,0-6.776-113.407,92.398-60 c79.104,42.599,117,17,117,17"/&gt;\
            &lt;polygon points="422.598,23.551 430.759,46.499 452.756,36.042 442.3,58.04 465.247,66.2 442.3,74.36 452.756,96.358 430.759,85.901 422.598,108.849 414.438,85.901 392.441,96.358 402.897,74.36 379.948,66.2 402.897,58.04 392.441,36.042 414.438,46.499  "/&gt;\
            &lt;/g&gt;\
        &lt;/svg&gt;';
    }
    //stitch it all together
    this.sp.appendChild(this.ol);
    this.wrapper.appendChild(this.sp);
};</pre>
I first created all the divs I needed, gave them all the classes I felt they need, then ran a check. I was checking to see two things:

A. that the space was not a bomb

B. if the space had any bombs around it(I'll come back to how I figured that out in a moment)

If both of those conditions were met I created a paragraph tag inside the main container and filled that with the amount of bomb spaces that are near this space(I will come back to how I did this i promise!)

If the space was a bomb then I just drew an svg bomb in the space and moved on.

Finally I stitched it all together and you may notice I have updated my

I would also point out now that as per my previous thoughts - I did need more properties for each space. The space object now looks like this:
<pre>function Space(space, x, y) {
    this.space = space;
    this.x = x;
    this.y = y;
    this.b = false;
    this.bombsNear = 0;
    this.neighbours = [];
    this.revealed = false;
}</pre>
this.space just holds a reference to which number in the array of spaces in the board that space is - I'm not sure If I will actually need that but I figure it doesn't hurt for the space to be aware.

this.bombsNear starts as 0 and then I run a check (that I will eventually get round to telling you about) on all squares around it and updates the number based on that.

this.revealed is pretty self explanatory and changes to true when a square is clicked on.

this.neighbours - As discussed before, the most complex one of the lot, an empty array to start, which after being run through a loop holds a reference to all spaces surrounding this space. This was the final property I added and probably the most critical in getting the game to work.

