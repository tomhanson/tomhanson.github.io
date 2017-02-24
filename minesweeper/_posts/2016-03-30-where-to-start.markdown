---
layout:     post
title:      "Where do I start?"
subtitle:   "Category: Minesweeper"
date:       2016-03-30 13:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

<p>So where did I start?</p>
<p>I went old school and sketched it all out. My first(very quick) attempt ended up with these two objects:</p>

<ul>
	<li class="p1">Board Object</li>
	<li class="p1">Space Object</li>
</ul>
<p>Actually this was't too far from how I ended up. I would mention at this point I was also watching a Javascript video series on Udemy by Anthony Alicea and my knowledge was increasing quickly so it became a little easier for me to make this decision.</p>

<p>I have also decided that the objects would hold the information(properties), and the prototypes of these objects would do the work and hold the methods. The reason I decided this was so that the objects wouldn't be too heavy processing every method every time they are called(which could potentially be a lot with the a variable board size) but they could all have access to the methods if needed.</p>

<p>The other thing I decided at sketch stage was which prototypes to use for each object an came up with this:</p>
<ul>
	<li>createBoard</li>
	<li>addBombs</li>
	<li>createSpace</li>
</ul>
<p>I'm not exactly sure what I am going to need so I thought think this is a good starting point...Now to get to work writing some actual code.

