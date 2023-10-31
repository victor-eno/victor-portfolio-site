---
title: "JavaScript Objects"
description: "As we know there are seven data types in JavaScript. Six of them are called “primitive”, because their values contain only a single thing (be it a string or a number or whatever).
In contrast, objects are used to store keyed collections of various data and more complex entities"
tags: ["web components", "ssr"]
pubDate: "2023-10-08"
author: "David"
fullPost: "Read more"
---

# Objects: the basics Objects
As we know there are seven data types in JavaScript. Six of them are called “primitive”, because their values contain only a single thing (be it a string or a number or whatever).
In contrast, objects are used to store keyed collections of various data and more complex entities.

 In JavaScript, objects penetrate almost every aspect of the language. So we must understand them first before going in-depth anywhere else.

A property is a “key: value” pair, where key is a string (also called a “property name”), and value can be anything.

# Create an empty object
- Using the javaScript object constructor syntax 

```js
let player = new Object();
```

- Using the object literal syntax. Usually, the figure brackets {...} are used. That declaration is called an object literal.

```js
let player = {

};
```
Both wil have the same output

```js
console.log(player);
console.log(`${typeof player}`);

// output
{}
object
```
Literals and properties
We can immediately put some properties into {...} as “key: value” pairs:

```js
let player = { 
    name: 'jin', 
    score: 23 
    }
```
A property has a key (also known as “name” or “identifier”) before the colon ":" and a value to the right of it.
In the player object, there are two properties:
1. The first property has the name   and the value "jin" .
2. The second one has the name   and the value 23.

- Add a boolean value
```js
player.logIn = true;

console.log(player);

// output
{ name: 'jin', score: 23, logIn: true }
```
delete a property
```js
delete player.score;

console.log(player);

// output 
{ name: 'jin', logIn: true }
```

Multiword can be add using quote. For multiword properties, the dot access doesn’t work: That’s because the dot requires the key to be a valid variable identifier. That is: no spaces and other limitations.
There’s an alternative “square bracket notation” that works with any string:

```js
let player = {};

console.log(player);
console.log(`${typeof player}`);

player = { 
    name: 'jin', 
    score: 23,
    "current level": 2
    }

player.logIn = true;
delete player.score; 
 player.previousLevel = 5;

console.log(player);

// output
{ name: 'jin', 'current level': 5, logIn: true }
```

Square brackets also provide a way to obtain the property name as the result of any expression – as opposed to a literal string – like from a variable as follows:

```js

 let stage = "current level";
 player[stage] = 10;


console.log(player);

// output 
{ name: 'jin', 'current level': 10, logIn: true }
```

Here, the variable key may be calculated at run-time or depend on the user input. And then we use it to access the property. That gives us a great deal of flexibility.

# Property value shorthand
In real code we often use existing variables as values for property names. For instance:
```js
function newPlayer(name, age){
    return {
        name: "name",
        age: age
    }
}

let player1 = newPlayer("jim", 23);
console.log(player1);
console.log(`${typeof player1}`);

// output
{ name: 'name', age: 23 }
object
```

In the example above, properties have the same names as variables. The use-case of making a property from a variable is so common, that there’s a special property value shorthand to make it shorter.
Instead of name:name we can just write name , like this:
```js
function newPlayer(name, age){
    return {
        name,
        age
    }
}

let player1 = newPlayer("jim", 23);
console.log(player1);
console.log(`${typeof player1}`);

// output
{ name: 'jim', age: 23 }
object
```

# Checking for the existence of a property
A notable objects feature is that it’s possible to access any property. There will be no error if the property doesn’t exist! Accessing a non-existing property just returns undefined . It provides a very common way to test whether the property exists – to get it and compare vs undefined:
- using the **hasOwnProperty()** method
```js
let player = {};

if (player.hasOwnProperty("score")) {
    console.log(player.score);
} else {
    console.log("no property");
}

// output
no such property
```
- use the `in` operator

```js
let player = {};

if ("score" in player) {
    console.log(player.score);
} else {
    console.log("no property");
}

// output
no such property
```
Please note that on the left side of in there must be a property name. That’s usually a quoted string. If we omit quotes, that would mean a variable containing the actual name will be tested. For instance:

```js
let player = {
    score: 23
};

let key = "score"; 

if (key in player) {
    console.log(key);
} else {
    console.log("no property");
}

// output
score
```

# The “for...in” loop
To walk over all keys of an object, there exists a special form of the loop: for..in . This is a completely different thing from the for(;;) construct.