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

# Lesson 2 JavaScript Concurrency: setTimeout and functions

setTimeout works in the same way when we wrap our code in functions, classes, or any other language construct.

1.
```js
function doAsyncWork() {
  const array = [];
  setTimeout(() => array.push('it worked'), 1000);
  return array;
}
const array = doAsyncWork();
array;
 ASYNC RESULT:
['it worked']
```

As our examples get more complex, we'll want to extract our asynchronous callbacks into their own functions. Here's an example of that: we asynchronously collect users' names in an array.

2.
```js
const users = [
  {name: 'Amir'},
  {name: 'Betty'},
];

function addUserName(names, user) {
  names.push(user.name);
}

function doAsyncWork() {
  const names = [];
  for (const user of users) {
    setTimeout(() => addUserName(names, user), 1000);
  }
  return names;
}

const array = doAsyncWork();
array;
 ASYNC RESULT:
['Amir', 'Betty']
```

Extracting callbacks like this makes changing code easier. If we're tracking down a bug where asynchronous code seems to execute in the wrong order, we'll start by looking at doAsyncWork. But if we're fixing a bug in the work itself, we'll start by looking at addUserName.

(Of course, real-world equivalents of addUserName will be more than one line long. Imagine that function being 10 lines long instead of 1!)

Here's a code problem:

Set a timer to call the addMessage function after 100 ms.

(This example uses .slice() to duplicate the messages array's value before any timers run. That lets us check the value of the array before and after the timer fires.)

3.
```js
const messages = [];
function addMessage() {
  messages.push('It worked!');
}
setTimeout(addMessage, 100);
const initialMessages = messages.slice();
[initialMessages, messages];

 ASYNC RESULT:
GOAL:
[[], ['It worked!']]
YOURS:
[[], ['It worked!']]
```

# Lesson 3 JavaScript Concurrency: Concurrent setTimeouts

setTimeout can have a delay of 0, which means "do this immediately". However, it's still asynchronous! Whenever we use a setTimeout with any delay, even 0, the JavaScript engine will finish running all the synchronous code before calling the setTimeout callback.

```js
console.log('first');
setTimeout(() => console.log('second'), 0);
console.log('third');

 async console output
  37 ms  first
  38 ms  third
  42 ms  second
```

Timers are asynchronous regardless of how many we create. When we create 3 timers, each with a delay of 0, they will only run once the current code finishes. Timers with a delay of 0 will always execute in the order that they were created.

(In this example, remember that 'first' and 'third' will be pushed onto the array before the timers run.)

1.
```js
const array = [];
array.push('first');
for (const i of [1, 2]) {
  setTimeout(() => array.push('second ' + i), 0);
}
array.push('third');
array;
 ASYNC RESULT:
['first', 'third', 'second 1', 'second 2']
```

Since the timeout is 0, 'second 1' was pushed to the array immediately after the other code finished running. What happens when we create timers with timeouts greater than 0? Each will fire when its timeout is reached. In the example below, the 'cat' timer will fire before the 'dog' timer.

```js
setTimeout(() => console.log('cat'), 1000);
setTimeout(() => console.log('dog'), 2000);
 async console output
1017 ms  cat
2019 ms  dog
```

The order of the setTimeout calls in the code doesn't matter here. If we switch their order, we'll get the same result: the timer with the shorter delay will fire first.

```js
setTimeout(() => console.log('dog'), 2000);
setTimeout(() => console.log('cat'), 1000);
 async console output
1007 ms  cat
2009 ms  dog
```

In those examples, we logged 'cat' after 1000 ms, then logged 'dog' after 2000 ms. It's important to note that the total execution time was about 2000 ms, not 3000 ms! The timers begin at the same time, and then fire about 1000 ms and 2000 ms from when they were scheduled.

The setTimeout calls below reference DELAY1, DELAY2, and DELAY3 variables that don't exist. Replace those variable references with 100, 200, and 300 to get the timers to run in an order that produces the expected array result.

2.
```js
const array = [];
3
setTimeout(() => array.push('cat'), 300);
setTimeout(() => array.push('dog'), 100);
setTimeout(() => array.push('horse'), 200);
array;
 ASYNC RESULT:
GOAL:
['dog', 'horse', 'cat']
YOURS:
['dog', 'horse', 'cat']
```

In our examples using console.log, the timing is never quite exact. The next example sets a timer for 1000 ms, but the actual time will be slightly longer when you run it:

```js
setTimeout(() => console.log('finished'), 1000);

 async console output
1061 ms  finished
```

When we set a 1000 ms timer, it means "wait 1000 ms, then call my function as soon as possible." Computers run at finite speeds, so we have no guarantees about how soon our timers will run. If a lot of other code is waiting to run, a 1000 ms timer could take 30 seconds instead of 1 second! (But you won't encounter anything that extreme in most systems.)

We can create that situation artificially by loading the CPU. The next example sets a timer, then runs a "busy loop" for 500 ms: the loop does nothing except take up time. The JavaScript runtime can't execute multiple blocks of code simultaneously, so this prevents it from moving on for about 500 ms.

```js
const now = () => new Date().getTime();
const startTime = now();

setTimeout(() => console.log('cat'), 0);
setTimeout(() => console.log('dog'), 1000);

while (now() < startTime + 500) {
  // Do nothing; we're wasting 500 ms on purpose.
}

 async console output
 533 ms  cat
1035 ms  dog
```

Unless your computer was very busy doing other things, your first console log should have come out around 500 ms. But it was scheduled for 0 ms! That shows that the timer couldn't fire while our 500 ms "busy loop" was running. However, the second setTimeout should have fired at roughly 1000 ms, since there was nothing blocking it.

This brings us to an important point: JavaScript runs one piece of code at a time. (There are some situations where this isn't true, but they're not applicable to this course.) When we write concurrent code, we schedule different code to run at some point in the future, like our console.log calls above. But only one piece of code is actually running at any given time.

This explains why our "0-second" setTimeout took about 500 ms to run. Our setTimeout call told the JavaScript engine to "please run this code as soon as possible." But "as soon as possible" had to wait for our 500 ms busy loop to finish.

The JavaScript engine maintains a queue of timers and other events, processing them in order. We can think of it like this:

```js
while (true) {
  for (const timer of eventQueue) {
    if (timer.isPastItsScheduledTime()) {
      timer.runCallback();
      eventQueue.removeTimer(timer);
    }
  }
}
```

When a timer's callback is fired, it begins, runs, and finishes, all without being interrupted. Then the JavaScript engine checks again. If there are other timers ready, they run.

Real JavaScript engines are more complex than the pseudocode above, but thinking about it in this way will get us surprisingly far!

# Lesson 4

# Lesson5