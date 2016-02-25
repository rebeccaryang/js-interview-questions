# **JavaScript Interview Questions!**

## Disclaimer

Please keep in mind that interviewers don't expect you to have definitions memorized word for word! As long as you can explain the important points and use examples to show your understanding, it will be fine.

## Useful Links

A curated list of resources on interviews and job hunting is [**here**](https://github.com/mi-lee/js-interview-questions/blob/master/resources.md). Take a look!

## Table of Contents

1. [What is a closure?](#1-what-is-a-closure)
2. [What is the difference between apply, call, and bind?](#2-what-is-the-difference-between-apply-call-and-bind)
3. [What is event delegation?](#3-what-is-event-delegation)
4. [What is event bubbling?](#4-what-is-event-bubbling)
5. [What is hoisting and how does it work?](#5-what-is-hoisting-and-how-does-it-work)
6. [What does this console?](#6-what-does-this-console)
7. [What is the prototype chain?](#7-what-is-the-prototype-chain)
8. [What does this console?](#8a-what-does-this-console)
9. [Rapid-fire questions!](#9-rapid-fire-questions)
10. [What determines the value of ‘this’?](#10-what-determines-the-value-of-this)
11. [What is the event loop?](#11-what-is-the-event-loop)
12. [What is the output?](#12-what-is-the-output)
15. [Implement curry.](#15-implement-curry-such-that)

----------------

# 1. What is a closure?

- Access to local variables, parameters, variables in the parent scope, and global variables
- 'Remembers' the environment in which it was created
- Still has access to those variables even after the outer function has returned

MDN: A "closure" is an expression (typically a function) that can have free variables together with an environment that binds those variables (that "closes" the expression).

'A closure is formed when an inner function is made accessible outside of the function in which it was contained, so that it may be executed after the outer function has returned. At which point it still has access to the local variables, parameters and inner function declarations of its outer function.'

Source:

- [MDN Definition](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [Understanding Closures with Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)


# 2. What is the difference between apply, call, and bind?

- Bind allows us to set the `this` value on methods
  - Allows us to borrow methods
  - '`bind` allows us to easily set which specific object will be bound to this when a function or method is invoked.'
  - Bind returns a function, call & apply invoke immediately
- Apply allows us to execute a function with an array of parameters
  - each parameter is passed to the function individually
  - good for varying number of arguments
- Call is similar to bind, but takes parameters separated by commas

Source:

- [JS Is Sexy - JavaScript’s Apply, Call, and Bind Methods](http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/)
- [MDN Bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)




# 3. What is event delegation?


- Event delegation is a strategy where you attach your event handlers to a parent element rather than on multiple child elements
- Classic example: a list (i.e. `ul`) with multiple `li` children.
- If you want to attach some behavior for when the user clicks an `li`, you could  either:
  - 1) attach an event handler to each `li` specifically, or
  - 2) attach one event listener to the parent `ul` and determine which child element was clicked by inspecting the event object itself when it bubbles up.

This can simplify things quite a bit, especially when `li` elements are going to be added and removed dynamically. It can be a hassle to manually attach and remove all those individual handlers.

Source:

- [David Walsh - How JavaScript Event Delegation Works](https://davidwalsh.name/event-delegate)
- [Javascript Info - Event Delegation](http://javascript.info/tutorial/event-delegation)

# **4. What is event bubbling?**


- Event bubbling has to do with how events are propagated through elements in the DOM
- When you click on an element (an li for example), that element will receive the event and then, unless you explicitly stop propagation, the event will "bubble up" to its parent element (the ul), and so on
- After an event triggers on the deepest possible element, it then triggers on parents in nesting order.
- The bubbling guarantees that click on Div 3 will trigger onclick first on the innermost element 3 (also caled the target), then on the element 2, and the last will be element 1.
- The order is called a bubbling order, because an event bubbles from the innermost element up through parents, like a bubble of air in the water.
- deepest element that triggered the element is called the `event.target` or `event.srcElement`

![title](http://javascript.info/files/tutorial/browser/events/event-order-bubbling.gif)


Source:

- [JS Info - What is event bubbling and capturing?](http://javascript.info/tutorial/bubbling-and-capturing)
- [Stack Overflow - What is event bubbling and capturing?](https://stackoverflow.com/questions/4616694/what-is-event-bubbling-and-capturing)

# **5. What is hoisting and how does it work?**


[MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting):

- 'Hoisting is JavaScript's behavior of moving declarations to the top of a scope (the global scope or the current function scope).'
- Compiler reads the entire program to find which functions/variables you have used
- Only declarations are hoisted, not initializations


# 6. What does this console?

```js
var a = 1;

function b() {
	a = 10;
	return;

	function a() {}

}
b();
console.log(a)
```

### Answer:

```
1
```

- within function `b`, the inner function `a` is hoisted to the top of the scope, before `a` is re-assigned to a value of 10.
- because `a` is hoisted to the top of the scope (within `b`), the re-assignment of 10 does not change the global variable `a`.

# 7. What is the prototype chain?

- Each object has an internal property called prototype, which links to another object
- The prototype object has a prototype object of its own, and so on – this is referred to as the prototype chain
- If you follow an object’s prototype chain, you will eventually reach the core Object prototype whose prototype is null, signalling the end of the chain.

What is the prototype chain used for?

- When you request a property which the object does not contain, JavaScript will look down the prototype chain until it either finds the requested property, or until it reaches the end of the chain
- This behaviour is what allows us to create “classes”, and implement inheritance.

Source:

- [MDN - Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [JS Is Sexy - JavaScript Prototype in Plain Language](http://javascriptissexy.com/javascript-prototype-in-plain-detailed-language/)

# 8a What does this console?

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i)
  }, i);
}
```

### Answer:

Almost simultaneously,

```
5
5
5
5
5
```

`i` is 5 because the for loop has completed and exited by the time the callback in setTimeout is called. Since the value of `i` in each callback is not bound to a specific value of i, every callback returns the current value of `i`.

It seems simultaneous because setTimeout delay is set on 0, 1, 2, 3, and 4 ms.

# 8b What does this console?

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
  	console.log(i)
  }, i*500);
};
```

### Answer:

500ms apart each:

```
5
5
5
5
5
```

Similar answer as above, but the callback is now being called at 0ms, 500ms, 1s, 1.5s, and 2s.

# 8c How would you turn this code into its intended effect?

For example, how could you produce the output

```
0
1
2
3
4
```

with 500ms apart?

### One answer:

```js
for (var i = 0; i < 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 500);
  })(i);
}
```

Turn it into an IIFE and pass the value of `i` for each one.



# 9. Rapid-fire questions!

What is ...

1. `console.log(typeof [])` ?
2. `console.log(typeof arguments)`?
3. `console.log(typeof "hi")`?
4. `console.log(typeof new String('hi'))`
5. `console.log(typeof NaN)`?
6. `console.log(2+true)`?
7. `console.log('6'+9)`?
8. `console.log(4+3+2+"1")`?
9. `console.log(-'34'+10)`?
10. `var a = (2, 3, 5); console.log(a)`?
11. `console.log(3 instanceof Number)`?
12. `console.log(typeof null)`?



### 9. Answers

1. `console.log(typeof [])` ? `object`. Array is derived from Object.
2. `console.log(typeof arguments)`? `object`. arguments is an array-like object.
3. `console.log(typeof "hi")`? `string`. This is a string primitive.
4. `console.log(typeof new String('hi'))` ? `object`. Using `new String` constructor makes it an object.
5. `console.log(typeof NaN)`?  `number`. Sidenote: ES6 has a `Number.isNaN` function that will reliably test whether `x` is `NaN` or not, unlike the ES5 `isNaN` - see [Flaws of ES5 `isNaN`](http://ariya.ofilabs.com/2014/05/the-curious-case-of-javascript-nan.html). [Stack Overflow explanation why NaN is a type of number](https://stackoverflow.com/questions/2801601/why-does-typeof-nan-return-number).
6. `console.log(2+true)`? 3. true is coerced into 1.
7. `console.log('6'+9)`? `69`
8. `console.log(4+3+2+"1")`? `91`. Evaluated from left to right.
9. `console.log(-'34'+10)`? -24. -34 is coerced to -34, in the same way +`34` is coerced to 34.
10. `var a = (2, 3, 5); console.log(a)`? 5. The comma operator evaluates each of its operands (from left to right) and returns the value of the last operand. [Detailed explanation](https://javascriptweblog.wordpress.com/2011/04/04/the-javascript-comma-operator/)
11. `console.log(3 instanceof Number)`? `false`, because 3 is a primitive. `console.log(new Number(4) instanceof Number)` will return true.
12. `console.log(typeof null)`? 'object'.




# 10. What determines the value of 'this'?

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this):

- determined by how a function is called
- can't be set by assignment during execution


4 main patterns:

- global reference (`this` usually refers to global object)
- free function invocation (global object)
- call/apply (first argument to call/apply)
- method invocation (object on the left of dot, at **call time**)
- construction mode (new object created for that invocation)


See HR notes on summary of binding patterns for `this` (since it is copyrighted material I won't put it here)



# 11. What is the event loop?

- event loop runs in a single thread
- want to avoid blocking the whole thread
- non-blocking I/O - define a callback that is called once the action is complete
- incoming callbacks are like events that are propagated to the event loop, which checks for new events in the queue and processes them
- instead of waiting for the results, you can register callbacks that are executed when the event is triggered - so that you can deal with long-taking actions


I ***highly*** recommend, especially if you have difficulties understanding the event loop and questions like #8 and #14, to watch this video: **[What the heck is the event loop anyway?](http://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)**

Other:

- [Altitude Labs - What is the JavaScript Event Loop?](http://altitudelabs.com/blog/what-is-the-javascript-event-loop/)
- [MDN - Concurrency model and Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

# -- Questions taken from [JS By Examples](https://github.com/bmkmanoj/js-by-examples/tree/master/examples) --

# 12. What is the output?

```js
var name = "John";

(function(){
   console.log("The name is : " + name);

   var name = "Jane";

   console.log("The name is : " +name);
})();
```

## Answer:

```
The name is : undefined
The name is : Jane

```

Explanation in [JS By Examples](https://github.com/bmkmanoj/js-by-examples/blob/master/examples/function_scope_and_hoisting.md)

# 13. What is the output?

```js

var data = [0, 1, 2];
var funcs = [];

function init() { // 0
  for (var i = 0; i < 3; i++) {

    var x = data[i]; // 1
    var innerFunc = function() { // 2
      return x;
    };

    funcs.push(innerFunc); // 3
  }
}

function run() { // 4
  for (var i = 0; i < 3; i++) {
    console.log(data[i] + ", " + funcs[i]()); // 5
  }
}

init();
run();

```

## Answer:

```
0, 2
1, 2
2, 2
```

Detailed solution by [JS By Examples](https://github.com/bmkmanoj/js-by-examples/blob/master/examples/closures_in_loop.md).


# 14. What is the output?

```js
(function() {
  console.log(1);
  setTimeout(function() {
    console.log(2)
  }, 2000);
  setTimeout(function() {
    console.log(3)
  }, 0);
  console.log(4);
})();
```


Answer:

```
1
4
3
2
```

Solution by [JS Examples](https://github.com/bmkmanoj/js-by-examples/blob/master/examples/time_for_some_timeout.md)



# 15. Implement curry such that:

```js
var add = curry(function(a, b, c) {
  return a + b + c;
});

console.log(add(1)(2)(3));
console.log(add(1, 2)(3));
console.log(add(1)(2, 3));
console.log(add(1,2,3));
```



### One possible solution

This is just my own solution. Aside: [`func.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length) specifies the number of arguments expected by the function.

```js
var curry = function(func) {
  var totalNumArgs = func.length;

  return function curriedFunc() {
    var args = [].slice.call(arguments);
    if (args.length === totalNumArgs) {
      return func.apply(null, args);
    } else {
      return function () {
        var args2 = [].slice.call(arguments);
        return curriedFunc.apply(null, args.concat(args2));
      };
    }
  };
};
```


