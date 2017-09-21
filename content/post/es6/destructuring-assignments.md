+++
date = "2017-09-17T21:44:08+02:00"
tags = []
title = "ES6 Destructuring assignments"
description= "Article that explains how to use Destructuring assignments in ES6"
+++

In this article I'am going to talk about Destructuring assignments in ES6. 

The Destructuring Assignments don't really offer us any new features, but rather they enable us to write more readable 
and less repetitive code. 

So, have you ever had a function that was dealing with these enormous deeply nested objects and it felt like your 
code was kind of sprawling out of control, it was getting hard to read? Yes? I think that almost all of us deal with this 
on a daily coding .

Well that's what destructuring assignments help us deal with. 

Let's take a look in some [examples](https://github.com/coderade/es6-examples/blob/master/src/examples/destructuring.js) 
to understand better how we can improve our code with ES6 feature.


```javascript
const event = {    //the object we will destructure
        name: 'Rock in Rio',
        stage: 'Trident Stage',
        artist: "Guns n Roses",
        songs: {
            firstSong: 'Sweet Child on mine',
            lastSong: 'Paradise City'
        }
    }
```

On the above code snippet, I've got a object that I've declared called "event" and we're going to try destructure it. 

Take a look on the following example:

```javascript
let {    
  name, 
  artist
} = event; 
```

To destructure an object we need to declare a new variable and then choose any number of keys to create into stand alone 
variables. We also need to specify the object that we're destructuring.

You can try to console log the value of the created variables:

```javascript
console.log('name = ', name);

console.log('artist = ', artist)
```
And now let's go to our browser and see what happens.

![](/images/posts/es6/destructuring-assignments/destructuring.png)

So, on the console you can see that I was able to access those two variables as their own variable instead of a 
`blank.blank` syntax key value pair of that larger object. 

Okay so that's the most basic way that we can access those variables using destructuring assignment, but we can also do 
some even cooler stuff.

## Destructuring nested variables


Let's say we want to get a deeply nested variable, well we can actually go to that by using the colon (,) so we can say 
`let stage, songs` and then we can find the `lastSong` within songs. 

If we look on the first example, we can see that `firstSong` is nested within songs, and that'll let us declare `firstSong` 
as it's own variable, and we can also rename the variables that we're destructuring.

```javascript
let {stage, songs: {firstSong}} = event;
let {lastSong} = event.songs; 
```

Then, let's say we want to get `artist` out of event but we already have a `artist` variable somewhere in our code, 
so we can rename it by using this colon, so we can say okay from now on: let artist be known as a favorite:

```javascript
let {artist: favorite} = event;
```
I encourage you to mess around with these objects and run them in your browser and see how destructuring works so you 
really get your hands on the idea.

```javascript
console.log('stage: ', stage);           //"Trident Stage"
console.log('first Song: ', firstSong);  //"Sweet Child on mine"
console.log('last Song: ',  lastSong);   //"Paradise City"
console.log('favorite: ', favorite);     //"Guns n Roses"
```

## Destructuring arrays

You also should know that you can use destructuring with arrays.
 
If I have an array called `festivals` and in it 
I've named all these mega festivals and I wanted to structure that, you can use almost the same syntax as object 
destructuring but with an array.

```javascript
const festivals = ['Coachella', 'Rock in Rio', 'Tomorrowland', 'Ultra Music Festival'];
```
 
Then, I can create a new variable that holds the index of these objects and I can skip over ones that I don't want to access, 
and then later on in my code.
    
```javascript
let [eventIdx0, , , eventIdx3] = festivals;
```

So, I can access those variables as these variable names, Rather than use: `festivals[0]` as we normally do with arrays:

```
console.log(eventIdx0);    //"Coachella"
console.log(eventIdx3);    //"Ultra Music Festival"
```

On the above example instead of having to do `festivals[3]` I can just say `eventIdx3` to get the `"Ultra Music Festival"` 
with the variable I've assigned it to. 

---


The examples of this article and project that I used to run them and other examples are available on my GitHub 
[repository](https://github.com/coderade/es6-examples).


