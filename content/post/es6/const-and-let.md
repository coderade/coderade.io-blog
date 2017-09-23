+++
date = "2017-09-04T21:44:08+02:00"
tags = ["es6", "web", "js"]
title = "var, const and let in ES6, when I use? They can be hoisted?"
description = "Perhaps the most recognizable addition to the JavaScript language was that of const and let. Two new key words that allow us to declare variables, which behave differently than var. "
+++

Perhaps the most recognizable addition to the JavaScript language was that of const and let. 
Two new key words that allow us to declare variables, which behave differently than var. 

When choosing which JavaScript variable to use, there are a number of factors we have to weigh. 
We have var, const, and let available to us, and we need to think what is the scope of each of these variables?
That means, what can it be called? And can these variables be re-declared? 
That means once you've called that variable, once you've created that variable already, can you create it again? 
We also need to think about, can this variable be reassigned? That means, once the variable's been created, 
can you change it's value to something else? 

And last, we need to know can this variable be hoisted? 
[Hoisting](https://www.w3schools.com/js/js_hoisting.asp) is something that happens to our code where variables at the 
bottom of the file are actually available to us at the top.

So, between var, const, and let: some of them are hoisted and some of them are not. 

![](/images/posts/es6/const-and-let/const-and-let-chart.png)

You can see on above chart that is marked some of the properties of var, const, and let,
but just looking at this isn't actually that helpful. 

So, in this post I try show how var, const, and let each work, using the 
[create-react-app](https://github.com/facebookincubator/create-react-app) to run my examples.

## Hoisting these type of variables

To understand better how each of these variables works, let's try do some hoisting with var, const and 
let.

### **Trying hoisting using var**

First, let's see how var works. 

On the below example I'm saying `iceCream = "watermelon"` and then I'm console logging 
the word hoisted and the value of iceCream:

```javascript
iceCream = "watermelon";
console.log("hoisted?", iceCream);

var iceCream = "chocolate";
console.log("declared", iceCream);
```

and then below it, I'm declaring the `var iceCream`, and calling it `"chocolate"`, and then I'm console logging declared 
and the iceCream value. 

So, lets see how this behaves.

When I go to the my browser, I can see that in the terminal it says `hoisted?, watermelon` and `declared chocolate`.

![](/images/posts/es6/const-and-let/const-and-let-term.png)

So, when I compare this to the example, I can see that we actually were able to access `iceCream` before it was declared and 
we can see that it took this variable of `"watermelon"`and console logged it, but then also let us reassign 
the variable to `"chocolate"`and then console logged that. 

As we can see in our console, my variable `iceCream` has two values, first `watermelon` and the `chocolate`. 
This is an example of both hoisting and reassignment, so we can see that our variable got hoisted and then also got reassigned.


But, we can also see on the terminal (and also on the browser console), the create-react-app tool linter has actually 
raised some warnings:
 
![](/images/posts/es6/const-and-let/const-and-let-warn.png)
 

It says: *`'iceCream' was used before it was defined`*. So, even though we can do that, even though var lets us do that, 
create-react-app's default linter has said:
 
> "That's probably not a good idea, it's kind of confusing." 

So, we can really use this in our application, but you aren't doing the recommended and don't following a good practice.

To read more about the `no-use-before-define` rule, please use your official 
ESLint [link](https://eslint.org/docs/rules/no-use-before-define).

### **Trying hoisting using const**

So, lets see what happens when we try and do the same thing that we did with var, with const. 

```javascript
gelato = "mango";
console.log("hoisted?", gelato);

const gelato = "lemon";
console.log("declared", gelato);
```

If I try to save my file, right away the React will give me an error:

![](/images/posts/es6/const-and-let/const-error.png)

This is because `const` cannot be reassigned before it's declared, it is not hoisted.
 
So, we can see rather than just getting a warning, like, that's not a good practice, we're actually getting an error, 
you cannot do that.


### **Trying hoisting using let**

Lets try it now with let. Here, we've got a variable called froYo and we're assigning it before declaring it.

```javascript
froYo = "brownie";
console.log("hoisted?", froYo);

let froYo = "cherry";
console.log("declared", froYo);
```

This example is saying `froYo = "brownie"` and then we're also declaring it below and we're going to say, lets make it cherry. 
So after I saved this file I'll go back to my browser.

Interestingly, we see that we're not getting an error here but the same warning for the hoisting with var example:

![](/images/posts/es6/const-and-let/let-warn.png)

So it's allowed us to reassign a let before it was declared and it's also hoisted the variable, just like with var.     


Now, remember that on the chart on the begin of the artivle said that const and let, that neither of them were hoisted. 
Well, it seems like let got hoisted here. 

That's because I'm testing inside a function that's being run inside a module, 
and the `create-react-app` tool does some sort of fancy things with the way our files are formatted, 
so it's actually going to let us hoist the `froYo` variable.

---

Now when we've gotten the chance to look at the way const, let and var interact with each other when we try 
and reassign them, or hoist them.
 
As you can see, there's a lot of intricate details between when to use var, const and let, so I'm going to go over a 
few more of them in the next articles on this blog. 

But I'd really like to encourage you to take some time on your own to try uncommenting out different parts of the code 
use on this example, that is available on your GitHub 
[link](https://github.com/coderade/es6-examples/blob/const-and-let/src/examples/constAndLet.js) 
to see how var, const and let interact with each other.

## So, when I use var, const and let? 

One thing I want to draw your attention to though, if this starts to get a little complicated is 
that you can think about it this way; in general, const means this is a variable that will not change.

You should use `const` when you want to make a variable that you don't intend to change. 

`let`, you should use when you have a variable that you do intend to change.

So, if this is going to get reassigned, or if it's a variable inside of a function, then it's probably 
not going to be needed elsewhere and as with older versions of JavaScript, var will always be 
available to you, but I think you should be very careful when using var, now that we have const and let. 

`var` is notorious for being both globally and locally scoped, which means that once you declare a var variable, 
it's going to be available throughout your entire file and this can sometimes create some strange interactions, 
whereas `const` and `let` are just scoped in the block in which they're declared.

This all gets a little bit difficult, but in general, I think it's good practice to use const and let, 
unless you have a real reason to use var and you understand the ramifications of it's scope.

---

The examples of this article and project that I used to run them and other examples are available on my GitHub 
[repository](https://github.com/coderade/es6-examples).
