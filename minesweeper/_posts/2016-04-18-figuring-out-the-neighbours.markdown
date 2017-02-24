---
layout:     post
title:      "Figuring Out the neighbours"
subtitle:   "Category: Minesweeper"
date:       2016-04-19 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

I've picked up a bit of pace now as I have started to get into it. I got my space working and I also instantiated the createSpace prototype when creating the board so now I have a board with 100 spaces.

A small victory but really this was the simple part.  Next up and as promised: to add the bombs to the squares and then once that is done to tell each space how many bomb spaces are around them.

My first attempt at this involved creating two numbers randomly and then checking the array for those x &amp; y coordinates. Although this did work I immediately felt like it was way to much work for what was going on so I 
changed it to look for a specific space number in the main array of spaces rather than the actual coordinates of a space. I felt this was better as each coordinate had 10 possible options for the second coordinate where as just choosing a number is simple enough.

So heres how it ended up:
<pre>for (z = this.bombs; z &gt; 0; z--) {
    //round down when picking a number as the array starts at 0
    var bombSpace = Math.floor( ( (Math.random() * this.totalSpace) ) );
    if(this.spaces[bombSpace].b !== true ) {
        this.spaces[bombSpace].b = true;
        //setup a for loop to run through the spaces around each bomb space from the earlier stored array
        for(th=0; th &lt; this.spaces[bombSpace].neighbours.length; th++ ) {
            //variable to hold the current array item of the neighbours array
            var currentSquare = this.spaces[bombSpace].neighbours[th];

            //if current square is undefined then skip it as its not valid in the array
            if( typeof currentSquare !== "undefined" ) {
                switch (th) {
                    case 0:
                        if ( currentSquare.y !== 10 ) {
                            currentSquare.bombsNear +=1;
                        }
                        break;
                    case 1:
                        currentSquare.bombsNear +=1;
                        break;
                    case 2:
                        if ( currentSquare.y !== 1 ) {
                            currentSquare.bombsNear +=1;
                        }
                        break;
                    case 3:
                        if ( currentSquare.y !== 10 ) {
                            currentSquare.bombsNear +=1;
                        }
                        break;
                    case 4:
                        if ( currentSquare.y !== 1 ) {
                            currentSquare.bombsNear +=1;
                        }
                        break;
                    case 5:
                        if ( currentSquare.y !== 10 ) {
                            currentSquare.bombsNear +=1;
                        }
                        break;
                    case  6:
                        currentSquare.bombsNear +=1;
                        break;
                    case  7:
                        if ( currentSquare.y !== 1 ) {
                            currentSquare.bombsNear +=1;
                        }
                }
            }
        }
    } else {
        //if the tested space has a bomb then you must add 1 back to this array otherwise it will keep counting down despite no bomb being added
        z++;
    }
}</pre>
Let me break down my thinking here. First I open a for loop which starts as high as the amount of bombs which has been defined when the game was created(in this case 15). For every Iteration of the loop I set the bombSpace variable equal 
to a randomly generated number between 0 and 99(the amount of spaces created). A simple calculation of a Math.random() * the total spaces and then rounded down. It had to be 0-99 due to arrays starting at 0.

The next job was to check the returned space to see if it already had a bomb assigned to it. If it hadn't then assign its bomb(b) property to true and move on but if it had I needed to add + 1 to the count (remember it was counting down) otherwise it would remove 1 from the count but miss a bomb assignment for every duplicate. 

Now it starts to get fun. I've got a space, it has a bomb, now for another for loop.

For each space in that first loop I looped through all of its neighbour spaces to check whats going on with them.

I used this.spaces[bombSpace].neighbours.length for my second for loop maximum count and its worth noting that this.spaces[bombSpace].neighbours.length will always be 8 but its good practice to make this dynamic just incase.

