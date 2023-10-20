---
title: "Understanding JavaScript Object Inheritance"
description: "JavaScript objects have a link to another object, known as the prototype, from which they inherit properties and methods. Since prototypes are objects and have their own prototype, objects form an inheritance chain that allows complex features to be defined once and used consistently"
tags: ["lorem", "ipsum"]
pubDate: "2022-07-18"
author: "Monica"
fullPost: "Read more"
---


JavaScript objects have a link to another object, known as the prototype, from which they inherit properties and methods. Since prototypes are objects and have their own prototype, objects form an inheritance chain that allows complex features to be defined once and used consistently.

```js

let bookOne = {
    title: "Seven Seals",
    author: "Jin",
    peopleRating: 3,
    boxRating() {
        return Number(this.peopleRating) * 2;
    }
}
console.log(`Seven Seals by: ${bookOne.author}, ${bookOne.boxRating()}`);
console.log(`toString: ${bookOne.toString()}`);
```
The first console.log statement receives a template string that includes the rating property, which is one of the bookOne object’s own properties. The new statement invokes the toString method. None of the bookOne object’s own properties is named toString, so the JavaScript runtime turns to the bookOne object’s prototype, which is Object and which does provide a property named toString

# Inspecting and Modifying an Object’s Prototype
**Object** is the prototype for most objects, but it also provides methods that are used directly, rather than through inheritance, and that can be used to get information about prototypes



|  Method                           |         Description                          |
| --------------------------------  | -------------------------------------------- |
| getPrototypeOf                    |  This method returns an object’s prototype   |
| setPrototypeOf                    |  set the protyoe an object’s prototype       |
| getOwnPropertyNames               |  returns the names of an object’s own properties|


```js

let bookOne = {
    title: "Seven Seas",
    author: "Jin",
    peopleRating: 3,
    boxRating() {
        return Number(this.peopleRating) * 2;
    }
};

let bookTwo = {
    title: "Black nights",
    author: "Runnie",
    peopleRating: 5,
    boxRating() {
        return Number(this.peopleRating) * 2
 }
}

let bookOnePrototype = Object.getPrototypeOf(bookOne); 
console.log(`bookOne Prototype: ${bookOnePrototype}`);

let bookTwoPrototype = Object.getPrototypeOf(bookTwo); 
console.log(`bookTwo Prototype: ${bookTwoPrototype}`);

console.log(`Common prototype: ${ bookOnePrototype === bookTwoPrototype}`);

console.log(`book name: ${bookOne.title} has ${bookOne.peopleRating} rating by readers`);
console.log(`bookOne toString: ${bookOne.toString()}`);

// output
bookOne Prototype: [object Object]
bookTwo Prototype: [object Object]
Common prototype: true
bookOne toString: [object Object]
```

The output shows that the bookOne and bookTwo objects have the same prototype. 
Because prototypes are regular JavaScript objects, new properties can be defined on prototypes, and new values can be assigned to existing properties.

```js

let bookOne = {
    title: "Seven Seas",
    author: "Jin",
    peopleRating: 3,
    boxRating() {
        return Number(this.peopleRating) * 2;
    }
};

let bookTwo = {
    title: "Black nights",
    author: "Runnie",
    peopleRating: 5,
    boxRating() {
        return Number(this.peopleRating) * 2
 }
}

let bookOnePrototype = Object.getPrototypeOf(bookOne);

bookOnePrototype.toString = function() {
return `toString: Name: ${this.title}, Rating:  ${this.peopleRating}`; }

console.log(bookOne.toString()); 
console.log(bookTwo.toString());

// output
toString: Name: Seven Seas, Rating:  3
toString: Name: Black nights, Rating:  5
```

# Creating Custom Prototypes
Changes to Object should be made cautiously because they affect all the other objects in the application. The new toString function produces more useful output for the bookOne and bookTwo objects but assumes that there will be title and rating properties, which won’t be the case when toString is called on other objects.
A better approach is to create a prototype specifically for those objects that are known to have name and price properties, which can be done using the Object.setPrototypeOf method.

