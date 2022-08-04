# Modern JavaScript

## Lesson 1 Modern JavaScript: Strict mode

This course covers JavaScript as it exists today. It assumes basic familiarity with JavaScript:

You should know how to define a function and write an if.
You should also have seen the this keyword before, but you don't need to know about its confusing quirks (there are many of them).
You should know common array operations, like map and filter. If you need to learn about these, we teach them in our Javascript Arrays course!

Historically, variable scoping was a sore point in JavaScript. The straightforward assignment syntax, x = 1, defines a global variable, which we rarely want!

Fortunately, modern versions of JavaScript have "strict mode". It prevents many kinds of mistakes, including global variable definitions like x = 1. Strict mode is enabled by putting the string 'use strict' at the top of a module or function.

Babel, TypeScript, and most other JavaScript compilers automatically insert 'use strict' for us. This course does that as well: all code here runs in strict mode. If we try to define a variable with x = 1, we'll get an error. (You can type error when a code example will throw an error.)

1.
```js
function defineX() {
  x = 1;
  return 'ok';
}
defineX();
RESULT:
ReferenceError: x is not defined
```

Note that we didn't reference x after trying to define it. The ReferenceError is happening simply because we said x = 1.

This isn't a flashy part of JavaScript, so why start a "Modern JavaScript" course here? Because strict mode marks the beginning of a decade-long (and continuing) push to modernize the JavaScript language. The first step in modernization was to disallow some of the old, confusing behavior, like x = 1 creating a global variable.

We'll look at a couple more strict mode examples before moving on to other new features in JavaScript.

JavaScript supports numbers in octal (base 8). For example, 0100 in octal is 64 in base 10. However, octal is rarely used; if you type 0100, it's more likely that you meant 100! To prevent that mistake, all uses of octal cause errors in strict mode.

2.
```js
64 === 0100
RESULT:
SyntaxError: on line 1: Legacy octal literals are not allowed in strict mode.
```

One final strict mode example. The delete keyword can be used to completely remove a key from an object.

3.
```js
const user = {name: 'Amir', age: 36};
delete user.age;
user;
RESULT:
{name: 'Amir'}
```

In the past, the delete keyword could also delete entire variables: delete user. However, in strict mode, trying to do that causes an error.

4.
```js
const user = {name: 'Amir', age: 36}
delete user
RESULT:
SyntaxError: on line 2: Deleting local variable in strict mode.
```

