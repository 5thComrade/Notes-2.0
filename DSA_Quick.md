# 1: Frequency Counter Pattern

## What is it?

The **Frequency Counter** pattern is a technique where you use a data structure (usually a **hash map / object / `Map`**) to count how many times each value occurs.

Instead of repeatedly searching through an array or string, you **pre-process the input into frequencies**, allowing you to solve many problems in **O(n)** time.

### Core idea

> **Count first → compare/use the counts → avoid nested loops.**

---

## When should I use it?

Look for problems involving:

* Counting occurrences of values
* Comparing two arrays or strings
* Checking whether two things contain the same elements
* Anagrams
* Duplicates
* Character frequencies
* Checking whether one collection is a permutation of another
* Problems where you are repeatedly asking:

  * "How many times does this appear?"
  * "Does this value exist?"
  * "Do both inputs have the same counts?"

### Common clue

If you see something like:

> "Given two arrays/strings, determine whether they contain the same elements with the same frequency."

Think:

**Frequency Counter.**

---

## Example: Are Two Strings Anagrams?

### Problem

Given two strings, determine whether they are anagrams.

```text
"listen"
"silent"

→ true
```

```text
"hello"
"world"

→ false
```

The characters must appear with the **same frequency**.

### Naive approach

For every character in the first string, search the second string and count occurrences.

This can lead to **O(n²)** time.

### Frequency Counter approach

Count the characters in each string and compare the counts.

```ts
function isAnagram(str1: string, str2: string): boolean {
  if (str1.length !== str2.length) {
    return false;
  }

  const frequency1 = new Map<string, number>();
  const frequency2 = new Map<string, number>();

  for (const char of str1) {
    frequency1.set(char, (frequency1.get(char) ?? 0) + 1);
  }

  for (const char of str2) {
    frequency2.set(char, (frequency2.get(char) ?? 0) + 1);
  }

  for (const [char, count] of frequency1) {
    if (frequency2.get(char) !== count) {
      return false;
    }
  }

  return true;
}
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

We make a few linear passes over the strings instead of repeatedly searching through them.

---

## Even simpler: One Frequency Counter

You don't always need two counters.

```ts
function isAnagram(str1: string, str2: string): boolean {
  if (str1.length !== str2.length) {
    return false;
  }

  const frequency = new Map<string, number>();

  for (const char of str1) {
    frequency.set(char, (frequency.get(char) ?? 0) + 1);
  }

  for (const char of str2) {
    const count = frequency.get(char);

    if (count === undefined) {
      return false;
    }

    if (count === 1) {
      frequency.delete(char);
    } else {
      frequency.set(char, count - 1);
    }
  }

  return frequency.size === 0;
}
```

The idea is:

```text
str1: "aabbc"

Frequency:
a → 2
b → 2
c → 1

Then consume those counts using str2.
```

---

## Generic Pattern

For an array:

```ts
const frequency = new Map<number, number>();

for (const value of arr) {
  frequency.set(value, (frequency.get(value) ?? 0) + 1);
}
```

For a string:

```ts
const frequency = new Map<string, number>();

for (const char of str) {
  frequency.set(char, (frequency.get(char) ?? 0) + 1);
}
```

Then use the frequency map to answer questions efficiently:

```ts
frequency.get(value)
frequency.has(value)
```

---

## Example: Are Two Arrays the Same?

The arrays must contain the same values with the same frequencies.

```ts
function same(arr1: number[], arr2: number[]): boolean {
  if (arr1.length !== arr2.length) {
    return false;
  }

  const frequency = new Map<number, number>();

  for (const value of arr1) {
    frequency.set(value, (frequency.get(value) ?? 0) + 1);
  }

  for (const value of arr2) {
    const count = frequency.get(value);

    if (count === undefined) {
      return false;
    }

    if (count === 1) {
      frequency.delete(value);
    } else {
      frequency.set(value, count - 1);
    }
  }

  return frequency.size === 0;
}

console.log(same([1, 2, 3], [3, 1, 2]));
// true

console.log(same([1, 2, 2], [1, 1, 2]));
// false
```

---

## Mental Model

When you see:

```text
"How many times does X occur?"
"Do these two collections contain the same things?"
"Do these characters have the same frequencies?"
"Is this an anagram?"
```

Think:

```text
Input
  ↓
Build frequency map
  ↓
Use frequency map to compare / lookup
  ↓
