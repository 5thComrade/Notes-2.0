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

---
---

# 3: Sliding Window Pattern

## What is it?

The **Sliding Window** pattern is used to efficiently examine a **contiguous portion** of an array or string.

Instead of repeatedly calculating the same values for overlapping subarrays/substrings, we maintain a **window** and move it through the input.

Think:

```text
Array:   [2, 1, 5, 1, 3, 2]
             └─────┘
             window
```

The window "slides" from left to right.

---

## When should I use it?

Think **Sliding Window** when the problem mentions:

* **Contiguous** subarray
* **Substring**
* A range/window of elements
* Maximum/minimum sum of `k` elements
* Longest/shortest substring satisfying a condition
* "At most `k`..."
* "At least `k`..."
* "Without repeating characters"
* "Contains no more than..."

### Common clues

If you see:

> "Find the maximum sum of a subarray of size `k`."

Think:

**Fixed Sliding Window.**

If you see:

> "Find the longest substring with..."

Think:

**Dynamic Sliding Window.**

---

# Two Types of Sliding Windows

There are two common forms:

```text
1. Fixed Window
2. Dynamic Window
```

---

# 3.1. Fixed Sliding Window

The window always has the same size.

### Example

Find the maximum sum of `k` consecutive elements.

```text
[2, 1, 5, 1, 3, 2]
 └─────┘
   k=3

sum = 8
```

Then slide:

```text
[2, 1, 5, 1, 3, 2]
    └─────┘
     sum = 7
```

Instead of recalculating the entire window, we:

```text
Remove the element leaving the window
+
Add the element entering the window
```

---

## TypeScript

```ts
function maxSubarraySum(arr: number[], k: number): number | undefined {
  if (arr.length < k) {
    return undefined;
  }

  let windowSum = 0;

  // Build the first window
  for (let i = 0; i < k; i++) {
    windowSum += arr[i];
  }

  let maxSum = windowSum;

  // Slide the window
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i];
    windowSum -= arr[i - k];

    maxSum = Math.max(maxSum, windowSum);
  }

  return maxSum;
}

console.log(maxSubarraySum([2, 1, 5, 1, 3, 2], 3));
// 9
```

The windows are:

```text
[2, 1, 5] → 8
[1, 5, 1] → 7
[5, 1, 3] → 9  ← maximum
[1, 3, 2] → 6
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Why is this better than brute force?

A brute-force solution might calculate every window from scratch:

```ts
for each window:
    calculate its entire sum
```

That can result in **O(n × k)** time.

Sliding Window reuses the previous calculation:

```text
Previous window:
[2, 1, 5]
   ↓
remove 2
add 1
   ↓
[1, 5, 1]
```

So every element is processed only a small number of times.

```text
O(n)
```

---

# 3.2. Dynamic Sliding Window

The window size **changes** depending on a condition.

Usually you have:

```text
left  → start of window
right → end of window
```

The `right` pointer expands the window.

The `left` pointer shrinks it when the window violates the condition.

---

# Example: Longest Substring Without Repeating Characters

### Problem

Find the length of the longest substring without duplicate characters.

```text
"abcabcbb"

"abc" → valid
"bca" → valid
"cab" → valid
"abc" → valid
```

The answer is:

```text
3
```

We can maintain a window containing only unique characters.

```text
a b c a b c b b
└─────┘
 window
```

When we encounter a duplicate, move `left` until the window becomes valid again.

---

## TypeScript

```ts
function longestUniqueSubstring(str: string): number {
  const seen = new Set<string>();

  let left = 0;
  let maxLength = 0;

  for (let right = 0; right < str.length; right++) {
    while (seen.has(str[right])) {
      seen.delete(str[left]);
      left++;
    }

    seen.add(str[right]);

    maxLength = Math.max(
      maxLength,
      right - left + 1
    );
  }

  return maxLength;
}

console.log(longestUniqueSubstring("abcabcbb"));
// 3
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# The Dynamic Window Mental Model

Think of it like this:

```text
             right
               ↓
[a, b, c, d, e, f]
 ↑
left
```

### Expand

Move `right` forward:

```text
[a, b, c]
 ↑     ↑
left  right
```

### Condition violated?

Move `left` forward:

```text
[a, b, c, d]
    ↑     ↑
   left  right
```

Keep doing this until the window satisfies the condition again.

---

# Generic Dynamic Window Template

A very useful template to remember:

```ts
let left = 0;

for (let right = 0; right < arr.length; right++) {
  // Add arr[right] to the window

  while (/* window is invalid */) {
    // Remove arr[left] from the window
    left++;
  }

  // Window is valid here
  // Update answer
}
```