```js

let BookProto = {
    toString: function() {
return `toString: Book: ${this.title}, Rating: ${this.peopleRating}`;
 } 
}

let bookOne = {
    title: "Seven Seas",
    author: "Jin",
    peopleRating: 3,
    boxRating() {
        return Number(this.peopleRating) * 2;
    }
};

let bookTwo = {
    title: "Black nights",
    author: "Runnie",
    peopleRating: 5,
    boxRating() {
        return Number(this.peopleRating) * 2
 }
}

Object.setPrototypeOf(bookOne, BookProto); 
Object.setPrototypeOf(bookTwo, BookProto);

console.log(bookOne.toString()); 
console.log(bookTwo.toString());

// output
toString: Book: Seven Seas, Rating: 3
toString: Book: Black nights, Rating: 5
```

Prototypes can be defined just like any other object. In the above, an object named BookPrototype that defines a toString method is used as the prototype for the bookOne and bookTwo objects. The BookPrototype object is just like any other object, and that means it also has a prototype, which is Object.The effect is a chain of prototypes that the JavaScript works its way along until it locates a property or method or reaches the end of the chain.

# Using Constructor Functions
A constructor function is used to create a new object, configure its properties, and assign its prototype, all of which is done in a single step with the new keyword. Constructor functions can be used to ensure that objects are created consistently and that the correct prototype is applied.

```js
let Book = function(title, rating) {
            this.title = title;
            this.rating = rating; 
}

Product.prototype.toString = function() {
    return `toString: Name: ${this.title}, Rating:
    ${this.rating}`; 
}

let bookOne = new Product("Seven Seas", 3);
let bookTwo = new Product("Black Nights", 5);

console.log(bookOne.toString()); 
console.log(bookTwo.toString());

// output
toString: Name: Seven Seas, Rating: 3
toString: Name: Black Nights, Rating: 5
```

The JavaScript runtime creates a new object and uses it as the this value to invoke the constructor function, providing the argument values as parameters. The constructor function can configure the object’s own properties using this, which is set to the new object.

```js
let Book = function(title, rating) {
            this.title = title;
            this.rating = rating; 
}
```

The prototype for the new object is set to the object returned by the prototype property of the constructor function. This leads to constructors being defined in two parts—the function itself is used to configure the object’s
own properties, while the object returned by the prototype property is used for the properties and methods that should be shared by all the objects the constructor creates. In the listing, a toString property is added to the Book constructor function prototype and used to define a method.

```js
Book.prototype.toString = function() {
    return `toString: Name: ${this.title}, Rating:
    ${this.rating}`; 
}
```
The result is the same as the previous example, but using a constructor function can help ensure that objects are created consistently and have their prototypes set correctly.

# Chaining Constructor Functions
Using the setPrototypeOf method to create a chain of custom prototypes is easy, but doing the same thing with constructor functions requires a little more work to ensure that objects are configured correctly by the functions and get the right prototypes in the chain. Listing 4-8 introduces a new constructor function and uses it to create a chain with the Product constructor.

```js
let Book = function(title, rating) {
            this.title = title;
            this.rating = rating; 
}

Book.prototype.toString = function() {
    return `toString: Name: ${this.title}, Rating:
    ${this.rating}`; 
}

let Discount = function (title, rating, discount) {
    Book.call(this, title, rating);
    this.discount = discount;
}

Object.setPrototypeOf(Discount.prototype, Book.prototype);
Discount.prototype.boxRating = function () {
    return Number(this.peopleRating) * this.rating; 
}

Discount.prototype.toDiscountString = function() {
    return `${this.toString()}, Discount: ${this.boxRating()}`;
}

let bookOne = new Discount("Seven Sea", 3);
let bookTwo = new Book("Black Nights", 5);

console.log(bookOne.toString()); 
console.log(bookTwo.toString());

// output
toString: Name: Seven Sea, Rating: 3
toString: Name: Black Nights, Rating: 5
```

Two steps must be taken to arrange the constructors and their prototypes in a chain. The first step is to use the call method to invoke the next constructor so that new objects are created correctly. In the listing, I want the Discount constructor to build on the Book constructor, so I have to use call on the Book function so that it adds its properties to new objects.

```js
Book.call(this, title, rating);
```

