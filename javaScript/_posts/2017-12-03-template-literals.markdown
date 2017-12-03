---
layout:     post
title:      "Template Literals"
subtitle:   "Category: JavaScript"
date:       2017-12-02 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript"
categories: "javascript"
header-img: "img/blank-space.jpg"
---

<p>When I first learned about template literals I thought brilliant, a simple bit of code which replaces the basic string notation if needs be. how much toit could there possibly be!?</p>
<p>Then I watched the videos by Wes Bos about them and realised that as per usual...alot!</p>
<p>Aside from writing strings with variables directly in them they have a number of other uses:</p>
<ul>
<li>A string across numerous lines without a backslash to stop your editor going mad.</li>
<li>The ability to nest any other javaScript you like within a string(and that nested code actually even being another template literal if you like).</li>
<li>Tagging a string with a function which can run before the string is created, allowing you to manipulate it as you see fit.</li>
</ul>

<p>And to make it even better its really rather simple</p>
<pre>
function taggedFunction(strings, ...values) {
    console.log(strings) //returns an array of the strings(including a blank one at the end). &nbsp;
    console.log(values) //returns the variables in that string(1 in this case);
}
const name = 'Tom';
const string = taggedFunction`I am a string by ${name}`;
</pre>
<p>The values of those 2 console logs are as follows:</p>
<pre>
["I am a string by ", ""]
["Tom"]
</pre>
<p>The final string in the first array will always either be the last bit of the string or if the last part of the string is a variable then a blank string.</p>
<p>The beauty of this is that it allows you to manipulate the string before its proccessed and just as an example you could wrap a span around each variable that gives it a tooltip if you wanted too.</p>
<p>All very handy stuff and more knowledge for me so its win win!</p>
<p>Next up...Destructuring</p>
