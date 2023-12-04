# JavaScript Design Patterns - Book

These are my personal notes from a book I read about JavaScript Design Patterns.

## Introduction to Design Patterns

"Good code is like a love letter to the next developer who will maintain it!"

A pattern is a reusable solution template that you can apply to recurring problems and themes in software design.

Learning and using design patterns is mainly advantageous for developers because of the following:

- Patterns are proven solutions.

  They are the result of the combined experience and insights of developers who helped define them. They are time-tested approaches known to work when solving specific issues in software development.

- Patterns can be easily reused.

  A pattern usually delivers an out-of-the-box solution you can adopt and adapt to suit your needs. This feature makes them quite robust.

- Patterns can be expressive.

  Patterns can help express elegant solutions to extensive problems using a set structure and a shared vocabulary.

- Patterns assist in preventing minor issues that can cause significant problems in the application development process.

- Patterns provide generalized solutions, documented in a fashion that doesn’t require them to be tied to a specific problem.

- Some patterns can decrease the overall code file-size footprint by avoiding repetition.

- Patterns add to a developer’s vocabulary, which makes communication faster.

- Popular design patterns can be improvised further by harnessing the collective experiences of developers using those patterns and contributing back to the community.

---

## “Pattern”-ity Testing, Proto-Patterns, and the Rule of Three

### What is a Proto-Pattern?

A pattern that has not yet conclusively passed the “pattern”-ity tests is usually referred to as a proto-pattern.

Alternatively, the individual(s) sharing the pattern may not have the time or interest in going through the “pattern”-ity process and might release a short description of their proto-pattern instead. Brief descriptions or snippets of this type of pattern are known as patlets.

A pattern may be considered “good” if it does the following:

- Solves a particular problem

  Patterns are not supposed to just capture principles or strategies. They need to capture solutions. This is one of the most essential ingredients of a good pattern.

- Does not have an obvious solution

  We can find that problem-solving techniques often attempt to derive from well-known first principles. The best design patterns usually provide solutions to issues indirectly—this is considered a necessary approach for the most challenging problems related to design.

- Describes a proven concept

  Design patterns require proof that they function as described, and without this proof, the design cannot be seriously considered. If a pattern is highly speculative in nature, only the brave will attempt to use it.

- Describes a relationship

  In some cases, it may appear that a pattern describes a type of module. Despite what the implementation looks like, the official description of the pattern must describe much deeper system structures and mechanisms that explain its relationship to code.

### Rule of Three

One of the additional requirements for a pattern to be valid is that it displays some recurring phenomenon. You can often qualify this in at least three key areas, referred to as the rule of three.

- Fitness of purpose

  How is the pattern considered successful?

- Usefulness

  Why is the pattern considered successful?

- Applicability

  Is the design worthy of being a pattern because it has broader applicability?

---

## Modern JavaScript Syntax and Features

### The Importance of Decoupling Applications

In the world of scalable JavaScript, when we say an application is modular, we often mean it’s composed of a set of highly decoupled, distinct pieces of functionality stored in modules. Loose coupling facilitates easier maintainability of apps by removing dependencies where possible. If implemented efficiently, it allows you to see how changes to one part of a system may affect another.

### Modules with Imports and Exports

Modules allow us to separate our application code into independent units, each containing code for one aspect of the functionality. Modules also encourage code reusability and expose features that can be integrated into different applications.

// Filename: staff.mjs

```js
export const baker = {
  bake(item) {
    console.log(`Woo! I just baked ${item}`);
  },
};
```

// Filename: cakeFactory.mjs

```js
import baker from "/modules/staff.mjs";
```

### Module Objects

A cleaner approach to importing and using module resources is to import the module as an object.

```js
import * as Staff from "/modules/staff.mjs";
```

### Modules Loaded from Remote Sources

ES2015+ also supports remote modules (e.g., third-party libraries), making it simplistic to load modules from external locations.

```js
import { cakeFactory } from "https://example.com/modules/cakeFactory.mjs";
```

### Static Imports

The type of import we discussed so far is called static import. The module graph needs to be downloaded and executed with static import before the main code can run. This can sometimes lead to the eager loading of a lot of code up front on the initial page load, which can be expensive and delay key features being available earlier.

### Dynamic Imports

Dynamic import introduces a new function-like form of import. import(url) returns a promise for the module namespace object of the requested module, which is created after fetching, instantiating, and evaluating all of the module’s dependencies, as well as the module itself.