Certain object properties are also undeletable. For example, trying to delete the Object.prototype property will cause an error. (Deleting that property would wreak havoc on JavaScript's prototype-based object system.)

5.
```js
delete Object.prototype;
RESULT:
TypeError: Cannot delete property 'prototype' of function Object() { [native code] }
```

## Lesson 2 Modern JavaScript: Let

Early in JavaScript's life, local variables were declared with the var keyword. When we define a var inside a function, it's only visible inside the function. We're allowed to use var in strict mode.

1.
```js
function defineX() {
  var x = 1;
  return 'ok';
}
defineX();
RESULT:
'ok'
```

However, trying to reference the variable outside of the function is an error.

Reference x outside of the function, which will cause an error. (console.log(x) is a fine choice.)


2.
```js
function defineX() {
  var x = 1;
}
defineX();
console.log(x);
GOAL:
ReferenceError: x is not defined
YOURS:
ReferenceError: x is not defined
```

Functions create a variable scope. Inside the function, variables defined in the function are visible. Outside the function, the function's variables are invisible. This is good!

All vars are "function-scoped", which means that they're visible to the entire function body. What if we put a var inside an if (...) { ... }? It seems like the variable should be visible inside the if block, which it is:

3.
```js 
function f() {
  if (true) {
    var x = 1;
    return x;
  }
}
f();
RESULT:
1
```

However, the variable is also visible to the rest of the function body, even outside of the if block!

Modify the function to return x.

4.
```js
function f() {
  if (true) {
    var x = 1;
  }
  return x;
}
f();
GOAL:
1
YOURS:
1
```

This was a mistake in JavaScript's design: var x = 1 inside an if shouldn't be visible outside the if. It's too easy to declare a variable, thinking that it will be local to the if, then accidentally use it later in the function.

Fortunately, this problem was fixed in 2015, when let was introduced. With let, a variable defined inside the if isn't visible outside the if. Trying to access it will cause an error. (You can type error when a code example will throw an error.)


5.
```js
function f() {
  if (true) {
    let x = 1;
  }
  return x;
}
f();
RESULT:
ReferenceError: x is not defined
```

let handles nested scopes properly. For example, we can define an x in the function body, then define another x inside an if. Changing the "inner" x won't change the "outer" x.


6.
```js

function f() {
  let x = 'outer';
  if (true) {
    let x = 'inner';
  }
  return x;
}
f();
RESULT:
'outer'
```

Those variables hold different values even though they have the same name. That's called "shadowing": the inner let x shadows the outer let x.

We've been using if for our examples, but these rules apply to any block of code in { curly braces }. For example, an outer scope can't access a variable defined inside a while; that causes an error.

7.
```js
function f() {
  let iterating = true;
  while (iterating) {
    let x = 1;
    iterating = false;
  }
  return x;
}
f();
RESULT:
ReferenceError: x is not defined
```

We can also introduce new scopes without using a construct like if or while. Any code contained in { curly braces } will have its own scope.


8.
```js
function f() {
  let x = 'outer';
  {
    let x = 'inner';
  }
  return x;
}
f();
RESULT:
'outer'
```

## Lesson 3 Modern JavaScript: Const

Sometimes we want to define a variable and ensure that it won't change. We can do that with const, which is like let except that a const variable can never be reassigned. If we try to assign a new value to it, that's an error. (You can type error when a code example will throw an error.)

1.
```js
function f() {
  const x = 1;
  return x;
}
f();
RESULT:
1
```

2.
```js
function f() {
  const x = 1;
  x = 2;
  return x;
}
f();
RESULT:
TypeError: Assignment to constant variable.
```

3.
```js
function f() {
  const x = 1;
  x = 2;
  return x;
}
f();
GOAL:
TypeError: Assignment to constant variable.
YOURS:
TypeError: Assignment to constant variable.
```

const doesn't stop us from mutating (changing) the value held by the variable. For example, we can mutate a const array by calling its push method. However, reassigning the array variable to hold a new array isn't allowed.

Use xs.push(...) to push a 3 onto the array. That's allowed even though we declared xs with const.

4.
```js
function f() {
  const xs = [1, 2];
xs.push(3);
  xs.push(3);
  return xs;
}
f();
GOAL:
[1, 2, 3]
YOURS:
[1, 2, 3]
```

Assign a new value to xs by doing xs = /* some value here */. That will cause an error because xs is const.

5.
```js
function f() {
  const xs = [1, 2];
xs = 1;
  xs = 1;
  return xs;
}
f();
GOAL:
TypeError: Assignment to constant variable.
YOURS:
TypeError: Assignment to constant variable.
```

The short version is: const applies to the variable, not the value held by the variable.

6.
```js
const x = 'first'
const x = 'second'
RESULT:
SyntaxError: on line 2: Identifier 'x' has already been declared.
```

However, we can shadow const variables, like we can with let.

7.
```js
function f() {
  const x = 'outer';
  {
    const x = 'inner';
  }
  return x;
}
f();
RESULT:
'outer'
```

We didn't define a duplicate variable there; instead, we defined the same variable name in two different scopes. That's allowed, just like we're allowed to define the same variable name in two different functions.

Variable scoping isn't exactly the bleeding edge of programming language research. But we declare, use, and think about variables more than almost anything else! It's important for it to be right.

Prior to 2015, JavaScript's variable situation was a minefield that made errors easy. Today, the minefield is still there: var is still part of the language and it won't be going away. But we can forget about that old world by fully switching to let and const.

(The computer can help you remember to use let. You can configure your linter to disallow var completely. If you like, it can also force you to use const for variables that are never reassigned. We recommend both.)

let and const nicely encapsulate the goals of ECMAScript 5, ECMAScript 2015, and new versions. (ECMAScript is another name for JavaScript; it's primarily used when talking about the official language specification documents. We'll rarely mention specific ECMAScript versions in this course. Those standards are all finalized now, so we can think of them all as just being JavaScript.)

let and const aren't flashy, and they won't make a web app twice as fast. But they fixed a twenty-year-old design mistake in JavaScript, and have made the language more suitable for large-scale application development. That's what ECMAScript 2015 and beyond have been about: modernizing the language, fixing the mistakes.

## Lesson 4 Modern JavaScript: For of loops

JavaScript has a for...in loop that iterates over an object's keys (not its values):

1.
```js
const obj = {a: 1, b: 2};
const keys = [];
for (const key in obj) {
  keys.push(key);
}
keys;
RESULT:
['a', 'b']
```

This seems OK, but it causes some strange edge cases. To see one edge case, we'll take a detour into JavaScript arrays. Unlike most other languages, JavaScript's arrays are a special kind of JavaScript object. typeof someArray is even 'object', not 'array'!

2.
```js
typeof [1, 2, 3];
RESULT:
'object'
```

That means that we can treat arrays as objects. For example, we can ask for their keys. The "keys" of ['a', 'b'] are ['0', '1']: the array's indexes converted into strings.

3.
```js
Object.keys(['a', 'b', 'c']);
RESULT:
['0', '1', '2']
```

JavaScript arrays can be "sparse": they can have elements missing. Suppose that we create an empty array (const numbers = []) and insert a value at index 3 (numbers[3] = 'a'). In most languages (Java, Python), that would be an error. In some languages (Ruby), it would implicitly fill indexes 0 through 2 with null, nil, or whatever other "not-a-value" value the language has.

In JavaScript, it does neither of those! That array has a value at index 3, but it has nothing at indexes 0, 1, or 2. This is an important point: it's not that the array has undefined or null at indexes 0-2. It has nothing at all there.

JavaScript's for...in loop skips missing array elements. If we loop over the object with for...in, the keys 0-2 don't even show up. Only 3 shows up (as the string '3')!

4.
```js
const letters = [];
letters[3] = 'a';
const keys = [];
for (const key in letters) {
  keys.push(key);
}
keys;
RESULT:
['3']
```

This can cause very confusing bugs if you're not expecting it. (There are similar problems with regular, non-array objects, but this array problem is much quicker to demonstrate.)

Fortunately, modern JavaScript provides a better type of loop: for...of. It loops over the values in an array, not the keys. This mimics the "for-each" loops available in Java, Python, Ruby, and most other languages.

5.
```js
const letters = ['a', 'b', 'c'];
const result = [];
for (const letter of letters) {
  result.push(letter);
}
result;
RESULT:
['a', 'b', 'c']
```

What about a sparse array (an array with some missing indexes)? Fortunately, for...of behaves as we'd expect. It iterates from index 0 until the highest index in the array. If some indexes are missing, it will still iterate over them, giving us the value undefined. In the example below, the loop body executes four times, giving us undefined the first three times.

6.
```js
const numbers = [];
numbers[3] = 'a';
const result = [];
for (const n of numbers) {
  result.push(n);
}
result;
RESULT:
[undefined, undefined, undefined, 'a']
```

What if we want to iterate over an object's keys, like for...in does? Fortunately, since 2011 JavaScript has had an Object.keys method that gives us an object's keys as an array. Then we can for...of over that array.

7.
```js
const obj = {a: 1, b: 2};
const keys = [];
for (const key of Object.keys(obj)) {
  keys.push(key);
}
keys;
RESULT:
['a', 'b']
```

And what if we want to iterate over a string? It does what you'd expect! The loop body executes once per character in the string. JavaScript doesn't have a dedicated character type, so the individual characters will show up as strings of length 1, like 'a'.

8.
```js
const s = 'loop';
const chars = [];
for (const char of s) {
  chars.push(char);
}
chars;
RESULT:
['l', 'o', 'o', 'p']
```

for...of loops also work well with other modern JavaScript features like iterators and generators. We'll see concrete examples when we get to those topics.

As is often the case, linters can stop you from accidentally using the old for...in syntax. ESLint's guard-for-in rule will stop you from accidentally using for...in when you mean for...of. (It will still allow for...in if you use certain patterns to make it safe.)

## Lesson 5 Modern JavaScript: Template literals

JavaScript has always had two syntaxes for strings: 'single quoted' and "double quoted". There's now a third type of string written with `backticks`. They're treated as ordinary strings: `hello` is the same as 'hello'.

1.
```js
`hello`;
RESULT:
'hello'
```

2.
```js
`hello` === 'hello';
RESULT:
true
```

These strings are called "template literals". They have several features that allow them to be used as a "templates", with holes that are filled in later.

Interpolation is the most common use case. "Interpolation" means "inserting something into something else". With template literals, we can insert the result of any JavaScript expression into the string by wrapping it in ${...}

```js
`1 + 1 = ${1 + 1}`;
RESULT:
'1 + 1 = 2'
```

3.
```js
`${'Shouting'.toUpperCase()} and ${'Whispering'.toLowerCase()}`;
RESULT:
'SHOUTING and whispering'
```

Interpolating with ${...} converts the value to a string by calling its .toString() method. For numbers, that works great. But for arrays, it probably won't do what we want. For objects, it definitely won't do what we want!

```js
[1, 2].toString();
RESULT:
'1,2'

({a: 1}).toString();
RESULT:
'[object Object]'
```

4.
```js
`two numbers: ${[1, 2]}`;
RESULT:
'two numbers: 1,2'
```

5.
```js
const obj = {a: 1};
`an object: ${obj}`;
RESULT:
'an object: [object Object]'
```

We can interpolate as many JavaScript expressions as we like.


6.
```js
const x = 4;
`1 + ${x} = ${x + 1}`;
RESULT:
'1 + 4 = 5'
```

Write a function that writes out a sum the long way. Use template literals to build a string including the two numbers and the final sum.

7.
```js
function longSum(x, y) {
  return `${x} + ${y} = ${x + y}`;
  }
[longSum(1, 2), longSum(10, 20)];
GOAL:
['1 + 2 = 3', '10 + 20 = 30']
YOURS:
['1 + 2 = 3', '10 + 20 = 30']
```

All of the usual string syntax also works inside template literals. For example, inside of 'quotes', \' is a single quote. We can also write \' inside template literals.

8.
```js
`\'`;
RESULT:
"'"
```

9.
```js
'\'';
RESULT:
"'"
```

Execute Program renders the string result identically no matter which syntax we use to define it. That's because template literals evaluate to regular strings when used in this way.

Normally, JavaScript strings can't have newlines in them:

```js
const x = 'oh
no'
RESULT:
SyntaxError: on line 1: Unterminated string constant.
```

However, template literals don't have that limitation: they can contain newlines! This simplifies a lot of code. For example, here's an email template written using old-style JavaScript:

10.
```js
const name = 'Amir';
const email = [
  'Hi, ' + name,
  '',
  "We've updated our privacy policy!",
].join('\n');
email === "Hi, Amir\n\nWe've updated our privacy policy!";
RESULT:
true
```

Here's a version with template literals. It's much cleaner: we don't have to constantly open and close quotes, and we don't have to merge the lines with join.

11.
```js
const name = 'Amir';
const email = `
  Hi, ${name},
  
  We've updated our privacy policy!
`;
email === "\n  Hi, Amir,\n  \n  We've updated our privacy policy!\n";
RESULT:
true
```

There is one important difference between the two examples above. In the template literal version, the string includes all of the whitespace between the opening and closing backtick. That includes the newlines, which we wanted. But it also includes the indentation on the front of the individual lines, which we may not want.

There are ways to remove that leading whitespace, like the dedent NPM module. But sometimes the whitespace doesn't matter. For example: in most cases, whitespace between HTML tags won't cause any problems, so we can leave it in.

## Lesson 6

## Lesson 7

## Lesson 8 Modern JavaScript: Computed Properties 

When creating an object, we can write the keys as unquoted words, like Amir in this example:

1.
```js
const loginCounts = {Amir: 5};
loginCounts.Amir;
RESULT:
5
```

We can also create objects with computed keys. To do that, we wrap the key in square brackets, like {[someVariable]: 1}:

2.
```js
const name = 'Amir';
const loginCounts = {[name]: 5};
loginCounts.Amir;
RESULT:
5
```

The [square brackets] can contain any expression, not just simple variables. For example, we can use string concatenation to build the key dynamically:

3.
```js
const loginCounts = {
  ['Be' + 'tty']: 7,
};
loginCounts.Betty;
RESULT:
7
```

Write a function that takes a user and returns a login count object for them, mapping their name to their loginCount. Use a computed property to construct the object. Our pre-written code will call your function for multiple users.

4.
```js
const users = [
  {name: 'Amir', loginCount: 5},
  {name: 'Betty', loginCount: 16},
];
function loginCount(user) {
  return {[user.name]: user.loginCount};
}
[
  loginCount(users[0]),
  loginCount(users[1]),
];
GOAL:
[{Amir: 5}, {Betty: 16}]
YOURS:
[{Amir: 5}, {Betty: 16}]
```

## Lesson 9 Modern JavaScript: isNaN 

JavaScript numbers have always been a sore point, especially issues around NaN.

"NaN" means "not a number". When the IEEE 754 authors wrote their floating point specification in 1985, they intended NaN to be used in rare cases. For example, 0/0 has no value, so they said that it should return NaN. But JavaScript returns NaN in many situations, including ones that commonly happen due to programming mistakes.

For instance, any arithmetic on undefined returns NaN.

```js
undefined - 1;
RESULT:
NaN
```
Here's a mistake where we typo length (a real method on arrays) as elngth.

```js
['a', 'b', 'c'].elngth;
RESULT:
undefined
```

1.
```js
['a', 'b', 'c'].elngth - 1;
RESULT:
NaN
```

Any operation on a NaN returns another NaN. Unfortunately, that means that NaNs will propagate through the system. By the time you actually see the NaN, it might have ended up in a very different place from where it started.

2.
```js
const lastIndex = ['a', 'b', 'c'].elngth - 1;
const middleIndex = Math.floor(lastIndex / 2);
middleIndex;
RESULT:
NaN
```

Even detecting the presence of a NaN is surprisingly difficult. NaN is the only value in JavaScript that isn't equal to itself. Even when compared using ===, NaN is never equal to NaN!

```js
NaN == NaN;
RESULT:
false
```

3.
```js
NaN === NaN;
RESULT:
false
```

NaN's behavior isn't JavaScript's fault: this is how IEEE 754 floating point was designed, and there are reasons that these decisions were made. However, JavaScript does make the mistake of returning NaN in many situations where it didn't have to. (We can see that this is unique to JavaScript because NaN doesn't show up frequently in other dynamic languages like Ruby and Python.)

Although NaN isn't equal to itself, we can check for it with the isNaN function. It does what it sounds like:

```js
isNaN(0);
RESULT:
false
```

4.
```js
isNaN(Infinity);
RESULT:
false
```

5.
```js
isNaN(NaN);
RESULT:
true
```

Unfortunately, isNaN also converts its argument to a number before checking. When we ask it whether isNaN(x) is true, we can imagine that we're really doing isNaN(0+x).

```js
0 + undefined;
RESULT:
NaN
```

6.
```js
isNaN(0 + undefined);
RESULT:
true
```

7.
```js
isNaN(undefined);
RESULT:
true
```

undefined most definitely is not NaN, so this is a mistake in JavaScript's design. But the isNaN function can't be removed or changed without breaking billions of lines of existing code.

Instead, newer versions of JavaScript have introduced a second isNaN function, Number.isNaN. Fortunately, it does what we expect in every case.

8.
```js
Number.isNaN(NaN);
RESULT:
true
```

9.
```js
Number.isNaN(0);
RESULT:
false
```

10.
```js
Number.isNaN(undefined);
RESULT:
false
```

It's difficult to remember that there are two isNaNs and that one of them is preferable. Often, we can use linters to ensure that we don't mix these things up. But, unfortunately, the eslint linter doesn't have direct support for disallowing the global isNaN. However, you can configure the no-restricted-globals rule to disallow isNaN, which will have the same effect.