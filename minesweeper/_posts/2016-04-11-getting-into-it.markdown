---
layout:     post
title:      "Getting into it"
subtitle:   "Category: Minesweeper"
date:       2016-04-11 11:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

<p>I've decided to add to the space object a property of bomb(this.b) which is set to false by default and then will be changed if needed further down the line.</p>

<p>Now the Board object gets its first method, and its createBoard.</p>

<p>The objective of this as you may imagine is to create the board. It was Relatively simple, I set up an empty array called spaces then I loop through each number from the totalspace property, to get the amount of squares I need and create a space object for each iteration of the loop. 
As well as each space being an object and it being easier to work on, this also gives the board a really good reference to each space within it. I've got a feeling that will come in handy when this thing gets more complex. Now where was I...?</p>

<p>This gave me the spaces for my test game (100 in this case as i used no parameters) but I've realised I will need to assign x &amp; y coordinates to each space so I've had to rethink this slightly. In the end I have had to add an extra 'for' loop in. The idea being that I have a for loop 
for each which runs once for each number in the width of the board(10 in this case), then for each number you do another for loop which populates the y coord. (e.g x:1 y:1, x:1 y:2, x:1 y:3, etc..) Its probably easier to explain with an example. it looked like this:</p>
<pre>for(x = 1; x &lt;= this.width; x++ ) {
    for(y = 1; y &lt;= this.height; y++ ) {
        this.spaces.push( new Space(x, y) );
    }
}</pre>
<p>The final bit there just pushes each space to the spaces array so they are all kept in one place.

<p>The last task of creating the board was to assign each space with a reference to its 'neighbour' spaces. Fortunately this will always be the same so each space can be tested relatively simply. Unfortunately it required another for loop...</p>
<pre>for (count = 0; count &lt; this.totalSpace; count++ ) {
    var diagUpLeft = this.spaces[ (count - this.width ) - 1 ];
    var up = this.spaces[ count - this.width ];
    var diagUpRight = this.spaces[ (count - this.width) + 1 ];
    var back = this.spaces[ count - 1 ];
    var forward = this.spaces[ count + 1 ];
    var diagDownLeft = this.spaces[ (count + this.width) - 1 ];
    var down = this.spaces[ count + this.width ];
    var diagDownRight = this.spaces[ (count + this.width) + 1 ];
    //add these to an array in each space for easy access
    this.spaces[count].neighbours.push(diagUpLeft, up, diagUpRight, back, forward, diagDownLeft, down, diagDownRight);
}</pre>
<p>The spaces around each space are listed out as variables symbolising their relation to the current space then the calculations are to get to the target space from the space you are currently on.  
The maths above is fairly self explanatory so I wont go into to much detail there. The final part of all that is just to push these spaces to an array for each space  which I called neighbours so they can be referenced again.</p>

<p>'Ah, but any space before the first space is surely going to return undefined?' I hear you say! Correct - but I have a plan and will deal with that which I think will be pretty handy, I'll come back to that when I actually use this data</p>