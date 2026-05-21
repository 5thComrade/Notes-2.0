## Advanced JavaScript

### Object Oriented JavaScript

```js
const obj1 = {};
const obj2 = new Object(); // same thing
```

Properties that don't exist on an object always return `undefined`

All keys get stringified

```js
o1[1] = "hello";
o1["1"] = "goodbye";

console.log(o1[1]); // will log goodbye
 
o2[true] = "boolean" // same as o2["true"]
```

---

### this

this references 'this object'

```js
const obj = {
  a: 2,
  logValue: function() {
    console.log("value of a: ", this.a); // value of a: 2
  }
}
```

#### this in Regular functions and Arrow functions

Regular Functions (Dynamic Scope)

- Key Characteristic: this is defined by how the function is called.

- Best for: Object methods or functions that need to be "re-bound" to different contexts.

The Trap: They "lose" their context when used as callbacks (like in setTimeout or .map()).

```js
const dashboard = {
  user: "Admin",
  // Method using a regular function
  showUser: function() {
    console.log(this.user); 
    
    // The "Lost Context" problem
    setTimeout(function() {
       console.log(this.user); // Output: undefined
    }, 100);
  }
};
```

Arrow Functions (Lexical Scope)

- Key Characteristic: this is defined by where the function is written.

- Best for: Callbacks, simple logic, and keeping the parent's context alive.

The Trap: Cannot be used as constructors (with new) and don't have an arguments object.

```js
const dashboard = {
  user: "Admin",
  // Method using arrow function for callback
  showUser: function() {
    console.log(this.user); 

    // Inherits 'this' from showUser
    setTimeout(() => {
       console.log(this.user); // Output: "Admin"
    }, 100);
  }
};
```

Better example

For a regular function, the value of this is not set when you write the function; it is determined at the moment the function is called.

The Analogy: Imagine a "Rental Car." The "owner" of the car changes depending on who is currently driving it. If Alice is driving, this is Alice. If Bob takes over, this is Bob.

```js
const user = {
  name: "Alex",
  greet: function() {
    console.log("Hello, " + this.name);
  }
};

user.greet(); // Output: Hello, Alex (The 'user' called it)
```

For an arrow function, the value of this is decided based on where the function is written in your code. It "inherits" this from its surrounding environment (the parent scope).

The Analogy: Imagine a "Family Heirloom." No matter who is holding the heirloom or where they take it, the "owner" (the family name) stays the same because of where the heirloom originated.

Inside a setTimeout, a regular function loses its connection to your object because the timer is calling the function, not you. Arrow functions solve this because they "remember" the this from the line of code where you created them.

```js
const user = {
  name: "Alex",
  greet: () => {
    console.log("Hello, " + this.name);
  }
};

user.greet(); // Output: Hello, undefined (or empty)
```

Now you may think isn't the object user the surrounding of the arrow function? Why didn't it look at the surrounding and go outside the curly braces?

Let us understand scope

In JavaScript, { } curly braces do not always create a "scope."

When you create an object like const user = { ... }, you are defining a data structure, not a new scope
- Functions create scope.
- Classes create scope.
- Objects do not.

When you write an arrow function inside an object literal, the "surrounding environment" isn't the object itself—it's whatever is outside the object (usually the Global Window or the Module).

**Test your knowledge**

What will happen in this case?

```js
const parentArrow = () => {
  setTimeout(() => {
    console.log('Name: ', this.name)
  }, 1000);
}

function regularFunc() {
  this.name = "Antony"
  
  return parentArrow()
}

regularFunc()
```

The result of your code will be: `Name: undefined`

Remember: Arrow functions do not care who calls them or where they are called. They only care about where they were defined.
- `parentArrow` was defined in the Global Scope (the top level of your script).
- Because it was born in the Global Scope, its this is locked to the Global Object (the Window) the moment you wrote it.


---

### Classes

- Classes are a "blueprint" of functionality.
- Defines the methods each instance of Class will have.
- An instance of the class can be created using the `new` keyword.
- `this` references the particular instance of the class.

Class names should be UpperCamelCase

```js
class Triangle {
  getArea() {
    return (this.a * this.b) / 2; 
  }
}

const myTri = new Trianagle() // instantiation
myTri.a = 3;
myTri.b = 4;
myTri.getArea(); // 6

console.log(myTri instanceOf Triangle) // true
```
---

### Constructors

Any method named `constructor` will be called on making a new instance

- We can do validation of the inputs
- Assign properties

```js
class Triangle {
 constructor(a, b) {
   this.a = a;
   this.b = b;
 }

 getArea() {
    return (this.a * this.b) / 2; 
 }
}

let myTri = new Triangle(3, 5) // <-- calls the constructor
```

### Methods

