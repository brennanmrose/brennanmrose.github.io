---
layout: post
title:      "Fundamentals of Scope and Hoisting"
date:       2019-07-02 21:21:12 -0400
permalink:  fundamentals_of_scope_and_hoisting
---


Scope and hoisting are two critical concepts in Javascript that any JS developer needs to understand. Let's get started with scope, which defines our access to variables. When you create a variable outside of a function, in the root scope or global scope, this variable will be accessible within any of your functions. When creating a variable in the global scope it is often referred to as the parent or outer scope. When a variable is declared within a function, referred to as the child or inner scope, it is only accessible from within that function. 

If a variable is declared in global scope and a variable of the same name is declared within a function, the variable declared wtihin the function will be the one that is used. For example:

```
var a = 1;

function foo() {
  var a = 2;
}

foo();
```

Can you guess what this will return? If you guessed `2` you are correct, as the engine first looks within the inner scope to define the variable, it will only look to the outer scope if that variable is undefined within the inner scope. It is important to note, however, that one can reassign the value of a global variable from within a function by not declaring `var`, `let` or `const` and simply stating `a = 2`, this is sometimes referred to as 'polluting the root scope' and should be avoided. A way to protect yourself from this is to declare `use strict` so that your global scope cannot access the variable declared within a function and will throw an uncaught reference error of variable is undefined if that occurs. Let's see what this looks like in the code without using `use strict`:

```
var m = 10;

function foo() {
  var m = 20;
	console.log(a);
}

function bar() {
  m = 30;
	console.log(a);
}

foo();   => 20
bar();   => 30
console.log(a);   => 30
```

Now let's dive into hoisting. Simply put, hoisting means that all declarations (not initializations) are moved to the top of the current scope, be it global or within a function. Initialization is when a value is assigned to a variable and declaration is when the variable is declared. One good practice is to put all variable declarations at the top of your code, because it's happening behind the scenes already, it's helpful for the developer to visualize it as well. An important take away is to remember when you are calling your variable or function, make sure you have initialized it first because even though the declaration has been moved to the top, the value assigned to it has not and you will encounter an `undefined` return if you call it before this point. Define your variables before you need to use them.

Functions (AKA function declarations) are also hoisted to the top of your code, meaning you can call a function before it's written in your code because it has already been hoisted. That said, you have a function assigned to a variable, that function will not be hoisted, only the variable declaration will be. For example:

```
functionFoo();

var functionFoo = function() {
  console.log('this function has been run');  
}
```
will throw `Uncaught TypeError: functionFoo is not a function`, because the initialization (in this case assigning a function to the variable) has not been hoisted, but simply `var functionFoo`. 

