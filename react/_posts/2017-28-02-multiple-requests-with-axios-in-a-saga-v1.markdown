---
layout:     post
title:      "Multiple Requests with Axios in a saga (v1)"
subtitle:   "Category: React"
date:       2017-02-28 15:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;React"
categories: "React"
header-img: "img/sagas.jpg"
---

<p>As a slight follow up to my previous post, heres something else I have figured out: making multiple requests within the same saga.</p>
<p>Here was the call I made inside my componentWillMount function of my home component. The parameters are a little tricky at first glance but they run like this:</p>
<ol>
    <li>post type(post, project, page)</li>
    <li>amount of posts</li>
    <li>post slug(if required)</li>
    <li>Any extra string information</li>
</ol>
<p>I will refine that to make it a tad easier but I was rushing as I got excited that it would work! Anyway the dispatch looked like this:</p>
<pre>
    this.props.shouldFetchData(['projects', 8, false, '&orderby=menu_order&order=desc'], ['pages', 10, false, '']);
</pre>
<p>My action creator was pretty basic</p>
<pre>
    export function shouldFetchData(...payload) {
    return  {
        type: 'FETCH_DATA',
        payload
    }
}
</pre>
<p>And finally here was the saga</p>
<pre>
    export function *shouldFetchDataAsync(action) {
    //get my current data from the store
    const posts = yield select(getPosts);
    const pages = yield select(getPages);
    const { payload } = action;



    let requests = [];


    for(let i=0; i <payload.length; i++) {
        requests[i] = yield call(axios.get, `${BASEURL}/wp/v2/${payload[i][0]}?per_page=${payload[i][1]}${payload[i][3]}`);
    }

    // payload.forEach( function *(request, i) {
    //
    //
    // });
    console.log('fff', requests);
    yield (axios.all(requests)
        .then(function(results) {
            console.log('results', results);
        }) );
}
</pre>

<p>I did the usual thing of getting my posts(and my pages this time), although in the above example I am not using that I will when I get round to finishing this. The next bit is a little bit tricky. I sent the payload to my saga using a spread operator(see ...payload) so it came out as a multi dimensional array(an array of arrays) which I looped over using a for loop. The for loop isnt as nice as a .map or a forEach but it was necessary as a <strong>yield can only be called within a generator function</strong>. i.e. it cannot be nested within another function.</p>
<p>Each iteration of the loop yields a new request and stores the result in an array I made called requests slightly above.</p>
<p>Finally I had a quick glance through the excellent <a href="https://github.com/mzabriskie/axios" target="_blank" rel="noopened">Axios documentation</a> and found that it has a function called 'all' which allows you to pass it an array of requests(hence the array I just mentioned) and it will return a final promise only when every single promise in that array has either completed or failed. I am just console logging the results at the moment and I will add to this as I update it so we can see how the items made their way to the store!</p>