The important idea is:

```text
right → expand
left  → shrink
```

---

# Fixed vs Dynamic Window

| Type    | Window size | Typical problem                        |
| ------- | ----------- | -------------------------------------- |
| Fixed   | Always `k`  | Maximum sum of `k` elements            |
| Dynamic | Changes     | Longest substring satisfying condition |

### Fixed

```text
[─────]
  k
   ↓
slide →
```

### Dynamic

```text
[────────]
 ↑      ↑
left   right

condition violated
      ↓
   [─────]
    ↑   ↑
   left right
```

---

# How to Recognize Sliding Window

Ask yourself:

### 1. Is the problem about a contiguous section?

```text
subarray
substring
consecutive elements
range
```

If yes → **Sliding Window may apply.**

### 2. Is there a fixed size?

```text
"subarray of size k"
"every window of k elements"
```

→ **Fixed Sliding Window**

### 3. Does the window need to satisfy a condition?

```text
"longest substring without..."
"smallest subarray whose sum..."
"at most k distinct..."
```

→ **Dynamic Sliding Window**

---

# Sliding Window vs Multiple Pointers

These patterns can look very similar because both often use `left` and `right`.

The distinction is mainly **what the pointers represent**.

### Multiple Pointers

Usually focuses on **relationships between positions/elements**.

```text
[1, 2, 3, 4, 5, 6]
 ↑              ↑
left           right

Find a pair...
```

### Sliding Window

The pointers define a **contiguous range** that you are actively maintaining.

```text
[1, 2, 3, 4, 5, 6]
    ↑        ↑
   left    right

Current window = [2, 3, 4, 5]
```

A useful rule:

> **Contiguous range + optimize/validate that range → think Sliding Window.**

---

# Mental Model

```text
                Sliding Window
                      │
          ┌───────────┴───────────┐
          │                       │
       Fixed                    Dynamic
          │                       │
       size = k             expand/shrink
          │                       │
    max sum of k            longest substring
    consecutive items       without duplicates
```

## Remember

> **Sliding Window = maintain a contiguous range and slide it through the input instead of recalculating overlapping ranges from scratch.**

### Quick trigger

```text
Contiguous?
   ↓
Yes
   ↓
Fixed size? ── Yes ──→ Fixed Window
   │
   No
   ↓
Condition-based? ──→ Dynamic Window
```

---
---

# 4: Divide and Conquer Pattern

## What is it?

The **Divide and Conquer** pattern solves a problem by:

1. **Divide** the problem into smaller subproblems.
2. **Conquer** those smaller problems, usually recursively.
3. **Combine** their results to produce the final answer.

Think:

```text
             Problem
                │
          ┌─────┴─────┐
          ↓           ↓
       Smaller     Smaller
       Problem     Problem
          │           │
          ↓           ↓
        Solve       Solve
          │           │
          └─────┬─────┘
                ↓
             Combine
                ↓
             Answer
```

The key idea is:

> **Solve a large problem by solving smaller versions of the same problem.**

---

# When should I use it?

Think **Divide and Conquer** when:

* The problem can be broken into **independent smaller problems**
* Those smaller problems are similar to the original problem
* Solving each smaller problem helps solve the original
* The input can naturally be split into sections
* You can significantly reduce the search space at every step

### Common clues

If you see:

> "Search for an element in a sorted array."

Think:

**Binary Search → Divide and Conquer**

If you see:

> "Sort this array efficiently."

Think:

**Merge Sort / Quick Sort → Divide and Conquer**

If you see:

> "Find something in one half or the other."

Think:

**Divide and Conquer**

---

# Example: Binary Search

Binary Search is one of the simplest examples.

### Problem

Find a target value in a **sorted** array.

```text
[1, 3, 5, 7, 9, 11, 13]

target = 9
```

Instead of checking every element:

```text
1 → 3 → 5 → 7 → 9
```

Look at the middle:

```text
[1, 3, 5, 7, 9, 11, 13]
          ↑
        middle
```

`7 < 9`, so we know the target must be on the **right**.

We can completely discard the left half:

```text
[1, 3, 5, 7] | [9, 11, 13]
                ↑
             search here
```

Then repeat.

---

## TypeScript

```ts
function binarySearch(arr: number[], target: number): number {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    const middle = Math.floor((left + right) / 2);

    if (arr[middle] === target) {
      return middle;
    }

    if (arr[middle] < target) {
      left = middle + 1;
    } else {
      right = middle - 1;
    }
  }

  return -1;
}

console.log(binarySearch([1, 3, 5, 7, 9, 11, 13], 9));
// 4
```

