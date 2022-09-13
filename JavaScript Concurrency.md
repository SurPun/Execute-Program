## JavaScript Concurrency

# Lesson 1 JavaScript Concurrency: setTimeout

This course covers concurrency, which means "two or more events happening at the same time." No concurrency knowledge is necessary, but you should know basic JavaScript, like:

1. null and undefined.
2. Variable definitions with var, let, and const.
3. Conditionals (if) and ternary conditionals (a ? y : b).
4. C-style for loops: for (let i=0; i<10; i++) { ... }.
5. Regular functions: function f() { ... }.
6. Arrow functions: const f = () => { ... }.
7. Class constructors: new Error(...)

We'll begin by looking at some synchronous code, then we'll see how asynchronous code is different.

JavaScript's map method is synchronous. It takes a callback function, which it calls for every element of the array. It then returns a new array containing the result of callback(x) for every x in the original array.

```js
[1, 2, 3].map(x => x + 1);
RESULT:
[2, 3, 4]
```

1.
```js
[1, 2, 3].map(x => x * 2);
RESULT:
[2, 4, 6]
```

The map function is synchronous because it always processes elements one at a time, in order. When it returns, the result array is fully constructed.

Asynchronous code is different: it doesn't run all at once, from beginning to end. Instead, async (asynchronous) code schedules other code to run in the future. (In the context of code, asynchronous means "outside of normal execution order".)

In JavaScript, the simplest way to run code asynchronously is with setTimeout. When we call setTimeout(someFunction, 1000), we're telling the JavaScript runtime to call someFunction 1000 milliseconds (1 second) in the future.

The next example prints two lines to the console: one immediately, and one after a delay created by setTimeout. You won't need to open your browser's built-in console to see the output. We'll show it right here, along with a timestamp for each line.

```js
console.log('first');
setTimeout(() => console.log('second'), 2000);

 async console output
  18 ms  first
```

Here's what happens in that example code:

1. The first console.log runs and prints to the console.
2. setTimeout creates a timer that will call our callback after 2000 ms have passed.
3. Both lines of code (console.log and setTimeout) have run, so execution ends.
4. After 2000 ms, the timeout fires and calls its callback, which prints 'second'. (You might have noticed that sometimes the time difference in the example above isn't exactly 2000 ms. We'll talk about why that happens in a future lesson.)
5. The browser's JavaScript engine sees that there are no timers left, so the example finishes.

An important thing to note is that our code finishes running, then the CPU sits idle for a while, and then our setTimeout callback function runs. We can see that by adding one more console.log call. Pay close attention to the order of the console output!

```js
console.log('first');
setTimeout(() => console.log('second'), 2000);
console.log('third');

 async console output
   4 ms  first
   5 ms  third
2007 ms  second
```

The console.log calls don't happen in the order they were written! First, all three lines are executed. The second line uses setTimeout to schedule our callback function to run in 2 seconds. Then the CPU sits idle for about 2 seconds, and then the callback runs and prints 'second'.

Note that the examples above say "async console output". When an example says "async console output" or "async result", Execute Program will wait for asynchronous code like timeouts to finish running.

Let's examine the order of operations in one more way. What if we run the code, but then stop the code example immediately, without waiting for any asynchronous code to run? In that case, we won't see 'second'. We'll only see 'first' and 'third'.

```js
console.log('first');
setTimeout(() => console.log('second'), 2000);
console.log('third');

/* We'll stop running the code here, as soon as the last line is printed. */
console output
  19 ms  first
  21 ms  third
```

Notice that this example simply says "console output", rather than "async console output". Most code examples in this course wait for asynchronous code and have an "async result" or "async console output" reminder. But some examples don't wait for async code to run, like the one above. In that case, the example will simply say "result" or "console output".

Sometimes this course will use the console.log function shown above. But sometimes we'll instrument our code in other ways to see what it's doing. (The verb "instrument" means "attach measurement instruments to".)

In the next example, we instrument setTimeout with an array instead of console.log calls. We append to the array three times: before calling setTimeout, in the setTimeout callback itself, and after calling setTimeout. Your answer should match what the array will look like after the setTimeout has run.

1.
```js
const array = [];
array.push('first');
setTimeout(() => array.push('second'), 2000);
array.push('third');
array;
 ASYNC RESULT:
['first', 'third', 'second']
```

Concurrency is tricky, so two quick notes. First, remember that most code examples in this course will wait for asynchronous code to run, like the example above did. That gives our timeout a chance to run. Examples that wait for asynchronous code will always say "async result" or "async console output".

Second, you may see different results if you run these examples in your browser's console. That's because the browser doesn't wait for the asynchronous code to finish. To see the same result as above, run every line of code except the last line, then wait 3 seconds, then run the last line. That gives the timeout a chance to run.

In the next example, we'll stop executing the code immediately, without waiting for the async code to run, just as we did before with console.log. It will give us the array exactly as it existed when the code first finished executing. At that point, the setTimeout callback hasn't run yet, so 'second' hasn't been pushed onto the array.

(You can tell that this example doesn't wait because the prompt says "result" instead of "async result".)

2.
```js
const array = [];
array.push('first');
setTimeout(() => array.push('second'), 2000);
array.push('third');
array;
RESULT:
['first', 'third']
```

This is a very important point: when that code finishes executing, 'second' isn't in the array! The 'second' string only shows up later, once we've waited for the setTimeout callback to happen after about 2000 ms.

Those are the basics of callback-based asynchronous programming! The rest of this course will cover:

- More complex callback situations.
- Promises (which are built on top of callbacks).
- Async/await (which are built on top of promises).
- Asynchronous programming subtleties like error handling and event scheduling.

# Lesson 2

# Lesson 3

# Lesson 4

# Lesson5