The call method allows the new object to be passed to the next constructor through the this value.
The second step is to link the prototypes together.

```js
Object.setPrototypeOf(Discount.prototype, Book.prototype);
```

Notice that the arguments to the setPrototypeOf method are the objects returned by the constructor function’s prototype properties and not the functions themselves. Linking the prototypes ensures that the JavaScript runtime will follow the chain when it looks for properties that are not an object’s own.


The Discount prototype defines a toDiscountString method that invokes toString, which will be found by the JavaScript runtime on the Product prototype

## Accessing Overridden Prototype Methods
A prototype can override a property or method by using the same name as one defined further along the chain. This is also known as shadowing in JavaScript, and it takes advantage of the way that the JavaScript runtime follows the chain.
Care is required when building on an overridden method, which must be accessed through the prototype that defines it. The Discount prototype can define a toString method that overrides the one defined by the Product prototype and can invoke the overridden method by accessing the method directly through the prototype and using call to set the this value.

```js
Discount.prototype.toString = function() {
let chainResult = Book.prototype.toString.call(this);
return `${chainResult}, Tax: ${this.boxRating()}`;
}
```
This method gets a result from the Book prototype’s toString method and combines it with additional data in a template string.

# Checking Prototype Types
The instanceof operator is used to determine whether a constructor’s prototype is part of the chain for a specific object

```js
let Book = function(title, rating) {
            this.title = title;
            this.rating = rating; 
}

Book.prototype.toString = function() {
    return `toString: Name: ${this.title}, Rating:
    ${this.rating}`; 
}


let Discount = function (title, rating, discount) {
    Book.call(this, title, rating);
    this.discount = discount;
}

Object.setPrototypeOf(Discount.prototype, Book.prototype);
Discount.prototype.boxRating = function () {
    return Number(this.peopleRating) * this.rating; 
}

Discount.prototype.boxRating = function () {
    return Number(this.peopleRating) * this.rating; 
}

let bookOne = new Discount("Seven Sea", 3);
let bookTwo = new Book("Black Nights", 5);

console.log(bookOne.toString()); 
console.log(bookTwo.toString());

console.log(`bookOne and Discount: ${ bookOne instanceof Discount}`);
console.log(`bookOne and Book: ${ bookOne instanceof Book}`);
console.log(`bookOne and Discount: ${ bookTwo instanceof Discount}`);
console.log(`bookTwo and Book: ${ bookTwo instanceof Book}`);

// output
toString: Name: Seven Sea, Rating: 3
toString: Name: Black Nights, Rating: 5
bookOne and Discount: true
bookOne and Book: true
bookOne and Discount: false
bookTwo and Book: true
```
The new statements use instanceof to determine whether the prototypes of the Discount and Book constructor functions are in the chains of the bookOne and bookTwo objects

Tip Notice that the instanceof operator is used with the constructor function. The Object.isPrototypeOf method is used directly with prototypes, which can be useful if you are not using constructors.

# Defining Static Properties and Methods
Properties and methods that are defined on the constructor function are often referred to as static, meaning they are accessed through the constructor and not individual objects created by that constructor (as opposed to instance properties, which are accessed through an object). The Object.setPrototypeOf and Object.getPrototypeOf methods are good examples of static methods. 

```js
let Book = function(title, rating) {
            this.title = title;
            this.rating = rating; 
}

Book.prototype.toString = function() {
    return `toString: Name: ${this.title}, Rating:
    ${this.rating}`; 
}

Book.process = (...books) =>
books.forEach(b => console.log(b.toString()));
Book.process(new Book("Seven Seas", 3), new Book("Black Nights", 5));

// output
toString: Name: Seven Seas, Rating: 3
toString: Name: Black Nights, Rating: 5
```

The static process method is defined by adding a new property to the Book function object and assigning it a function. Remember that JavaScript functions are objects, and properties can be freely added and removed from objects. The process method defines a rest parameter and uses the forEach method to invoke the toString method for each object it receives and writes the result to the console.

# Using JavaScript Classes
JavaScript classes were added to the language to ease the transition from other popular programming languages. Behind the scenes, JavaScript classes are implemented using prototypes, which means that JavaScript classes have some differences from those in languages such as C# and Java

