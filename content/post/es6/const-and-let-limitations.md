+++
date = "2017-09-10T21:44:08+02:00"
tags = []
title = "ES6 - Limitations of using const and let"
description = "One of the main things that we need to understand about const and let: is that const, is a constant, so, it's not going to change and let is something that might change, it could change" 
+++

As I mentioned at my previous article, one of the main things that we need to understand about const and let: 
is that const, is a constant, so, it's not going to change and let is something that might change, it could change. 
 
Let's explore a little bit what that means because this is actually a gotcha in some cases. 
So, be careful, const isn't actually an immutable variable, it can be changed.
 
Let's look at some code to see what happens. 

On the below example, I'm going to declare a constant and I'm going to call `canIChangeThis` and I'm just going to make 
it a string and set is to `"What is this variable?"`

And then, in my console, I'm going to log it and then, beneath it, so I'm going to change it.

```javascript
const canIChangeThis = "What is this variable?";
console.log(canIchangeThis);

canIChangeThis = "Did I change this?";
console.log(canIChangeThis)
```
And then, I'm going to console.log the variable once again to see if I was, in fact, able to change this.
We can go to the browser to test. 

Here, as we can see on the below image this will give us an error.
 
![](/images/posts/es6/const-and-let-limitations/const-error.png)

When I look at it, it says, *`Syntax error: canIChangeThis is read-only`*. 

Okay, cool, so, in this way, constant did act as a constant. It was a string and then, when we tried to reassign it, 
it gave us an error, it said: you can't do that. 

But, let's look at something else. As I mentioned, const is not immutable.

## Changing the value of a const variable

That means, if I make const an array, let's say const toppings equals "sprinkles" and "strawberries" and then, I try and 
push something into it using for example the 
[array.push](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/push) method: `toppings.push`, 
I can add a value to my array.
 
For this example I'm going to try add the "lemon pie" value to the array.

```javascript
const toppings = ["sprinkles", "strawberries"];

toppings.push("lemon pie");

console.log(toppings)
```

I actually already have this written here. 

So, once I save my file and go to my browser, I could see, at below image, my const actually has been modified.

![](/images/posts/es6/const-and-let-limitations/const-array-mutable.png)

So, it was an array and I added "lemon pie" to it. So, that's why you shouldn't think of const variables as being 
unchangeable because they, in fact, can be changed. 

## Making a const variable really immutable with the Object.freze method 

One thing you can think about, there is a method if an immutable variable is what you're looking for, 
we can use [Object.freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze). 

Now, this isn't actually supported in all browsers and is considered experimental syntax. 
So, you should be careful when using the `Object.freeze` method because, if you're not compiling with the right version of Babel, 
it might not work.

But if it is available to you, like is for me, wa can test your use, you can see that it actually is going to freeze 
our object so that we can't push to it. 

```javascript
const toppings = ["sprinkles", "strawberries", "lemon pie"];

Object.freeze(toppings);

toppings.push('raisins');
```

On the above example, the `Object.freeze` method  makes our constant variable act a little bit more like we'd imagine it would.
 
To test this, we can save this and go to our browser and we will see that we'll get an error:

![](/images/posts/es6/const-and-let-limitations/object-freeze-error.png)
 
So, we were able to change it before we freezed the object. But now, we can see we do get an error because 
the object is not extensible. So, that's one solution if you want to protect your constants. 

---

The examples of this article and project that I used to run them and other examples are available on my GitHub 
[repository](https://github.com/coderade/es6-examples).