Functions placed in a class are methods (formally, instance methods)
They have access to properties of the object with `this`.

### Inheritance

```js
class ShyTriangle extends Triangle {
  describe() {
    console.log('this is a shy trinagle which inherits from Triangle')
  }
}
```

### Super Keyword

```js
class ColorTriangle extends Triangle {
  constructor(a, b, color) {
    super(a, b); // calls the parent constructor
    this.color = color;
  }

  describe() {
    return `Area is ${super.getArea()} and color is ${this.color}`;
  }
}
```

### Static Properties

Modern JS also offers "static properties", where individual pieces of data are on the class, not on instances.

```js
class Cat {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  static species = "feline catus";
}

console.log(Cat.species); // directly accessible from the class hence you will see feline catus

const cat1 = new Cat("Tom", "Some Breed");

console.log(cat1.species); // this will log undefined cause static properties are not available in class instances
```

### Static methods

JS gives us "static methods", where the method is called on a Class, not an object - therefore it cannot have access to individual object attributes

```js
class CatWithStaticMethod {
   constructor(fname) {
     this.fname = fname;
   }

   static myStaticMethod() {
     console.log("my static method this: ", this); // this inside static methods refers to the class itself
   }

   myMethod() {
      console.log("my method this: ", this);
   }
}

const newCat = new CatWithStaticMethod("Tom");
newCat.myMethod(); // logs the instance
CatWithStaticMethod.myStaticMethod(); // logs the class CatWithStaticMethod
```

**What are the use cases for static methods?**

We are aware of the Math class in JavaScript, where we do operations like Math.random(), Math.sqrt() etc. What we never do is instantiate an object using the Math class, we simply use the static methods in the Math class for our operations. Well thats a use case of static methods.

```js
class MyMath {
  constructor() {
    throw new Error("MyMath cannot be instantiated.")    
  }

  static add(a, b) {
    return a + b;
  }
}
```

Another use case for static methods is, it can be used as factory functions/methods.

A factory function in JavaScript is a function that creates and returns a new object without using the new keyword

```
class Cat {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  static createStrayCat() {
    return new Cat(`${Date.now()}`, "unknown")
  }

  meow() {
    console.log(`${this.name} says meow!`)
  }
}

const strayCat = Cat.createStrayCat(); // now we created a new object without using the new keyword.
```

### Getters

JavaScript provides special methods, termed "getters" and "setters", which allow you to define the ways to retrive or change the values of object properties respectively.

Getters allow you to retrieve the value of an object's property

```js
class Circle {
   constructor(radius) {
      this.radius = radius;
   }

   get diameter() {
      return this.radius * 2;
   }
}

const circle = new Circle(5);
console.log(circle.diameter); // 10 - If you look we treat diameter as a property and not a method, we do not use parenthesis 
```

### Setters

Allows you to set the value of an object's property.

```js
class Circle {
  constructor(radius) {
    this._radius = radius; // this is a way of telling another developer this is an internal variable. Please don't change it directly from outside the class. Use a proper method instead
  }

  // Setter for the radius, this is the proper method to set the value of _radius
  set radius(value){
    if (value < 0) {
      throw new Error("Radius cannot be negative");
    } else {
      this._radius = value; // this is the proper way to set the internal variable
    }
  }
}

const circle = new Circle(5);
circle.radius = -3; // Error: Radius cannot be negative. Here we are not calling the internal variable _radius, here we are calling the setter radius and trying to set its value to -3
circle._radius = -5; // This will set the internal _radius to -5, but the underscore is the only way of telling developers DON'T DO IT!
```

Setter can be very useful for validation and to make the constructor shorter.

### Public Fields

Private instance class fields provide a way to maintain encapsulation and not allow external access.

```ts
class MyClass {
  publicField = "Public Field"; // these lines are called class fields same as static variables

  #privateField = "Private Field";

  getPrivateField() {
    return this.#privateField;
  }
}

const instance = new MyClass();
console.log(instance.publicField); // 'Public Field'
console.log(instance.getPrivateField()); // 'Private Field'
console.log(instance.#privateField()); // Error
```

The public class fields are available in the instance or the object. If you log the instance you'd see the public field in the object.

If we want private fields, follow the syntax of naming the variable with `#` at the start.

So why not use the constructor instead? Use the public field to store default values so the constructor looks clean.

You can define public class fields without any values and they will be set to undefined until the value gets set in the constructor.

So defining class fields at the top of the class makes it clear for anyone looking at the class to identify all the fields of the class instance.

```ts
class Cat {
    static numOfCats = 0

    name;
    breed;
    noOfLegs = 4;

    constructor(name, breed) {
        this.name = name;
        this.breed = breed;
    }
}

const cat = new Cat("Tom", "feline");

console.log('cat: ', cat)
```

### Private Fields

