---
layout:     post
title:      "A lot of work to do on the click still"
subtitle:   "Category: Minesweeper"
date:       2016-04-20 08:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

Theres one small thing I never achieved last time out - and it was pretty simple so I've sorted it. I needed to check when i right clicked that the space wasn't revealed for obvious reasons. I just wrapped the whole right click event in this:
<pre>if( self.revealed !== true ) {</pre>
The next thing I need to do is prevent flag spaces being clickable. I tackled this next because it is almost identical to what I've just done.
<pre>if( self.ol.classList.contains('flagged')) {</pre>
I ran the check just after I checked to see if the space had been revealed because there is no point checking if the space has a flag if the space has already been revealed.

Now for something a little more serious. As I mentioned previously, in 'real' minesweeper if you click a square which has no bombs around it then it will reveal that space and if it has no bombs around it(i.e none of its neighbouring spaces are a bomb) it will also 
reveal all the spaces around it. If any of those spaces have no bombs around them it will continue to reveal until the space has bombs around it.

Now I've got that logic clear in my head its time to figure out how I am going to achieve it. Heres my plan of action.
<ul>
 	<li>on left click check if the space has been revealed if it hasn't then reveal it and move on to the next step</li>
 	<li>check if the space has any bombs near it. If it does then stop there, if it doesn't then I need to reveal all its neighbours spaces and run the same check on them. This needs to carry on until all spaces have a bomb near them.</li>
</ul>
With the plan in place I've started with this:
<pre>function spaceCheck(space) {
    //check the space isn't flagged
    if( !space.ol.classList.contains('flagged') ) {
        //check if the given space falls within the parameters of the board and it isn't holding a bomb
        if( typeof space !== "undefined" &amp;&amp; space.b !== true ) {
            space.ol.classList.remove('cover');
            space.revealed = true;
            //start a for loop to run through each item in the neighbours array(spaces around current space)
            for(ch=0; ch &lt; space.neighbours.length; ch++ ) {
                var ch;
                //variable to hold the current space as an object
                var currentSpace = space.neighbours[ch];
                //check if space is defined
                if( typeof currentSpace !== "undefined" &amp;&amp; currentSpace.b !== true ) {
                    //if defined check if it has any bombs near
                    if( currentSpace.bombsNear === 0 ) {
                        //make sure the space does not have the 'revealed' property set to true
                        if( currentSpace.revealed !== true ) {
                            //recursively run the function(it takes a space object parameter) until it returns false.
                            this.spaceCheck(currentSpace);
                        }
                    } else  {
                        currentSpace.ol.classList.remove('cover');
                        currentSpace.revealed = true;
                    }
                }
            }
        }
    }
};</pre>
Its pretty meaty so I'll walk through my thinking. The first thing to mention is the amount of conditional statements. I actually got this function semi working before I wrote this so I had a bit of time to play around with the positioning of these conditional checks, and this is how I felt it worked best.

First of all I run my check to see if the space is flagged. If it isn't I move forward.

Next I check to see if the space is undefined. You may think this is a bit of an odd check consdiering the function is called on click but as I am hoping to run this function within itself there is potential for some of the neighbour spaces to be 
undefined because I changed them earlier on in the code to make this check easier. I also check to see if the space is a bomb at this point, obviously if it is then theres no point checking spaces!

At this point I need to do two things
<pre>space.ol.classList.remove('cover');
space.revealed = true;</pre>
remove the spaces cover and set it to revealed. I've had to add this.revealed as a property of each space and set it to false by default to help me keep track of what is going on. The reason 
behind this is that if the board doesn't know which spaces are revealed then it will keep trying to check them and I will be stuck in an infinite loop with a browser not responding!

Now assuming all of the above conditions have been met, I now need to check all of the spaces neighbour spaces. Fortunately earlier on I stored a reference to every space around each space in an array on the space object(using the property this.neighbours). So I ran a for loop through all of those to get their values.

for each item in the for loop I am doing the exact same checks as before, only within this part of the function I also need to check if the current neighbour space has any bombs near it. If it does 
then the space will be revealed and the loop will move on. If it doesn't then I will be calling my function again - the function takes the space object as its parameter so it can run as many times as it needs to.

At first glance this seems to be working, I've been getting some peculiar results when testing it though. It seems that when the function calls itself it will break from the loop when it needs to call 
itself again, not revealing any more neighbour spaces after that. Time to debug.

Update:

After quite some time banging my head against a brick wall, purely by chance I moved(hoisted) the ch variable to the top of the function and it worked. I think the reason for this is because the main spaceCheck function couldn't see 
the ch variable within the for loop as it was out of scope. by moving it higher it bought it into the scope of the function and allowed the function to keep track of where in the for loop it is.

I have also decided to move this function to its own prototype, just for ease of reading more than anything else. It needs cleaning up a bit but thats a job for a bit further down the line I think!. It finished up looking like this:
<pre>Space.prototype.spaceCheck = function(space) {
  var ch;
  //check the space isn't flagged
  if( !space.ol.classList.contains('flagged') ) {
      //check if the given space falls within the parameters of the board and it isn't holding a bomb
      if( typeof space !== "undefined" &amp;&amp; space.b !== true ) {
          space.ol.classList.remove('cover');
          space.revealed = true;
          //start a for loop to run through each item in the neighbours array(spaces around current space)
          for(ch=0; ch &lt; space.neighbours.length; ch++ ) {
              //variable to hold the current space as an object
              var currentSpace = space.neighbours[ch];
              //check if space is defined
              if( typeof currentSpace !== "undefined" &amp;&amp; currentSpace.b !== true ) {
                  //if defined check if it has any bombs near
                  if( currentSpace.bombsNear === 0 ) {
                      //make sure the space does not have the 'revealed' property set to true
                      if( currentSpace.revealed !== true ) {
                          //recursively run the function(it takes a space object parameter) until it returns false.
                          this.spaceCheck(currentSpace);
                      }
                  } else  {
                      currentSpace.ol.classList.remove('cover');
                      currentSpace.revealed = true;
                  }
              }
          }
      }
  }
};</pre>
<p>Whittling that list down now, just a few things left to do here:</p>
<ul>
 	<li>I still need to prevent the context menu appearing</li>
 	<li>I need to be able to remove flags</li>
 	<li>I need a visual representation of the bombs left for the user to be able to see.</li>
</ul>

