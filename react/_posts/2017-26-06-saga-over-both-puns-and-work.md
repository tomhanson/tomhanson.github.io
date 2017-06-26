---
layout:     post
title:      "Saga Over - Both puns and work"
subtitle:   "Category: React"
date:       2017-06-26 12:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;React"
categories: "React"
header-img: "img/sagas.jpg"
---

<p>Its been a few months but I think its time to finish this saga of both (terrible) puns and work</p>
<p>So after all that excitment about multiple sagas what did I finally decide to do? Well...I decided not to use multiple sagas in one call.</p>
<p>The reason for this is down to one teeny tiny little thing that I overlooked: performance. Waiting for all my requests to complete before allowing for data 
meant potentially long waits with a loading screen. I kept a similar format with the arrays of data but I just split the requests up into seperate ones for projects and pages.</p>
<p>So here is how they ended up</p>

<pre>
    export function *shouldFetchPageAsync(action) {
        //get my current pages from store
        try {
            //get my current pages from the store
            const pages = yield select(getPages);
            //check to see if i have that page slug;
            const requestedPage = pages[action.slug] || undefined;
            let requestData;
            if( requestedPage === undefined ) {
                requestData = yield call(axios.get, `${BASEURL}/wp/v2/pages?slug=[${ action.slug }]`);
                yield put({
                    type: `FETCH_PAGE_SUCCEEDED`,
                    response: requestData,
                    toggleLoader: action.toggleLoader
                });
            } else {
                if(action.toggleLoader) yield put({type: 'TOGGLE_LOADER'});
            }
        } catch(e) {
            console.log('error in should fetch data async saga', e);
        }
    }
</pre>
<pre>
    export function *shouldFetchProjectsAsync(action) {
        try {
            //get my current data from the store
            const projects = yield select(getProjects);
            //destructure the two arrays passed from the action
            const { payload } = action;
            //multiple sagas
            let requestData;
            if(projects.length <= 7) {
                requestData = yield call(axios.get, `${BASEURL}/wp/v2/${payload[0]}${payload[1]}${payload[3]}`);
                yield put({
                    type: `FETCH_PROJECTS_SUCCEEDED`,
                    response: requestData
                });
            } else {
                yield put({type: 'TOGGLE_LOADER'});
            }
        } catch(e) {
            console.log('error in should fetch data async saga', e);
        }
    }
</pre>

<p>There are a few quirks in there that I had to add to help with my request handling. An example of this is counting for more than 7 posts. This was 
because if you landed straight onto an individual project page you were only get 1 project returned. If you then moved to the overall project page
the logic was checking for projects and not requesting more if there were any. I didn't want to get them all every time but similarly I didn't want
to be stuck with none! I'm sure there are numerous better ways to achieve this but this works for me for now so I left it as it is.</p>

<p>I really liked using sagas in the end but I just don't quite feel comfortable with them just yet. I think part of my problem is that they are not used that 
frequently in an application and I havn't forced myself to write more unless needed.</p>

<p>Having said the above about sagas I have been keeping an eye on redux observable, promising myself that I will learn about them(at least to compare them to thunks/sagas) 
and I feel like now is the time, I am going to watch <a href="https://www.youtube.com/watch?v=AslncyG8whg">this awesome video by Jay Phelps</a> and hopefully prgoressing 
from there!</p>

<p>Dealing with asynchronous data is a bit of an achilles heel for me due to a bit of a lack of knowledge in the area which makes it a bit of a frightening topic for me 
so I am going to try and jump in at the deep end and keep at them until I am much more comfortable. Expect some more content about that coming up pretty soon!(or maybe in a few months time!)