---
layout: post
title: "Node.js, socket.io, jQuery, ntwitter, node-cron"
date: 2013-09-20 06:29
comments: true
categories: Programming 
---

The most commonly advertised strengths of [Node](http://nodejs.org/) are:

- **Performance**: Improved throughput and better latency in typical cases where the database queries are the limiting performance factor (not CPU).
- **Scalability**: Solving the C10K problem by using optimized OS-specific network interfaces (epoll/kqueue, IOCP).
- **Productivity**: The node module ecosystem will provide immediately usable solutions for most of your needs, allowing a very short time to market, or fast prototyping. Javascript as a server language is a debatable but pragmatic choice, since everyone already knows it, and JSON apis or communication with clients are made trivial.
- **Socket.io**: Undoubtedly one of the "killer features", allows for impressive realtime one-page web apps that can compete with regular native software, without the hassles of deployment or cross-platform compatibility (the browser takes care of that).


The one strength that draws me back to node regularly is the productivity boost. It would be possible to use alternatives like Python and [gevent](http://www.gevent.org/), with [gevent-websocket](http://www.gelens.org/code/gevent-websocket/), with a micro-framework like [Flask](http://flask.pocoo.org/). The parts feel more disconnected and pip is not as dead simple as npm, so deployment will always be harder than just "npm install", but Python is arguably a more comfortable language, and the coroutine approach has a shorter learning curve than node's callback nesting. People who like Ruby could probably argue that Ruby on Rails can achieve comparable results, despite falling somewhat out of fashion these last few years.

You can have an **idea**, and implement a **working prototype** in no time (whether the prototype can scale as advertised without careful planning is another more controversial story).

Imagine the following idea:

> I want a website where I can watch tweeted pictures of a city in realtime.

We'll take San Francisco for our example, but any location in the world is possible. 

## An example project: "World-o-vision"

I assume the reader has at least basic familiarity with Node, underscore.js, and socket.io.

The whole code is available from a [GitHub repository](https://github.com/prgreen/worldovision). Don't forget to "npm install" the missing modules if you intend to clone it, and fill in the relevant twitter account information.

We'll use [express](http://expressjs.com/) as our web application framework of choice, for convenience, although we only have one route (it's a one page web app).

We'll use [Twitter streaming API](https://dev.twitter.com/docs/streaming-apis) (version 1.1). The awesome [ntwitter](https://github.com/AvianFlu/ntwitter) node module makes it easier to use Twitter API from Node.

**You will need your own Twitter keys to make this work, since every account is limited to one connection.**


``` javascript app.js
//After node configuration...

var NPICS = 10;
var picList = [];

// routes
app.get('/', function(req, res) {
    res.render('index', { data: picList });
});

// socket.io server
var sio = io.listen(server);
 
sio.sockets.on('connection', function(socket) { 
    socket.emit('data', _.last(picList, NPICS));
});

// twitter streamer
var t = new twitter({
    consumer_key: 'USE_YOUR_OWN',           
    consumer_secret: 'USE_YOUR_OWN',        
    access_token_key: 'USE_YOUR_OWN', 
    access_token_secret: 'USE_YOUR_OWN'
});
 
t.stream('statuses/filter', { locations : '-122.75,36.8,-121.75,37.8' }, function(stream) {
  stream.on('data', function(tweet) {
    if (tweet.text !== undefined) {
        //console.log(tweet.text+'\n');
        if (tweet.entities != undefined && tweet.entities.media != undefined && tweet.entities.media[0].media_url != undefined) {
            picList.push(tweet.entities.media[0].media_url);
            console.log(picList.length);
            sio.sockets.emit('data', _.last(picList));
        }
      }
    });
});

```

The node server will connect to the Twitter streaming API, filter tweets by location (San Francisco in our example), add **all links to tweeted photos in a list**, and send either the last (more recent) 10 elements of the list (first update) or the individual elements (in subsequent realtime updates) to the client.

We limit ourselves to 10 pictures to avoid putting too much strain on the browser. On the server side though, we could save all photo links regularly to a database, if we wanted.

For now we'll use this problem as an excuse to use the lovely node-cron module, and we'll ask for the list to simply be emptied every 20 minutes:

``` javascript
new cronJob('0 */20 * * * *', function(){
    picList = [];
    }, null, true);
```

We can devise a simple design for a client:

- display 9 pictures as thumbnails in a row, clicking a thumbnail opens the picture in another tab or window, in its original size
- display one large picture underneath (the last received)

The client-side javascript can be quickly hacked together with a simple jade template:

``` javascript worldovision.js
$(function() {
    var socket = io.connect(window.location.hostname);

    var picList = [];
    var NBPICS = 8;

    function updatePics() {
      $('#pictures').html('');
      for (var i=0 ; i < picList.length-1 ; i++) {
        $('#pictures').append('<a href="' + picList[i] + ':large" target="_blank"><img src="' + picList[i] + ':thumb"></a>');
      }
      $('#big-picture').html('<img src="' + picList[picList.length-1] + ':large">');
    }

    socket.on('data', function(data) {
        if(data instanceof Array) {
            picList = data;
            picList.splice(0, picList.length - NBPICS);
        } else {
            if (picList.length >= NBPICS) {
                picList.splice(0, 1); //remove first element
            }
            picList.push(data);
        }
       updatePics();
    });
});
```

``` jade index.jade
extends layout

block content
  #pictures
  #big-picture
```


Although jQuery makes DOM manipulation easy and solves our problem, it's worth noting that AngularJS components are usually the best long-term solution with a more correct level of abstraction.

The result can be observed [here](http://tty.mooo.com:3456).

All the photos are realtime tweeted pictures from San Francisco!

All that can be coded and presented as a prototype, in less time than it took me to write this article!

## Possibilities for future evolutions

- We could add more interaction with the users by asking them to mark their favorite pictures. At the end of every day, we select the most liked pictures, more deserving of representing the city they originate from.

- We could offer the use a choice of cities. Wonder what people are eating in Paris? Connect to World-o-vision at the right time of the day, and you'll know!

- We could display the average time between photos. The activity is a lot higher during the day. During the night the same photos will stay for a long time.

- Elect the most attractive city, based on how liked the tweeted pictures from each city are.

- Have various sources of photos to inject in our application, for example using the Instagram API.
