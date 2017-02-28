---
layout:     post
title:      "The saga of Sagas"
subtitle:   "Category: React"
date:       2017-02-28 09:00:00
author:     "Tom Hanson"
tags:       "JavaScript, &nbsp;React"
categories: "React"
header-img: "img/sagas.jpg"
---

<p>Let me start you off with a bit of history about this one...</p>
<p>I started making a small portfolio site for myself using React. It is going to be buzzword central when its finished with things such as React, Redux, Redux Sagas etc... but I figured what the hell, you only live once!
My first pass at this was to just simply call Axios in componentDidMount. I can't see any reason why this is inherently wrong if you were just adding a tiny component inside a pre build website but as previously mentioned I am using Redux 
so even though this actually did work, something rang an alarm bell for me that this was wrong.
</p>
<p>After thinking about this for a while I remembered something Wes Bos said in one of his Redux videos about using something called middleware and his recommendations were Redux Thunk &amp; Redux Sagas. This was a good starting point so I did some googling and Redux Saga seemed to come out on top based on the fact that it was using JavaScript generators(fancy new style JS), so I went for it!</p>
<p>You can find the docs <a href="https://redux-saga.github.io/redux-saga/docs/ExternalResources.html" target="_blank" rel="noopener">here</a> and although they are incredibly in depth I did struggle with quite a bit of unfamiliar syntax to me</p>
<p>At this point I was getting nowhere fast until I stumbled across <a href="https://www.youtube.com/watch?v=msx0Qiu8NxQ" target="_blank" rel="noopener">this</a>tutorial by a guy called Dave Kiss. It really helped me grasp some of the basics and after a brief twitter exchange for him to show me a few more niche things I ended up with a working saga! Success. </p>
<p>I ended up with code looking like this:</p>
<pre>
   const getPosts = state => state.posts;
    export function *shouldFetchPostAsync(action) {
    try {
        //get my current posts from store
        const posts = yield select(getPosts);
        let response;
        //check if the post slug I am requested is in the store already
        if(posts.length) {
            response = posts.filter( (item) => {
               if(item.slug === action.postID) {
                   return item;
               }
               return false;
            });
            yield put({type: 'TOGGLE_LOADER', response})
        } else {
            response = yield call(axios.get, `${BASEURL}/wp/v2/projects?slug=${ action.postID }`);
            function arrayUnique(array) {
                let a = array.concat();
                for(let i=0; i<a.length; ++i) {
                    for(let j=i+1; j<a.length; ++j) {
                        if(a[i] === a[j])
                            a.splice(j--, 1);
                    }
                }
                return a;
            }
            // concatenate both arrays and then filter them for only unique ones
            response.data = arrayUnique(posts.concat(response.data));

            yield put({type: 'FETCH_POSTS_SUCCEEDED', response})
        }

    } catch(e) {
        console.log('error', e);
    }
}

export function *watchshouldFetchPost() {
    yield takeEvery('SHOULD_FETCH_POST', shouldFetchPostAsync)
}

export default function *rootSaga() {
    yield [
        watchshouldFetchPost()
    ]
}
</pre>

<p>The tutorial I mentioned will do a way better job than I can explaining the guts of this but there are a couple of things I would like to mention</p>
<p>Firstly, the following line:
<pre>
    const getPosts = state => state.posts;
</pre>
will actually check my store and grab my posts. That is so I can run a small check to see if I have any posts yet. If I do then I will just find that post and return it, If I dont then I will run the http request.
This is basically just a buffer to prevent requests every time the components load.</p>
<p>My thinking behind this is that from a list of posts you cannot get to the individual post without clicking its link(and the posts data will therefore already have been requested) and if you do land straight on the page there will be no posts so I will go and fetch that one and put it in my store</p>
<p>the second part is this:
<pre>
    function arrayUnique(array) {
        let a = array.concat();
        for(let i=0; i<a.length; ++i) {
            for(let j=i+1; j<a.length; ++j) {
                if(a[i] === a[j])
                    a.splice(j--, 1);
            }
        }
        return a;
    }
</pre>
This basically takes an array and checks no posts are the same in it. Just so I never have any duplicate posts in my store - though as I look at it now I am wondering if I will ever need that so that may go!
</p>
<p>Anyway, thats about it for sagas for now. I will hopefully do an updated post when I actually know a bit more but for now it is working and I soret of understand why!</p>
<p>back to learning!</p>