### Complexity

```text
Time:  O(log n)
Space: O(1)
```

Every step cuts the search space roughly in half:

```text
n
↓
n / 2
↓
n / 4
↓
n / 8
↓
...
↓
1
```

That's why the time complexity is:

```text
O(log n)
```

---

# Example: Merge Sort

Merge Sort is a classic **Divide and Conquer** algorithm.

Given:

```text
[8, 3, 5, 4, 7, 6, 1, 2]
```

### Divide

Split the array:

```text
[8, 3, 5, 4]       [7, 6, 1, 2]
```

Split again:

```text
[8, 3] [5, 4]       [7, 6] [1, 2]
```

Continue until each array contains one element:

```text
[8] [3] [5] [4] [7] [6] [1] [2]
```

### Conquer + Combine

Now merge the small arrays in sorted order:

```text
[3, 8] [4, 5]       [6, 7] [1, 2]
```

Then:

```text
[3, 4, 5, 8]       [1, 2, 6, 7]
```

Finally:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

---

## TypeScript

```ts
function mergeSort(arr: number[]): number[] {
  if (arr.length <= 1) {
    return arr;
  }

  const middle = Math.floor(arr.length / 2);

  const left = mergeSort(arr.slice(0, middle));
  const right = mergeSort(arr.slice(middle));

  return merge(left, right);
}

function merge(left: number[], right: number[]): number[] {
  const result: number[] = [];

  let i = 0;
  let j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }

  return [
    ...result,
    ...left.slice(i),
    ...right.slice(j),
  ];
}

console.log(mergeSort([8, 3, 5, 4, 7, 6, 1, 2]));
// [1, 2, 3, 4, 5, 6, 7, 8]
```

### Complexity

```text
Time:  O(n log n)
Space: O(n)
```

---

# Divide and Conquer vs Recursion

These are **not the same thing**.

### Recursion

A function calls itself.

```ts
function countdown(n: number): void {
  if (n === 0) return;

  console.log(n);
  countdown(n - 1);
}
```

This is recursion, but it isn't necessarily Divide and Conquer.

### Divide and Conquer

The problem is broken into **smaller independent subproblems**.

```text
             Problem
             /     \
        Problem   Problem
          /          \
      smaller      smaller
```

So:

> **Divide and Conquer often uses recursion, but recursion does not necessarily mean Divide and Conquer.**

---

# Divide and Conquer vs Binary Search

Binary Search is an **example** of Divide and Conquer, not a separate competing pattern.

```text
Divide and Conquer
        │
        ├── Binary Search
        ├── Merge Sort
        └── Quick Sort
```

---

# How to Recognize It

Ask these questions:

### 1. Can I split the problem?

```text
Problem
   ↓
Problem A + Problem B
```

### 2. Are the smaller problems similar to the original?

For example:

```text
Sort 8 elements
   ↓
Sort 4 elements + Sort 4 elements
```

### 3. Can I combine their results?

```text
Result A + Result B
        ↓
   Final result
```

If the answer to these is **yes**, consider **Divide and Conquer**.

---

# Mental Model

```text
                Original Problem
                       │
                 ┌─────┴─────┐
                 ↓           ↓
              Problem      Problem
                 │           │
            ┌────┴───┐   ┌───┴────┐
            ↓        ↓   ↓        ↓
          Small    Small Small   Small
            │        │    │        │
            └────┬───┘    └───┬────┘
                 ↓             ↓
               Solve         Solve
                 └──────┬──────┘
                        ↓
                     Combine
                        ↓
                     Answer
```

---

# Common DSA Problems

When practicing, common Divide and Conquer examples include:

* **Binary Search**
* **Merge Sort**
* **Quick Sort**
* Finding an element in a sorted structure
* Problems where each step eliminates a large portion of the search space

---

# Quick Comparison With Other Patterns

| Pattern           | Main idea                   | Common clue          |
| ----------------- | --------------------------- | -------------------- |
| Frequency Counter | Count occurrences           | "How many times?"    |
| Multiple Pointers | Move multiple indexes       | Sorted array / pairs |
| Sliding Window    | Maintain a contiguous range | Subarray / substring |
| Divide & Conquer  | Split into smaller problems | Search/sort/split    |

---

## Remember

> **Divide and Conquer = break a problem into smaller versions of itself, solve those smaller problems, then combine their results.**

### Quick trigger

```text
Can I divide the problem?
        ↓
Are the pieces similar?
        ↓
Can I solve them independently?
        ↓
Can I combine the results?
        ↓
Divide & Conquer
```
