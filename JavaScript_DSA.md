# JavaScript Data Structures and Algorithms

## Big O Notation

Imagine we have multiple implementations of the same function. How can we determine which one is the best?

It's important to have a precise vocabulary to talk about how our code performs. Useful for discussing trade-offs between different approaches.

Lets write a function that takes a number as its parameter and returns the sum of all the numbers from 1 to n(including n)

- Method 1
```js
const sumUpTo_v1 = (n) => {
    let total = 0;
    for(let i = 1; i <= n; i++) {
        total += i
    }

    return total;
}

let t1 = performance.now();

sumUpTo_v1(10000000)

let t2 = performance.now();

console.log(`Time lapsed ${(t2 - t1)/1000} seconds.`)
```

- Method 2
```js
const sumUpTo_v2 = (n) => {
    return n * (n + 1) / 2
}

let t1 = performance.now();

sumUpTo_v2(10000000)

let t2 = performance.now();

console.log(`Time lapsed ${(t2 - t1)/1000} seconds.`)
```

Rather than counting seconds which can vary from machine to machine, lets count the simple operations the computer has to perform.

Big O allows us to talk formally about how the runtime of an algorithm grows as the input grows.

**We say that an algorithm is O(f(n)) if the number of simple operations the computer has to do is eventually less than a constant times f(n), as n increases.**

Therefore in the above code example, Method 1 has O(n) and Method 2 has O(1).

### Simplifying Big O Expressions

1. Constants don't matter
   
    O(2n) -> O(n)
   
    O(500) -> O(1)
   
    O(13n<sup>2</sup>) -> O(n<sup>2</sup>)

2. Smaller terms don't matter

   O(n + 10) -> O(n)

   O(1000n + 50) -> O(n)

   O(n<sup>2</sup> + 5n + 8) -> O(n<sup>2</sup>)

### Big O Shorthands

1. Arithmetic operations are constant
2. Variable assignment is constant
3. Accessing elements in an array(by index) or object(by key) is constant
4. In a loop, the complexity is the length of the loop times the complexity of whatever happens inside of the loop.
---
