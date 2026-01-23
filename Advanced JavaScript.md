## Advanced JavaScript

### Object Oriented JavaScript

```js
const obj1 = {};
const obj2 = new Object(); // samr thing
```

Properties that don't exist on an object always return `undefined`

All keys get stringified

```js
o1[1] = "hello";
o1["1"] = "goodbye";

console.log(o1[1]); // will log goodbye
 
o2[true] = "boolean" // same as o2["true"]
```

### this

this references 'this object'

```js
const obj = {
  a: 2,
  logValue: function() {
    console.log("value of a: ", this.a);
  }
}
```

### Classes

- Classes are a "blueprint" of functionality.
- Defines the methods each instance of Triangle will have.
- An instance of the class can be created use the `new` keyword.
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

JS gives us "static methods", wehere the method is called on a Class, not an object - therefore it cannot have access to individual object attributes

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

We are aware of the Math class in JavaScript, where we do operations like Math.random(), Math.sqrt() etc. What we never do is instantiate an object using the Math class, we simply class the static methods in the Math class for our operations. Well thats a use case of static methods.

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


