So I set that into its own variable called currentSpace and ran a quick conditional if statement to see if it was actually a defined space(remember neighbours around really low and really high spaces will be undefined as they aren't on the board) If that was true I checked what number it was in the array. 

I am trying to make the game as dynamic as possible so it can handle all manner of different possibilities but no matter how hard I tried I couldn't figure this bit out and ended up giving in and making this next switch statement hard coded:
<pre>switch (th) {
    case 0:
        if ( currentSquare.y !== 10 ) {
            currentSquare.bombsNear +=1;
        }
        break;
    case 1:
        currentSquare.bombsNear +=1;
        break;
    case 2:
        if ( currentSquare.y !== 1 ) {
            currentSquare.bombsNear +=1;
        }
        break;
    case 3:
        if ( currentSquare.y !== 10 ) {
            currentSquare.bombsNear +=1;
        }
        break;
    case 4:
        if ( currentSquare.y !== 1 ) {
            currentSquare.bombsNear +=1;
        }
        break;
    case 5:
        if ( currentSquare.y !== 10 ) {
            currentSquare.bombsNear +=1;
        }
        break;
    case  6:
        currentSquare.bombsNear +=1;
        break;
    case  7:
        if ( currentSquare.y !== 1 ) {
            currentSquare.bombsNear +=1;
        }
}</pre>
This is a very basic yet vitally important switch statement. I'm basically working it off the fact that I know that each item in the array will always be the same neighbour to each space(e.g 0 will be diagonal up left). From there I check certain items 
in the array to see if their y coordinate is a 10 or a 1 and if they aren't I add 1 to <strong><em>their</em></strong> bombsNear property. This took me a while to get my head around but essentially if the neighbour spaces to the left of my current 
space are a 10 then I know it is not next to it - it will be the last space in the row above. The neighbours above and below I don't have to worry about this but similarly if the neighbours to the right are 1 then they are the start of a different line and therefore 
not a neighbour - I set them to undefined as well.

Now the bombsNear property of every neighbour space of all the bomb spaces know how many bombs are around them(and have a record of it stored in a variable as well).  I can use this information in the space creation prototype and print out to the html how many bombs are around each space.

I would love to hear of any better ways to achieve the same result if anyone has any!

The last part of this process to mention is this:
<pre>for (cs = 0; cs &lt; this.totalSpace; cs++ ) {
    //create the space html
    var thh;
    for(thh=0; thh &lt; this.spaces[cs].neighbours.length; thh++ ) {
        var newSquare = this.spaces[cs].neighbours[thh];
        //variable to hold the current array item of the neighbours array
        //if current square is undefined then skip it as its not valid in the array
        if( typeof newSquare !== "undefined" ) {
            switch (thh) {
                case 0:
                    if ( newSquare.y === 10 ) {
                        this.spaces[cs].neighbours[0] = undefined;
                    }
                    break;
                case 2:
                    if ( newSquare.y === 1 ) {
                        this.spaces[cs].neighbours[2] = undefined;
                    }
                    break;
                case 3:
                    if ( newSquare.y === 10 ) {
                        this.spaces[cs].neighbours[3] = undefined;
                    }
                    break;
                case 4:
                    if ( newSquare.y === 1 ) {
                        this.spaces[cs].neighbours[4] = undefined;
                    }
                    break;
                case 5:
                    if ( newSquare.y === 10 ) {
                        this.spaces[cs].neighbours[5] = undefined;
                    }
                    break;
                case  7:
                    if ( newSquare.y === 1 ) {

                        this.spaces[cs].neighbours[7] = undefined;
                    }
                    break;
                default:
                    newSquare = undefined;
            }
        }
    }
    //init these here so that the course has a chance to give them a bomb and their surrounding bombs info
    this.spaces[cs].createSpace();
    this.spaces[cs].clickEvent();
}</pre>
It works very similarly to the last loop except this time it loops through every space and checks its neighbours. It does exactly the same check as the previous loop except this time if the space falls on a 
different line because it is either the 1st or the 10th space then I am re setting it to undefined so it can't be confused for it in the future.

It may seem overkill doing the loop twice but the last one only runs for bomb spaces, this needs to run for every space.

From now on my neighbours array on each space tells me which spaces are around it only. Again I am sure there is a better way to do this but I havn't quite figured it out yet.

The final part of this for loop is to initialise the createSpace function. On the surface it may seem a bit odd placing the initialisation here but I can't do it before as the board doesn't know all the information 
to create the space fully(bombs near, is it a bomb etc..) before now and this is the first loop through every space that has given me the chance to do it.


