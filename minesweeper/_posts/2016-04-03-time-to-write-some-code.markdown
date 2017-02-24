---
layout:     post
title:      "Time to write some code."
subtitle:   "Category: Minesweeper"
date:       2016-04-03 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;Minesweeper"
categories: "minesweeper"
header-img: "img/learnjs-bg.jpg"
codepen:    "http://codepen.io/tomhanson1985/pen/JXegpO"
---

<p>I've started with my board object. Its a constructor function and it takes 3 parameters - width, height, amount of bombs. All pretty self explanatory really 
but you've got to start somewhere. Some how I've managed to break my method on the prototype rule within my first few lines of code...oops, but it was only small so I've forgiven myself...width * height gives total spaces on the board.</p>

<p>Another small note is that I have left all three parameters optional because in the traditional minesweeper you can just play without entering any information. I used a bit of a cool JavaScript trick for that:</p>
<pre>this.width = width || 10;
this.height = height  || 10;
this.totalSpace = this.width * this.height;
this.bombs = bombs || (this.totalSpace / 100) * 15;</pre>
<p>which basically means the || (or) operator will be assessed first due to operator precedence and then the variable set. I was pretty happy with that one.</p>

<p>So the board object made, now the space object. I really wasn't sure how to approach this so I kept it simple to start with and went for x &amp; y as the parameters.</p>

<p>My first thought is that this is going to be enough parameters.</p>