O(n)
```

### Remember

> **Frequency Counter = use a hash map to turn repeated counting/searching into O(1) lookups.**

### Typical data structures

In TypeScript:

```ts
Map<K, number>
```

or, for simple string keys:

```ts
Record<string, number>
```

For DSA problems, `Map` is generally the cleaner default.

---
---

# 2: Multiple Pointers Pattern

## What is it?

The **Multiple Pointers** pattern uses **two or more pointers** to move through a data structure, usually an **array or string**, instead of repeatedly searching through it.

The pointers can move:

* Toward each other
* In the same direction
* At different speeds
* Based on some condition

The goal is usually to solve a problem in **O(n)** time instead of **O(n²)**.

---

## When should I use it?

Think **Multiple Pointers** when:

* You have a **sorted array**
* You need to find a **pair** that satisfies a condition
* You need to compare values from different positions
* You need to remove duplicates
* You need to reverse or rearrange elements
* You need to find something from **both ends**
* You can eliminate part of the search space by moving a pointer

### Common clues

If you see:

> "Given a sorted array, find two values..."

Think:

**Multiple Pointers.**

If you see:

> "Find a pair whose sum is..."

Think:

**Two pointers.**

---

# Example: Pair With Zero Sum

### Problem

Given a **sorted** array, find a pair whose sum is `0`.

```text
[-4, -3, -2, -1, 0, 1, 2, 5]

          ↑           ↑
        left         right
```

Start with pointers at both ends.

```text
left  = 0
right = arr.length - 1
```

Calculate:

```text
arr[left] + arr[right]
```

Then:

* If sum is `0` → found the pair
* If sum is **greater than `0`** → move `right` left
* If sum is **less than `0`** → move `left` right

Why?

Because the array is **sorted**.

---

## TypeScript

```ts
function sumZero(arr: number[]): [number, number] | undefined {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    const sum = arr[left] + arr[right];

    if (sum === 0) {
      return [arr[left], arr[right]];
    }

    if (sum > 0) {
      right--;
    } else {
      left++;
    }
  }

  return undefined;
}

console.log(sumZero([-4, -3, -2, -1, 0, 1, 2, 5]));
// [-2, 2]
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

Each pointer only moves forward through the array.

---

# Why is this better than a nested loop?

### Brute force

```ts
for (let i = 0; i < arr.length; i++) {
  for (let j = i + 1; j < arr.length; j++) {
    // check pair
  }
}
```

```text
Time: O(n²)
```

### Multiple pointers

```ts
let left = 0;
let right = arr.length - 1;

while (left < right) {
  // ...
}
```

```text
Time: O(n)
```

The sorted nature of the array allows us to **eliminate many possibilities without checking them**.

---

# Example: Check for Palindrome

Multiple pointers aren't limited to sorted arrays.

A palindrome reads the same forwards and backwards.

```text
"racecar"

r →       ← r
 a →   ← a
  c → ← c
   e
```

Use one pointer at the beginning and one at the end.

```ts
function isPalindrome(str: string): boolean {
  let left = 0;
  let right = str.length - 1;

  while (left < right) {
    if (str[left] !== str[right]) {
      return false;
    }

    left++;
    right--;
  }

  return true;
}

console.log(isPalindrome("racecar"));
// true

console.log(isPalindrome("hello"));
// false
```

```text
Time:  O(n)
Space: O(1)
```

---

# Same Direction Pointers

Multiple pointers can also move in the **same direction**.

A common example is removing duplicates from a sorted array.

```text
[1, 1, 2, 2, 3, 4]

 ↑
write

 ↑
read
```

One pointer tracks where the next unique value should go, while another scans the array.

```ts
function removeDuplicates(arr: number[]): number {
  if (arr.length === 0) {
    return 0;
  }

  let write = 1;

  for (let read = 1; read < arr.length; read++) {
    if (arr[read] !== arr[read - 1]) {
      arr[write] = arr[read];
      write++;
    }
  }

  return write;
}
```

---

# Mental Model

When you see a problem involving an array/string, ask:

```text
Can I place a pointer at the beginning?
Can I place another pointer at the end?
Can I move them based on a condition?
```

Or:

```text
Can I have one pointer read/scan
while another pointer tracks a position?
```

If yes, **Multiple Pointers** may be the right pattern.

---

# Quick Decision Guide

| Problem clue                        | Pattern           |
| ----------------------------------- | ----------------- |
| Count occurrences                   | Frequency Counter |
| Sorted array + find a pair          | Multiple Pointers |
| Compare beginning and end           | Multiple Pointers |
| Palindrome                          | Multiple Pointers |
| Remove duplicates from sorted array | Multiple Pointers |
| Contiguous subarray / substring     | Sliding Window    |
| Repeatedly count/search values      | Frequency Counter |

---

## Remember

> **Multiple Pointers = use multiple indexes to intelligently traverse a data structure, eliminating unnecessary comparisons.**

The biggest clue is often:

> **"The input is sorted" + "find/compare a pair" → think Multiple Pointers.**

