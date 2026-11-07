# Evaluation 02: A setTimeout loop with var

**Area:** JavaScript · **Date:** 2026-11-07
**Response A:** ChatGPT · **Response B:** Gemini

## Question
This JavaScript loop is meant to print 1, 2 and 3, one second apart. What does it actually print, and why? How would you fix it?

```js
for (var i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 1000 * i);
}
```

## Response A
````text
The code actually prints:
4
4
4
Each output appears at approximately one-second intervals (1s, 2s, and 3s).
Why does this happen?
The problem is the use of var.
* var is function-scoped, not block-scoped.
* All three setTimeout() callbacks share the same variable i.
* The for loop executes immediately without waiting for the timers.
* Once the loop finishes, i has become 4.
* When the callbacks execute, they all read the current value of i, which is 4.
Fix 1: Use 
let
(recommended)
for (let i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 1000 * i);
}
Output:
1  // after 1 second
2  // after 2 seconds
3  // after 3 seconds
Why does this work?
Unlike var, let creates a new binding for each iteration of the loop. Each callback retains access to its own value of i.
Fix 2: Use an IIFE (Immediately Invoked Function Expression)
for (var i = 1; i <= 3; i++) {
  ((num) => {
    setTimeout(() => console.log(num), 1000 * num);
  })(i);
}
Here, each iteration passes the current value of i into a separate function, preserving it as num.
Key lesson
Keyword	Scope	Behaviour in this loop
var	Function-scoped	All callbacks share one variable
let	Block-scoped	Each iteration gets its own binding
const	Block-scoped	Cannot be reassigned; unsuitable for this incrementing loop variable
My recommendation: Use let for loop counters when working with asynchronous callbacks such as setTimeout, promises, and event handlers.
Understanding the relationship between closures, scope and asynchronous JavaScript is essential for avoiding bugs like this.
````

## Response B
````text
What it actually prints
Instead of printing 1, 2, and 3, this code prints 4 three separate times, spaced one second apart (at 1 second, 2 seconds, and 3 seconds).
Why this happens
This behavior comes down to two things: variable scoping with var and asynchronous event handling.
1. var is function-scoped (not block-scoped): When you declare a variable with var inside a for loop, that variable exists outside the loop block as well. There is only one shared variable i for the entire loop.
2. Synchronous vs. Asynchronous execution: The for loop runs synchronously and finishes almost instantly (in microseconds). By the time the first setTimeout callback finally executes after 1 second, the loop has already completed, and i has incremented to 4 (the value that caused the loop condition i <= 3 to fail).
3. Closure reference: The arrow functions inside setTimeout create closures that capture a reference to the variable i, not its value at the time the timeout was created. When the callbacks run, they all look up the shared variable i and see its final value: 4.
How to fix it
Here are the three most common ways to fix this issue, starting with the modern, cleanest approach:
1. Replace var with let (Recommended)
Switching var to let introduces block scope. let creates a brand-new binding for i on every single iteration of the loop, preserving the correct value for each closure.
for (let i = 1; i <= 3; i++) {
  setTimeout(() => console.log(i), 1000 * i);
}
2. Pass the value directly to setTimeout
setTimeout accepts additional arguments after the delay, which are passed directly into your callback function. This captures the value of i immediately at the time the timeout is scheduled.
for (var i = 1; i <= 3; i++) {
  setTimeout((j) => console.log(j), 1000 * i, i);
}
3. Use an IIFE (The classic ES5 workaround)
If you are stuck working in older environments where let isn't available, you can wrap the setTimeout in an Immediately Invoked Function Expression (IIFE) to create a new local scope for each iteration.
for (var i = 1; i <= 3; i++) {
  (function(j) {
    setTimeout(() => console.log(j), 1000 * j);
  })(i);
}
````

## How I checked
I ran the original loop in Chrome's console, and it printed 4 three times. With `let` instead of `var`, it printed 1, 2 and 3. I ran both of A's fixes and the first two of B's, and each printed 1, 2 and 3. I didn't run B's third fix, but its code uses an arrow function: MDN's page on [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) shows they've only worked across browsers since 2016, so it won't run in the older environments B describes. I compared the explanations with MDN's page on [the for statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for), which covers this exact case.

## Scores (1 to 5)
| Dimension | A | B |
| --- | --- | --- |
| Correctness | 5 | 4 |
| Completeness | 5 | 5 |
| Reasoning | 5 | 5 |
| Safety and security | 5 | 5 |
| Clarity | 5 | 4 |

## Verdict
Preferred: A. Both explain the bug and give working fixes, but A is fully correct and clearer, while B makes one error.

## Explanation
Both responses correctly say the loop prints 4 three times, one second apart, and every fix I ran printed 1, 2 and 3. Response B explains the cause in more depth, noting that each callback captures a reference to `i` rather than its value, and it offers an extra fix that passes `i` to `setTimeout` as an argument. However, B makes one error: it calls its third fix "the classic ES5 workaround" for environments without `let`, but the code uses an arrow function, which those older environments don't support either. Response A is fully correct and easier to follow, ending with a short table that sums up how `var`, `let` and `const` behave, so I prefer A.
