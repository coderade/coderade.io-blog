+++
date = "2017-09-14T21:44:08+02:00"
tags = ["es6", "web", "js"]
menu: "main"
title = "ES6 Arrow Functions"
description = "Arrow functions are another notable addition that came with ES5 and are available here in ES2016."
+++

Arrow functions are another notable addition that came with ES5 and are available here in ES2016. 
They are useful for two essential reasons: the first is that they reduce boilerplate and are more succinct and 
the second thing is that they allow us to preserve the context of the wrapping function. 

As I write before, the arrow function, one of its main features is that it's more succinct, it reduces boilerplate. 
Let's look at an example:

```javascript
function withACallBack(options, callback) {
  callback(options)
}
```
 
I've created a function and it's called `function withACallBack` and it accepts two arguments, `options` and `callback` and 
then inside of it, it just calls the `callback` on the `options`. 

Let's see three examples where we call this function now. 

```javascript
withACallBack('so long', function(options) {
    return options
  }
)
```
   
On the above example, I call withACallBack, say "so long" as the option and then I just declare an anonymous function 
and insert it here as the second argument and in this case, I am doing it the old fashioned way, I'm saying function,
options and then returning our options.

On the next example, I'm using an arrow function. You can see already that it's a bit more concise, we didn't have to write 
functions but fundamentally these are pretty much the same thing.

```javascript
withACallBack('a little shorter', (options) => {
        return options
    }
)
```

Look on the below example: 

```javascript
withACallBack('very short', options => options)
```

we have an even more concise option. We're calling a function with options as the argument and we're returning the options. 
This function on this example and that of second example do almost exactly the same thing but the last one is a lot more concise. 

Let's move on and see the way that arrow functions preserve this. 

On the below example, I created an object:

```javascript
const store = {
  address: '7th Ave, New York, NY, USA',
  what: function() {
    return this.address
  },
  arrow: () => {
    return this.address
  }

}
```

I'm calling this object store and then inside of it, I'm giving the object a key value pair of address and 
"7th Ave, New York, NY, USA" and then I'm giving it two other key value pairs that are both functions.

One is a traditional function and the other is an arrow function, because the arrow function preserves the context of 
the wrapping function, this address is actually going to end up being undefined, because it's looking at the `this` of 
our wrapping function, which is this `arrow` function, where as the `this` of the traditional function is going to look 
at the `this` of store.


So, don't just take my word for it, let's console log these and we'll go into the browser and see what happens. 
I'll save my file and I'll navigate to the browser and I'll refresh the page. 

![](/images/posts/es6/arrow-functions/undefined-error.png)


As we can see on the above image I'm getting an error in our console and it says that cannot read property address of undefined. 
That's because inside the arrow function, we actually don't have this address.

When we look in the example, we see that this address exists in store but it doesn't exist in the larger wrapping 
function of arrowFunctions. 

I think that this article can show how you can use the arrow function in some different ways, if you want to understand more 
about the arrows functions, we can use the [Arrow_functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 
reference doc on the JS Mozilla official [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

---

The examples of this article and project that I used to run them and other examples are available on my GitHub 
[repository](https://github.com/coderade/es6-examples).
