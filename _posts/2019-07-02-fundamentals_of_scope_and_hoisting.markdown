---
layout: post
title:      "Fundamentals of Scope and Hoisting"
date:       2019-07-03 01:21:11 +0000
permalink:  fundamentals_of_scope_and_hoisting
---


Scope and hoisting are two critical concepts in Javascript that any developer writing in the language needs to understand. Let's get started with scope, which defines our access to variables. When you create a variable outside of a function, in the root scope or global scope, which is in the window object. This variable will be accessible within any of your functions. When creating a variable in the global scope it is often referred to as the parent scope. When a variable is declared within a function, referred to as the child scope, it is only accessible from within that function. 

If a variable declared in global scope and a variable of the same name is declared within a function, the variable declared wtihin the function will be the one that is used. For example:

```
var a = 1;

function foo() {
  var a = 2;
}

foo();
```

Can you guess what this will return? If you guessed `2` you are correct, as the engine first looks within the inner scope to define the variable, it will only look to the outer scope if that variable is undefined within the inner scope. It is important to note, however, that one can change the value of a global variable from within a function by not declaring `var`, `let` or `const` and simply stating `a = 2`, this is sometimes referred to as 'polluting the root scope' and should be avoided. A way to protect yourself from this is to declare `use strict` so that your global scope cannot access the variable declared within a function and if it tries to get called from outside of the function, it will throw an uncaught reference error of variable is undefined. Let's see what this looks like in the code without using `use strict`:

```
var a = 1;

function foo() {
  var a = 2;
	console.log(a);
}

function bar() {
  a = 3;
	console.log(a);
}

foo();   => 2
bar();   => 3
console.log(a);   => 3
```
