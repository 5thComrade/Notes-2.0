# **TypeScript**

TypeScript is a super set of JavaScript.

To read more about TypeScript visit [John Smilga's TypeScript Repo](https://github.com/john-smilga/typescript-course)

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

#### **TypeScript Optional Types**

Example:

```ts
type obj = {
    name: string,
    age?: number, // age could either be a number or undefined
}
```

---

### **Functions in TypeScript**

Example: Function param type

```ts
function sayHi(name: string) {
  console.log(`Hello ${name}`)
}
```

Example: Function return type

```ts
function calculateDiscount(price: number): number {
  return price * 0.9;
}
```

Example: Function with optional params

```ts
function calculatePrice(price: number, discount?: number): number { // discount is an optional paramater
  if (discount) {
    return price - discount;
  }
  return price;
}
```

Example: Functions with default param value

```ts
function calculatePrice(price: number, discount: number = 0): number { // default value of discount is 0 if discount isn't provided
  if (discount) {
    return price - discount;
  }
  return price;
}
```

Example: Functions with the rest operator

```ts
function sum(message: string, ...numbers: number[]): string {
  return 'some string that uses the parameters'
}
```

Example: Functions that don't return anything

```ts
function logMessage(message: string): void {
  console.log(message)
}
```

Example: Function with union type params

```ts
function processInput(input: number | string) {
  if (typeof input === 'number') { // this is a type guard
    console.log(input * 2)
  } else {
    console.log(input.toLowerCase())
  }
}
```

Example: Functions with objects as params

```ts
function createEmployee({ id }: { id: number }): {
  id: number;
  isActive: boolean;
} {
  return {
    id: id,
    isActive: true,
  };
}

// alternative approach

type StudentT = {
  id: number;
  name: string;
};

function createStudent(student: StudentT): void {
  console.log("student: ", student);
}
```

Example: Default value for object parameter when passed as prop

```ts
function processData(
  input: string | number,
  config: { reverse: boolean } = { reverse: false }
) {
  console.log("reverse:", config.reverse);
}
```

---

## Type Alias

```ts
type UserT = { id: number; name: string; isActive: boolean }; // this is a type alias
```

Intersection Type

```ts
type Book = {
    id: number;
    name: string;
    price: number;
}

const book1: Book = {
    id: 1,
    name: 'How to cook dragons'
    price: 200
}

type DiscountedBook = Book & { discount: number } // this is intersection type

const discountedBook: DiscountedBook = {
    id: 2,
    name: 'How to negotiate a discount'
    price: 180,
    discount: 10
}
```

You can also have computed values as props in type alias's

```ts
const propName = "age";

type Tiger = {
  [propName]: numner;
};
```

---

## Interface

Type Alias and Interface can be used interchangeably, they both do the same.

```ts
// There are two main tools to declare the shape of an
// object: interfaces and type aliases.

// We recommend you use interfaces over type
// aliases. Specifically, because you will get better error
// messages.
interface Book {
  readonly isbn: number;
  title: string;
  author: string;
  genre?: string;
}

const deepWork: Book = {
  isbn: 123,
  title: "deep work",
  author: "cal newport",
  genre: "self-help",
};
```

### Methods in Interfaces

```ts
interface Book {
  readonly isbn: number;
  title: string;
  author: string;
  genre?: string;
  printAuthor(): void; // methods in interface
  printTitle(message: string): string;
  printSomething: (n: number) => number; // alternate way to define methods
}

const deepWork: Book = {
  isbn: 123,
  title: "deep work",
  author: "cal newport",
  genre: "self-help",
  printAuthor() {
    console.log("Author: ", this.author);
  },
  printTitle(m) {
    return `${this.title} ${m}`;
  },
  printSomething: (n) => {
    return n;
  },
};
```

### Merging interfaces

If you know the name of the interface you can add additonal props to it like below.

```ts
interface Person {
  name: string;
  getDetails: () => string;
}

interface Person { // notice that the name is same as the interface above. Now Person interface has 3 properties name, age and getDetails
  age: number;
}
```

### Extending interfaces

Creating brand new interfaces that simply has all the properties from the parent interface plus additional properties.

```ts
interface Person {
  name: string;
  getDetails: () => string;
}

interface Employee extends Person {
  employeeId: number;
}
```

Extending from multiple interfaces

```ts
interface Person {
  name: string;
  getDetails: () => string;
}

interface Employee {
  employeeId: number
}

interface Manager extends Person, Employee { // notice how Manager interface extends from both Person and Employee
  managePeople: () => void;
}
```


### Type Predicates

A predicate takes the form parameterName is Type, where parameterName must be the name of a parameter from the current function signature.

```ts
const employee: Person | DogOwner | Manager = getEmployee(); // employee can either be of instance Person or of instance DogOwner or of instance Manager

const isManager = (obj: Person | DogOwner | Manager): obj is Manager => { // obj is Manager is Type Predicate. 
  return 'delegateTasks' in obj; // this returns true if delegateTasks is a method in the obj therefore its a manager
}
```

---

## Interface vs Type Alias

A type alias is a way to give a name to a type. It can represent primitive types, union types, intersection types, tuples and any other types. Once defined,
the alias can be used anywhere in place of the actual type.

```ts
type Person = {
  name: string;
  age: number;
}

let john: Person = { name: 'John', age: 25 }
```

An interface is a way to define a contract for a certain structure of an object.

```ts
interface Person {
  name: string;
  age: number;
}

let john: Person = { name: 'John', age: 25 }
```

Interfaces cannot be used for primitive types like numbers, strings etc
Interfaces can be merged using declaration merging.

---

## Tuples

A tuple is a typed array with a pre-defined length and types for each index.

```ts
let person: [string, number] = ['antony', 27]

let date: readonly [number, number, number] = [12, 17, 2024];

date.push(34); // this will now not be possible because we marked date as readonly
```

---

## Enums

Enums allow a developer to define a set of named constants. Using enums can make it easier to document intent, or create a set of distinct cases. TypeScript provides both numeric and string-based enums.

```ts
enum ServerResponseStatus {
  SUCCESS,
  ERROR
}

interface ServerResponse {
  result: ServerResponseStatus; // assigning the enum
  data: string[];
}

function getServerResponse() {
  return {
    result: ServerResponseStatus.SUCCESS, // this is how we use enums
    data: ['a', 'b', 'c']
  }
}
```

---

## Type Assertion

It is essentially a way to tell TypeScript that we know what the type is.

```ts
let someValue: any = 'this is a string';

let stringLength: number = (someValue as string).length; // the as keyword asserts that someValue is actually of type string
```

---

## Type unknown

The unknown type in TypeScript is a type-safe counterpart of the type any. It's like saying that a variable could be anything, but we need to 
perform some type-checking before we can use it.

```ts
let unknownValue: unknown

unknownValue = 'hello world';

if (typeof unknownValue === 'string') { // we need to check the type of unknown value before any operations on it
  console.log(unknownValue.length)
}
```

---

## Type never

In TypeScript, never is a type that represents the type of values that never occur. You can't assign a value to a variable of type never.

```ts
let someValue: never = 0; // typescript will not be happy setting a value to a variable of type never
```

---

Modules in TypeScript

TypeScript doesn't treat a file as a module by default, they are actually created as scripts in the global scope. That is, if we define a variable in one file and then again define another variable with the same name in a different file, TypeScript is going to complain.

How to fix this?

1: if you add import/export statement in the file, the file is immediately treated as a module.

2: open tsconfig.json and add 'moduleDetection' as force

```json
{
  "moduleDetection": "force"
}
```

How does importing something from a .js file inside a .ts file work?

By default ts will not be able to detect the .js file cause its not .ts. We can however force ts to allow js by setting allowJs to true in the tsconfig.json file.

---

### Type Guarding

Type guarding is a term in TypeScript that refers to the ability to narrow down the type of an object within a certain scope. This is usually done using conditional statements that check the type of an object.

- typeof

```ts
function checkValue(v: ValueT) {
  if (typeof v === 'string') {
    console.log('this type guard suggests the value is of type string.')
  }
}
```

- Equality narrowing

  In TypeScript, equality narrowing is a form of type narrowing that occurs when you use equality checks like === or !== in your code.

  ```ts
  type Dog = { type: 'dog'; name: string; bark: () => void }
  type Cat = { type: 'cat'; name: string; meow: () => void }

  type Animal: Dog | Cat

  function makeSound(a: Animal) {
    if (a.type === 'dog') {
      a.bark()
    } else {
      a.meow()
    }
  } 
  ```

- check for property

  ```ts
  type Dog = { type: 'dog'; name: string; bark: () => void }
  type Cat = { type: 'cat'; name: string; meow: () => void }

  type Animal: Dog | Cat

  function makeSound(a: Animal) {
    if ('bark' in a) { // checking the property
      a.bark()
    } else {
      a.meow()
    }
  } 
  ```

- "Truthy"/"Falsy" guard

  ```ts
  function printLength(str: string | null | undefined) {
    if (str) { // checking for "Truthy"/"Falsy"
      console.log('length: ', str.length);
    }
  }
  ```

- instanceof type guard

  ```ts
  try {
    throw new Error("some error")
  } catch(error) {
    if (error instanceof Error) {
      console.log('Error message: ', error.message)
    } else {
      console.log(error)
    }
  }
  ```

  ```ts
  if (input istanceof Date) {
    console.log('this input is a Date')
  }
  ```

- Type Predicate

  A type predicate is a function whose return type is a special kind of type that can be used to narrow down types within conditional blocks.

  ```ts
  function isStudent(person: Person): person is Student { // this is a type predicate function
    return 'study' in person;
  }
  ```

- Discriminated Unions and exhaustive check using the never type

  A discriminated union in TypeScript is a type that can be one of several different types, each identified by a unique literal property (the discriminator), allowing for type-safe handling of each possible variant.

  ```ts
  type IncrementAction = {
    type: 'increment'; // -------> this is how you create a discriminated union
    amount: number;
    timestamp: number;
    user: string;
  }

  type DecrementAction = {
    type: 'decrement'; // -------> this is how you create a discriminated union
    amount: number;
    timestamp: number;
    user: string;
  }

  type Action = IncrementAction | DecrementAction;

  function reducer(state: number, action: Action) {
    switch(action.type) {
      case 'increment':
        return state + action.amount;
      case 'decrement':
        return state - action.amount;
      default:
        return state;
    }
  }

  const newState = reducer(15, {
    amount: 5,
    timestamp: 123456,
    user: 'John',
    type: 'increment',
  })
  ```

---

## Generics

```ts
let array1: Array<string> = ['Apple', 'Mango', 'Banana']; // Array is a default interface that comes with TypeScript and the angle brackets defines generics

function genericFunction<T>(arg: T): T {
  return arg;
}

const someStringValue = genericFunction<string>('Hello');

interface GenericInterface<T> {
  value: T;
  getValue: () => T;
}

const genericString: GenericInterface<string> = {
  value: 'Hello World!',
  getValue() {
    return this.value;
  }
}

async function someAsyncFunc(): Promise<string> {
  return 'hello world!';
}
```

Multiple generic types

```ts
function pair<T, U>(param1: T, param2: U): [T, U] {
  return [param1, param2];
}
```

If you want to restrict generic types use the following syntx

```ts
function processValue<T extends string | number>(value: T): T {
  return value;
}

type Car = {
  brand: string;
  model: number;
}

type Product = {
  name: string;
  price: number;
}

type: Student = {
  name: string;
  age: number;
}

function printName<T extends { name: string }>(input: T): void {
  console.log(input.name); // this was possible because T extends two types that have the name prop, only Product and Student will be accepted by this function
}
```

Default generic type

```ts
interface StoreData<T = any> { // any is the default type of T
  data: T[];
}

const storeNumbers: StoreData<number> = {
  data: [1, 2, 3, 4]
}

const storeRandom: StoreData = { // we didn't even pass a generic type here
  data: ['hello', 1]
}
```

Axios.get expects a generic type for the data property it returns.

```ts
const { data } = axios.get<{name: string}[]>(someUrl); // now data will be considered as an array of objects with 1 prop ie name. If the generic is not passed data will be of type any
```

