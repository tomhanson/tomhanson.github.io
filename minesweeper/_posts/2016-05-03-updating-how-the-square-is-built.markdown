---
layout:     post
title:      "Updating how the square is built"
subtitle:   "Category: Minesweeper"
date:       2016-05-03 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

The first stop for me in tidying up my game was one I've been itching to do for a long time...I've made my board into a thing of 3d beauty(ish). 
It's not perfect but its good enough for what I need.

The only problem was that I had to create a whole load more html elements in my space creation object to make it work!

It now looks like this:

<pre>Space.prototype.createSpace = function() {
    this.overallContain = document.querySelector('.main-contain');
    this.space = document.createElement("div");
    this.space.classList.add('space-container');

    this.cube = document.createElement("div");
    this.cube.classList.add('cube');
    //create divs to make 3d space
    this.topSpace = document.createElement("div");
    this.topSpace.classList.add('space__side', 'top');

    this.bottomSpace = document.createElement("div");
    this.bottomSpace.classList.add('space__side', 'bottom');

    this.leftSpace = document.createElement("div");
    this.leftSpace.classList.add('space__side', 'left');

    this.rightSpace = document.createElement("div");
    this.rightSpace.classList.add('space__side', 'right');
    //this space will perform the actions so needs more markup added.
    this.frontSpace = document.createElement("div");
    this.frontSpace.classList.add('space__side', 'front');

    this.overlay = document.createElement("div");
    this.frontSpace.classList.add('space');
    this.frontSpace.classList.add(this.b);
    this.overlay.classList.add('cover');
    //variables for how many bombs are nearby
    if(this.b !== true &amp;&amp; this.bombsNear !== 0 ) {
        //if it is not a bomb space then print out the number of bombs around that space
        //bombP is the paragraph the text sits in
        this.bombP = document.createElement('p');
        //text is the amount of bombs near converted to a string
        var text = this.bombsNear.toString();
        //bombText creates the text node that holds the bomb number string
        this.bombText = document.createTextNode(text);
        //put that text inside the paragraph
        this.bombP.innerHTML = text;
        //append this info to the space
        this.frontSpace.appendChild(this.bombP);
    } else if (this.b === true ) {
        //if it is a bomb space put an SVG bomb in
        this.frontSpace.innerHTML = '&lt;svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="512px" height="512px" viewBox="0 0 512 512" enable-background="new 0 0 512 512" xml:space="preserve"&gt;\
        &lt;g&gt;\
            &lt;path d="M218.255,131.197c16.958,0,33.379,2.285,48.987,6.542V91.448h-92.995v45.008 C188.357,133.027,203.091,131.197,218.255,131.197z"/&gt;\
            &lt;circle fill="#020202" cx="218.255" cy="316.947" r="171.502"/&gt;\
            &lt;path fill="none" stroke="#000000" stroke-width="10" stroke-miterlimit="10" d="M218.255,108.849c0,0-6.776-113.407,92.398-60 c79.104,42.599,117,17,117,17"/&gt;\
            &lt;polygon points="422.598,23.551 430.759,46.499 452.756,36.042 442.3,58.04 465.247,66.2 442.3,74.36 452.756,96.358 430.759,85.901 422.598,108.849 414.438,85.901 392.441,96.358 402.897,74.36 379.948,66.2 402.897,58.04 392.441,36.042 414.438,46.499  "/&gt;\
            &lt;/g&gt;\
        &lt;/svg&gt;';
    }
    //stitch it all together
    this.frontSpace.appendChild(this.overlay);

    this.backSpace = document.createElement("div");
    this.backSpace.classList.add('space__side', 'back');
    //all divs put inside of cube container
    this.cube.appendChild(this.topSpace);
    this.cube.appendChild(this.bottomSpace);
    this.cube.appendChild(this.leftSpace);
    this.cube.appendChild(this.rightSpace);
    this.cube.appendChild(this.frontSpace);
    this.cube.appendChild(this.backSpace);

    //add cube inside of perspective wrapper for 3d effect
    this.space.appendChild(this.cube);
    //add cube inside of the minesweeper board
    this.overallContain.appendChild(this.space);
       
    //lastly disable the right click menu so it does not hinder game play.
    this.overlay.oncontextmenu = function() {return false;};

};</pre>
It looks complicated but really its just creating more elements, I commented it alot to make sure I didn't confuse myself going over it so its fairly easy to read!

It actually works exactly the same as before, and the space clicked is actually just the 'front' space of each cube.