```js
class Book {
    constructor(title, rating) {
        this.title = title;
        this.rating = rating;
    }
    toString() {
        return `toString: Title: ${this.title}, Rating: ${this.rating};`
    }
}

let bookOne = new Book("Seven Seas", 3);
let bookTwo = new Book("Black Nights", 5);

console.log(bookOne.toString());
console.log(bookTwo.toString());

// output
toString: Title: Seven Seas, Rating: 3;
toString: Title: Black Nights, Rating: 5;
```

Classes are defined with the class keyword, followed by a name for the class. The class syntax may appear more familiar, but classes are translated into the underlying JavaScript prototype system described in the previous section.
Objects are created from classes using the new keyword. The JavaScript runtime creates a new object and invokes the class constructor function, which receives the new object through the this value and which is responsible for defining the object’s own properties. Methods defined by classes are added to the prototype assigned to objects created using the class.

# Using Inheritance in Classes
Classes can inherit features using the extends keyword and invoke the superclass constructor and methods using the super keyword

```js
class Vector {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return `toString: x: ${this.x}, y: ${this.y};`
    }
 }

 class Speed extends Vector {
    constructor(x, y, t = 5) {
        super(x, y);
        this.t = t;
    }
    objSpeed() {
        return Number(this.y) / this.t;
    }
    toString() {
        let chainResult = super.toString();
        return `${chainResult}, Speed: ${this.objSpeed()}`
    }
 }

 let firtObject = new Vector(1, 2);
 console.log(firtObject.toString());

 let secondObject = new Vector(3, 4, 5) 
 console.log(secondObject.toString());

// output 
toString: x: 1, y: 2;
toString: x: 3, y: 4;
```

A class declares its superclass using the extends keyword. In the listing, the Speed class uses the extend keyword to inherit from the Victor class. The super keyword is used in the constructor to invoke the superclass constructor, which is equivalent to chaining constructor functions.


The toString method defined by the objectSpeed class invoked the superclass’s toString method, which is equivalent to overriding prototype methods


# Defining Static Methods
The static keyword is applied to create static methods that are accessed through the class, rather than the object it creates.

```js
class Vector  {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    
    toString() {
        return `toString: x: ${this.x}, y: ${this.y}`
    }
}

class Speed extends Vector {
    constructor(x, y, t=5 ) {
        super(x, y);
        this.t = t;
    }
    objSpeed() {
        return Number(this.y) / this.t;
    }
    toString() {
        let chainResult = super.toString();
        return `${chainResult}, Speed: ${this.objSpeed()}`
    }
    static process(...vectors) { vectors.forEach(v =>
        console.log(v.toString())); 
        }
}

Speed.process(new Speed(3, 4, 5));

// output
toString: x: 3, y: 4, Speed: 0.8
```

# Using JavaScript Collections
Traditionally, collections of data in JavaScript have been managed using objects and arrays, where objects are used to store data by key, and arrays are used to store data by index. JavaScript also provides dedicated collection objects that provide more structure, although they can also be less flexible, as explained in the sections that follow.

# Storing Data by Key Using an Object
Objects can be used as collections, where each property is a key/value pair, with the property name being the key

```js
class Book {
    constructor(title, rating) {
        this.title = title;
        this.rating = rating;
    }
    toString() {
        return `toString: Title: ${this.title}, Rating: ${this.rating}`;
    }
}

let data = {
    bookOne: new Book("Seven Seas", 3)
}

data.bookTwo = new Book("Black nights", 5);

Object.keys(data).forEach(key => console.log(data[key].toString()));


// output
toString: Title: Seven Seas, Rating: 3
toString: Title: Black nights, Rating: 5
```
This example above uses an object named data to collect Book objects. New
values can be added to the collection by defining new properties, like this:

```js
data.boots = new Product("Boots", 100); 
```

Object provides useful methods for getting the set of keys or values from an object.

**Object.keys(object)**
This method returns an array containing the property names defined by the object.

**Object.values(object)**
This method returns an array containing the property values defined by the object.

the Object.keys method to get an array containing the property names defined by the data object and uses the array forEach method to get the corresponding value. When a property name is assigned to a variable, the corresponding value can be obtained using square brackets, like this:

```js
Object.keys(data).forEach(key => console.log(data[key].toString()));
```

The contents of the square brackets are evaluated as an expression, and specifying a variable name, such as key, returns its value. 

# Storing Data by Key Using a Map
Objects are easy to use as basic collections, but there are some limitations, such as being able to use only string values as keys. JavaScript also provides Map, which is purpose-built for storing data using keys of any type

```js
class Book {
    constructor(title, rating) {
        this.title = title;
        this.rating = rating;
    }
    toString() {
        return `toString: Title: ${this.title}, Rating: ${this.rating}`;
    }
}

let data = new Map();
data.set("bookOne", new Book("Seven Seas", 3));
data.set("booktwo", new Book("Black nights", 5));

[...data.keys()].forEach(key => console.log(data.get(key).toString()));

// output
toString: Title: Seven Seas, Rating: 3
toString: Title: Black nights, Rating: 5
```
The API provided by Map allows items to be stored and retrieved, and iterators are available for the keys and values.

|       Name                |       Description                                     |
|---------------------------|-------------------------------------------------------|
| set(Key, Value)           | This method stores a value with the specified key.    |
| get(Key)                  | This method retrieved the value stored with a specific key |
| keys()                    | this method returns an iterator for the keys in the map |
| values()                  | This method returns a value for the value in the map |
| entries()                 | This method returns an iterator for the key/value pairs in the map, each of which is presented as an array containing the key value and pair. This is the default iterator for Map objects.


# Using Symbols for Map Keys
The main advantage of using a Map is that any value can be used as a key, including Symbol values. Each Symbol value is unique and immutable and ideally suited as an identifier for objects. below defines a new Map that uses Symbol values as keys.

```js
class Book {
    constructor(title, rating) {
        this.id = Symbol();
        this.title = title;
        this.rating = rating;
    }
}

class Publisher {
    constructor(title, bookId) {
        this.title = title;
        this.bookId = bookId;
    }
}

let zinePublisher = [new Book("Seven Seas", 3), new Book("Black nights", 4)];
let xinPublisher = [new Book("Seven Seas", 3), new Book("Black nights", 4)]

let books = new Map();
[...zinePublisher, ...xinPublisher].forEach(p => books.set(p.id, p));

let publisher = new Map();
publisher.set("zine", new Publisher("Zine co", zinePublisher.map(p => p.id)));
publisher.set("xin", new Publisher("xin co", xinPublisher.map(p => p.id)));

publisher.get("zine").bookId.forEach(id => console.log(`Title: ${books.get(id).title}`))

// oputput
Title: Seven Seas
Title: Black nights
```

The benefit of using Symbol values as keys is that there is no possibility of two keys colliding, which can happen if keys are derived from the value’s characteristics. The previous example used the Book.title value as the key, which is subject to two objects being stored with the same key, such that one replaces the other. In this example, each Book object has an id property that is assigned a Symbol value in the constructor and that is used to store the object in the Map. Using a Symbol allows me to store objects that have identical name and price properties and retrieve them without difficulty.


# Storing Data by Index
JavaScript also provides Set, which stores data by index but has performance optimizations and —most usefully—stores only unique values.

```js
class Book {
    constructor(title, rating) {
        this.id = Symbol();
        this.title = title;
        this.rating = rating;
    }
}

let book = new Book("Seven Seas", 3);

let bookArray = [];
let bookSet = new Set();

for (let i = 0; i < 5; i++) {
    bookArray.push(book);
    bookSet.add(book);
}

console.log(`Book Array: ${bookArray.length}`);
console.log(`Set size: ${bookSet.size}`);

// output
Book Array: 5
Set size: 1
```

useful set models:

|       Name        |       Description                 |
|-------------------|-----------------------------------|
|   add(value)      | adds the value to the set         |
|   entries()       | returns an iterator from the items in the set, in order in which they were arranged           |
| has(value)        | returns true is the set contains the specified value |
| for(callback)     | invokes a function for each value in the set |

# Using Modules 
Everthing here is defined by the JavaScript specification, which is the most broadly supported by popular JavaScript development tools and application frameworks.