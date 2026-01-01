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

Classes are a "blueprint" of functionality.
Defines the methods each instance of Triangle will have.
An instance of the class can be created use the `new` keyword.
`this` references the particular instance of the class.

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