```js
form.addEventListener("submit", (e) => {
  e.preventDefault();
  import("/modules/cakeFactory.js").then((module) => {
    // Do something with the module.
    module.oven.makeCupcake("sprinkles");
    module.oven.makeMuffin("large");
  });
});
```

Dynamic import can also be supported using the await keyword:

```js
let module = await import("/modules/cakeFactory.js");
```

#### Import on Interaction

Some libraries may be required only when a user starts interacting with a particular feature on the web page. Typical examples are chat widgets, complex dialog boxes, or video embeds.

```js
const btn = document.querySelector("button");

btn.addEventListener("click", (e) => {
  e.preventDefault();
  import("lodash.sortby")
    .then((module) => module.default)
    .then(sortInput()) // use the imported dependency
    .catch((err) => {
      console.log(err);
    });
});
```

#### Import on Visibility

Many components are not visible on the initial page load but become visible as the user scrolls down. Since users may not always scroll down, modules corresponding to these components can be lazy-loaded when they become visible. The IntersectionObserver API can detect when a component placeholder is about to become visible, and a dynamic import can load the corresponding modules.

### Advantages of Using Modules

- Modules scripts are evaluated only once.
  The browser evaluates module scripts only once, while classic scripts get evaluated as often as they are added to the DOM.
- Modules are auto-deferred.
  Unlike other script files, where you have to include the defer attribute if you don’t want to load them immediately, browsers automatically defer the loading of modules.
- Modules are easy to maintain and reuse.
  Modules promote decoupling pieces of code that can be maintained independently without significant changes to other modules.
- Modules provide namespacing.
  Modules create a private space for related variables and constants so that they can be referenced via the module without polluting the global namespace.
- Modules enable dead code elimination.
  Before the introduction of modules, unused code files had to be manually removed from projects. With module imports, bundlers such as webpack and Rollup can automatically identify unused modules and eliminate them.

### Classes with Constructors, Getters, and Setters

In addition to modules, ES2015+ also allows defining classes with constructors and some sense of privacy. JavaScript classes are defined with the class keyword.

```js
class Cake {
  // We can define the body of a class constructor
  // function by using the keyword constructor
  // with a list of class variables.
  constructor(name, toppings, price, cakeSize) {
    this.name = name;
    this.cakeSize = cakeSize;
    this.toppings = toppings;
    this.price = price;
  }

  // As a part of ES2015+ efforts to decrease the unnecessary
  // use of function for everything, you will notice that it is
  // dropped for cases such as the following. Here an identifier
  // followed by an argument list and a body defines a new method.

  addTopping(topping) {
    this.toppings.push(topping);
  }

  // Getters can be defined by declaring get before
  // an identifier/method name and a curly body.
  get allToppings() {
    return this.toppings;
  }

  get qualifiesForDiscount() {
    return this.price > 5;
  }

  // Similar to getters, setters can be defined by using
  // the set keyword before an identifier
  set size(size) {
    if (size < 0) {
      throw new Error(
        "Cake must be a valid size: " + "either small, medium or large"
      );
    }
    this.cakeSize = size;
  }
}

// Usage
let cake = new Cake("chocolate", ["chocolate chips"], 5, "large");
```

You can also use the extends keyword to indicate that a class inherits from another class:

```js
class BirthdayCake extends Cake {
  surprise() {
    console.log(`Happy Birthday!`);
  }
}

let birthdayCake = new BirthdayCake(
  "chocolate",
  ["chocolate chips"],
  5,
  "large"
);
birthdayCake.surprise();
```

JavaScript classes also support the super keyword, which allows you to call a parent class’ constructor. This is useful for implementing the self-inheritance pattern. You can use super to call the methods of the superclass:

```js
class Cookie {
  constructor(flavor) {
    this.flavor = flavor;
  }

  showTitle() {
    console.log(`The flavor of this cookie is ${this.flavor}.`);
  }
}

class FavoriteCookie extends Cookie {
  showTitle() {
    super.showTitle();
    console.log(`${this.flavor} is amazing.`);
  }
}

let myCookie = new FavoriteCookie("chocolate");
myCookie.showTitle();
// The flavor of this cookie is chocolate.
// chocolate is amazing.
```

Modern JavaScript supports public and private class members. Public class members are accessible to other classes. Private class members are accessible only to the class in which they are defined. Class fields are, by default, public. Private class fields can be created by using the # (hash) prefix:

