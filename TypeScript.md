# **TypeScript**

TypeScript is a super set of JavaScript.

### **Why TypeScript?**

- Types increase productivity by helping you avoid many mistakes. By using types, you can catch bugs at the compile-time instead of having them occurring at runtime.
- TypeScript supports the upcoming features planned in the ES Next for the current JavaScript engines. It means that you can use the new JavaScript features before web browsers (or other environments) fully support them.

### **Install TypeScript compiler**

```sh
npm install -g typescript
```

Check version by running

```sh
tsc --v
```

If you use TypeScript for backend NodeJS activity install ts-node

```sh
npm install -g ts-node
```

### **Types in TypeScript**

- Primitive types
  - string
  - number
  - boolean
  - null
  - undefined
  - symbol
- Object types
  - functions
  - arrays
  - classes etc.

### **Type Annotations**

TypeScript uses type annotations to explicitly specify types for identifiers such variables, functions, objects, etc.  
Syntax

```js
let variableName: type;
let variableName: type = value;
const constantName: type = value;
```

### **Type inference**

Type inference describes where and how TypeScript infers types when you don’t explicitly annotate them.

Example:

```js
let counter = 0; // is the same as let counter: number = 0
// TypeScript will infer the counter is of type number by default
```

#### **TypeScript Number**

Syntax

```js
let price: number = 99;
```

#### **TypeScript String**

Syntax

```js
let firstName: string = "John";
```

#### **TypeScript Boolean**

Syntax

```js
let pending: boolean = true;
```

#### **TypeScript object Type**

There are 2 ways to use object type  
Syntax 1

```js
let employee: object;

employee = {
  firstName: "John",
  lastName: "Doe",
  age: 25,
  jobTitle: "Web Developer",
};
```

Syntax 2

```js
let employee: {
  firstName: string,
  lastName: string,
  age: number,
  jobTitle: string,
};

employee = {
  firstName: "John",
  lastName: "Doe",
  age: 25,
  jobTitle: "Web Developer",
};
```

In Syntax 1 we simply use the type object, however in Syntax 2 we explicitly assign the types to the object properties.

#### **TypeScript Array type**

Syntax

```js
let arrayName: type[];

// example
let skills: string[] = ["JS", "TS", "ReactJS", "NextJS"];
```

```js
// storing values of mixed types
let scores: (string | number)[];
scores = ["Programming", 5, "Software Design", 4];
```

#### **TypeScript Tuple**

A tuple works like an array with some additional considerations:

- The number of elements in the tuple is fixed.
- The types of elements are known.

```js
let skill: [string, number];
skill = ["Programming", 5];
```

Optional Tuple Elements

```js
let bgColor: [number, number, number, number?];
bgColor = [0, 255, 255]
```

#### **TypeScript Enum**

An enum is a group of named constant values. Enum stands for enumerated type.  
To define an enum, you follow these steps:

- First, use the enum keyword followed by the name of the enum.
- Then, define constant values for the enum.

Example:

```js
enum Month {
    Jan,
    Feb,
    Mar,
    Apr,
    May,
    Jun,
    Jul,
    Aug,
    Sep,
    Oct,
    Nov,
    Dec
};

function isItSummer(month: Month) {
    let isSummer: boolean;
    switch (month) {
        case Month.Jun:
        case Month.Jul:
        case Month.Aug:
            isSummer = true;
            break;
        default:
            isSummer = false;
            break;
    }
    return isSummer;
}

console.log(isItSummer(Month.Jun)); // true
```

#### **TypeScript any Type**

Sometimes, you may need to store a value in a variable. But you don’t know its type at the time of writing the program. And the unknown value may come from a third party API or user input.

In this case, you want to opt-out of the type checking and allow the value to pass through the compile-time check.

Syntax

```js
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

#### **TypeScript void Type**

The void type denotes the absence of having any type at all. It is a little like the opposite of the any type.

Typically, you use the void type as the return type of functions that do not return a value.  
Example:

```js
function log(message): void {
  console.log(messsage);
}
```

#### **TypeScript never Type**

The never type is a type that contains no values. Because of this, you cannot assign any value to a variable with a never type.
Typically, you use the never type to represent the return type of a function that always throws an error.

Example:

```js
function raiseError(message: string): never {
  throw new Error(message);
}
```

#### **TypeScript union Type**

Example:

```js
let result: number | string;
result = 10; // OK
result = "Hi"; // also OK
result = false; // a boolean value, not OK
```

#### **TypeScript Type Aliases**

Example:

```js
type alphanumeric = string | number;
let input: alphanumeric;
```

#### **TypeScript String Literal Types**

Example:

```js
let click: "click";
click = "click"; // valid
click = "dblclick"; // compiler error

let mouseEvent: "click" | "dblclick" | "mouseup" | "mousedown";
mouseEvent = "click"; // valid
mouseEvent = "dblclick"; // valid
mouseEvent = "mouseup"; // valid
mouseEvent = "mousedown"; // valid
mouseEvent = "mouseover"; // compiler error
```
