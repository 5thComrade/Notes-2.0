# Frequency Counter Pattern

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