```js
class CookieWithPrivateField {
  #privateField;
}

class CookieWithPrivateMethod {
  #privateMethod() {
    return "delicious cookies";
  }
}
```

JavaScript classes support static methods and properties using the static keyword. Static members can be referenced without instantiating the class. You can use static methods to create utility functions and static properties for holding configuration or cached data:

```js
class Cookie {
  constructor(flavor) {
    this.flavor = flavor;
  }
  static brandName = "Best Bakes";
  static discountPercent = 5;
}
console.log(Cookie.brandName); //output = "Best Bakes"
```

## Categories of Design Patterns

Design patterns can be categorized based on the type of problem they solve. The three principal categories of design patterns are:

- Creational design patterns
- Structural design patterns
- Behavioral design patterns

### Creational Design Patterns

Creational design patterns focus on handling object-creation mechanisms where objects are created in a manner suitable for a given situation. The basic approach to object creation might otherwise lead to added complexity in a project, while these patterns aim to solve this problem by controlling the creation process.

Some patterns that fall under this category are Constructor, Factory, Abstract, Prototype, Singleton, and Builder.

### Structural Design Patterns

Structural patterns are concerned with object composition and typically identify simple ways to realize relationships between different objects. They help ensure that when one part of a system changes, the entire structure of the system need not change. They also assist in recasting parts of the system that don’t fit a particular purpose into those that do.

Patterns that fall under this category include Decorator, Facade, Flyweight, Adapter, and Proxy.

### Behavioral Design Patterns

Behavioral patterns focus on improving or streamlining the communication between disparate objects in a system. They identify common communication patterns among objects and provide solutions that distribute the responsibility of communication among different objects, thereby increasing communication flexibility. Essentially, behavioral patterns abstract actions from objects that take the action.

Some behavioral patterns include Iterator, Mediator, Observer, and Visitor.

### Design Pattern Classes

| Creational       | Based on the concept of creating an object                                                    |
| ---------------- | --------------------------------------------------------------------------------------------- |
| **Class**        |                                                                                               |
| Factory method   | Makes an instance of several derived classes based on interfaced data or events               |
| **Object**       |                                                                                               |
| Abstract Factory | Creates an instance of several families of classes without detailing concrete classes         |
| Builder          | Separates object construction from its representation; always creates the same type of object |
| Prototype        | A fully initialized instance used for copying or cloning                                      |
| Singleton        | A class with only a single instance with global access points                                 |

| Structural | Based on the idea of building blocks of objects                                                             |
| ---------- | ----------------------------------------------------------------------------------------------------------- |
| **Class**  |                                                                                                             |
| Adapter    | Matches interfaces of different classes so that classes can work together despite incompatible interfaces   |
| **Object** |                                                                                                             |
| Bridge     | Separates an object’s interface from its implementation so that the two can vary independently              |
| Composite  | A structure of simple and composite objects that makes the total object more than just the sum of its parts |
| Decorator  | Dynamically adds alternate processing to objects                                                            |
| Facade     | A single class that hides the complexity of an entire subsystem                                             |
| Flyweight  | A fine-grained instance used for efficient sharing of information that is contained elsewhere               |
| Proxy      | A placeholder object representing the true object                                                           |

| Behavioral              | Based on the way objects play and work together                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Class**               |                                                                                                                        |
| Interpreter             | A way to include language elements in an application to match the grammar of the intended language                     |
| Template method         | Creates the shell of an algorithm in a method, then defers the exact steps to a subclass                               |
| **Object**              |                                                                                                                        |
| Chain of responsibility | A way of passing a request between a chain of objects to find the object that can handle the request                   |
| Command                 | A way to separate the execution of a command from its invoker                                                          |
| Iterator                | Sequentially accesses the elements of a collection without knowing the inner workings of the collection                |
| Mediator                | Defines simplified communication between classes to prevent a group of classes from referring explicitly to each other |
| Memento                 | Captures an object’s internal state to be able to restore it later                                                     |
| Observer                | A way of notifying change to a number of classes to ensure consistency between the classes                             |
| State                   | Alters an object’s behavior when its state changes                                                                     |
| Strategy                | Encapsulates an algorithm inside a class, separating the selection from the implementation                             |
| Visitor                 | Adds a new operation to a class without changing the class                                                             |

## JavaScript Design Patterns
