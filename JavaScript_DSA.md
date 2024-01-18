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

