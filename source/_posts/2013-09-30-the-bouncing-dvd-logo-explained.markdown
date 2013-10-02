---
layout: post
title: "The bouncing DVD logo explained"
date: 2013-09-30 01:25
comments: true
categories: Math 
---

Many people seem to experience great satisfaction watching their DVD player screensaver on their TV, looking forward to the moment when the logo hits a corner perfectly.

I don't judge, because it's actually fascinating, from a mathematical point of view.

It was mentioned in a cold opening from The Office, and is still a great pastime when you're under the influence.

<iframe width="420" height="315" src="//www.youtube.com/embed/iI-RY8ducRc" frameborder="0" allowfullscreen></iframe>

## The math behind the greatest mystery of all time

We'll make a few reasonable assumptions:

- The TV screen is a W by H rectangle, the DVD logo is a w by h rectangle, with

$$
0 < w < W \\
0 < h < H
$$

- The DVD logo moves with a perfectly diagonal constant speed (same absolute speed on both axes).

- It bounces off the edges of the screen perfectly. 

- Sometimes the DVD logo image and its collision rectangle aren't perfectly aligned, but this is irrelevant to our discussion, just choose the appropriate collision shapes in your calculations.


Now we can start our investigations.


I leave the demonstration to the reader (simple high-school level(?) math, modular arithmetic, least common multiple and greatest common divisor, and using Bézout's identity to solve a simple Diophantine equation).


{% img center /images/dvdlogo.png %}

*__x__ and __y__ are the coordinates of the starting position.* 
*__w__ and __h__ are the dimensions of the logo.* 
*__W__ and __H__ are the dimensions of the screen.* 


We discover the following fascinating properties:

> When you hit a corner, you will hit a different corner before hitting the same one again, etc.

> You can never hit 1, 3 or 4 corners, always **2 or 0**.

> It's a loop, at some point it will always repeat.

> The traveled distance between corners is **lcm(W - w, H - h)**.

> The **condition for corners to be reached** is

$$
|x-y| \equiv 0 \mod \gcd(W-w, H-h)
$$

> When this condition is not met, no corner will be hit.

The corners hit **can be determined in advance**. The following Javascript code does it (once provided with lcm and gcd definitions).

L = Left 
R = Right 
B = Bottom 
T = Top

```javascript corner.js
var x = 0;
var y = 0;
var w = 90;
var h = 60;

var W = 400;
var H = 300;

var W0 = W - w; //the effective displacement
var H0 = H - h;

if (Math.abs(x-y) % gcd(W0, H0) == 0) {
    // corners will be reached
    if ((Math.abs(x-y) / gcd(W0, H0)) % 2 == 0) {
        console.log((lcm(W0,H0)/H0) % 2 == 0 ? 'T' : 'B');
        console.log((lcm(W0,H0)/W0) % 2 == 0 ? 'L' : 'R');
        
        console.log('T');
        console.log('L');
        
    } else {
        console.log((lcm(W0,H0)/H0) % 2 != 0 ? 'T' : 'B');
        console.log((lcm(W0,H0)/W0) % 2 != 0 ? 'L' : 'R');
        
        console.log('B');
        console.log('R');
    }
} else {
    // no corner will be hit
    console.log("No corner!");
}
```

For programmers out there, I made a simple HTML5 Canvas demo (might not work on some browsers) that you can integrate in an html file. If you're patient it will eventually hit a corner after a few minutes, then another one, then the same again, indefinitely. You can play with the variables, check the Firebug console for results, or replace the bouncing rectangle with a proper DVD logo.


```javascript dvdlogo.html
<canvas id="c">

<script type="text/javascript">
var x = 0;
var y = 0;
var vx = 1;
var vy = 1;
var w = 90;
var h = 60;

var W = 400;
var H = 300;

function gcd(a,b) {
    var temp;
    if(a < 0) {a = -a;};
    if(b < 0) {b = -b;};
    if(b > a) {temp = a; a = b; b = temp;};
    while (true) {
     a %= b;
     if(a == 0) {return b;};
     b %= a;
     if(b == 0) {return a;};
    };
    return b;
}
function lcm(a,b) {
    return Math.abs(a*b)/gcd(a,b);
}

var W0 = W - w;
var H0 = H - h;


console.log('W0 ' + W0);
console.log('H0 ' + H0); 
console.log('gcd ' + gcd(W0, H0));
console.log('lcm w h ' + lcm(W0, H0));

if (Math.abs(x-y) % gcd(W0, H0) == 0) {
    // corners will be reached
    if ((Math.abs(x-y) / gcd(W0, H0)) % 2 == 0) {
        console.log((lcm(W0,H0)/H0) % 2 == 0 ? 'T' : 'B');
        console.log((lcm(W0,H0)/W0) % 2 == 0 ? 'L' : 'R');
        
        console.log('T');
        console.log('L');
        
    } else {
        console.log((lcm(W0,H0)/H0) % 2 != 0 ? 'T' : 'B');
        console.log((lcm(W0,H0)/W0) % 2 != 0 ? 'L' : 'R');
        
        console.log('B');
        console.log('R');
    }
} else {
    console.log("No corner!");
}


function animate() {

    reqAnimFrame = window.mozRequestAnimationFrame    ||
                window.webkitRequestAnimationFrame ||
                window.msRequestAnimationFrame     ||
                window.oRequestAnimationFrame
                ;

    reqAnimFrame(animate);
    
    for (var i=0;i<1;i++) { // change if you want it to go faster
        x += vx;
        y += vy;
        if(x+w==W) vx = -vx;
        if(y+h==H) vy = -vy;
        if(x==0) vx = -vx;
        if(y==0) vy = -vy;
        

    }

    draw();
}


function draw() {
    var canvas  = document.getElementById("c");
    var context = canvas.getContext("2d");
    
    context.clearRect(0, 0, W, H);
    context.fillStyle = "#000000";
    context.fillRect(0, 0, W, H);
    context.fillStyle = "#00ff00";
    context.fillRect(x, y, w, h);
    //context.drawImage(img, x, y, w, h);
    
    // test whether we are in a corner
    if (x == 0 && y == 0) {
        context.clearRect(0, H, W, 200);
        context.fillText("TOP LEFT", 10, H+100);
    } else if (x == 0 && y + h == H) {
        context.clearRect(0, H, W, 200);
        context.fillText("BOTTOM LEFT", 10, H+100);
    } else if (x + w == W && y == 0) {
        context.clearRect(0, H, W, 200);
        context.fillText("TOP RIGHT", 10, H+100);
    } else if (x + w == W && y + h == H) {
        context.clearRect(0, H, W, 200);
        context.fillText("BOTTOM RIGHT", 10, H+100);
    }
}
var ctx = document.getElementById("c").getContext("2d");
ctx.canvas.width = W;
ctx.canvas.height = H+200;
ctx.font = 'italic 20pt Calibri';
//var img=document.createElement('image');
//img.src='http://www.otakia.com/wp-content/uploads/V_1/article_3565/7388.jpg';


animate();
</script>
```

