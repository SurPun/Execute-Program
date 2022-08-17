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

## Lesson 6 Modern JavaScript: Rest parameters

JavaScript allows functions to take a variable number of parameters. In recent versions of JavaScript, we can do that by adding ... to the parameter list. The arguments will show up in a single array, regardless of how many arguments there were.

```js
function f(...args) {
  return args;
}
f('a', 'b');
RESULT:
['a', 'b']
```

1.
```js
function f(...args) {
  return args;
}
f(1, 2, 3);
RESULT:
[1, 2, 3]
```

In JavaScript, this feature is called "rest parameters" because it collects "the rest" of the function's parameters into an array. In other languages, you may see it called "varargs", which stands for "VARiable number of ARGuments".

2.
```js
function max(...numbers) {
  let max = numbers[0];
  for (const n of numbers) {
     if (n > max) {
       max = n;
     }
  }
  return max;
}
max(1, 2, 3);
RESULT:
3
```

Write a function sum that sums numbers. It should take the numbers as rest parameters. If no arguments are given, it should return 0.

3.
```js
function sum(...num) {
  let sum = 0;
  for (const n of num) {
    sum += n;
  }
  return sum;
}
const sums = [sum(), sum(100), sum(2000, 1), sum(-500, -300)];
sums;
GOAL:
[0, 100, 2001, -800]
YOURS:
[0, 100, 2001, -800]

```

Rest parameters can be used after regular positional parameters.

4.
```js

function addMany(toAdd, ...numbers) {
  const result = [];
  for (const n of numbers) {
    result.push(n + toAdd);
  }
  return result;
}
addMany(2, 1, 7.7, 1000);
RESULT:
[3, 9.7, 1002]
```

However, positional parameters can't be used after rest parameters. Trying to do that is an error! (You can type error when a code example will throw an error.)

5.
```js
function addMany(...numbers, toAdd) {
  const result = [];
  for (const n of numbers) {
    result.push(n + toAdd);
  }
  return result;
}
addMany(1, 7.7, 1000, 2);
RESULT:
SyntaxError: on line 1: Rest element must be last element.
```

We also can't have multiple rest parameters. Trying to do that also produces an error. (The JavaScript virtual machine would have no way to know which argument values go in which rest parameter.)

6.
```js
function multiplyArrays(...numbers1, ...numbers2) {
  const result = []
  for (let i = 0; i < numbers1.length; i++) {
    result.push(numbers1[i] * numbers2[i])
  }
  return result
}
multiplyArrays(1, 2, 3, 4)
RESULT:
SyntaxError: on line 1: Rest element must be last element.
```

So far we've seen rest parameters in function definitions. They also work when calling a function. When we call f(...args), it means "pass the elements of the args array as separate parameters."

7.
```js
function add(x, y) {
  return x + y;
}
const numbers = [1, 2];
add(...numbers);
RESULT:
3
```

## Lesson 7 Modern JavaScript: Basic array destructuring

Historically, working with arrays and objects was clunky in JavaScript. For example, suppose that we want to extract the first three elements of an array into separate variables:

1.
```js
const letters = ['a', 'b', 'c', 'd'];
const a = letters[0], b = letters[1], c = letters[2];
[a, b, c];
RESULT:
['a', 'b', 'c']
```

It gets the job done, but it's not nice to read. Fortunately, there's a nicer syntax now. The next example does the same thing as the previous one, but notice how much shorter the [a, b, c] assignment is.

2.
```js
const letters = ['a', 'b', 'c', 'd'];
const [a, b, c] = letters;
[a, b, c];
RESULT:
['a', 'b', 'c']
```

This feature is called "destructuring". An array has some structure: certain values at certain indexes. We use destructuring syntax to unpack, or remove, that structure to access the individual pieces. We "de-structure" the array.

Use array destructuring to extract the first two user names in the list. Put them in the firstUser and secondUser variables.

3.
```js
const names = ['Amir', 'Betty', 'Cindy', 'Dalili'];
const [firstUser, secondUser] = names;
const users = [firstUser, secondUser];
users;
GOAL:
['Amir', 'Betty']
YOURS:
['Amir', 'Betty']
```

Assignments with let, const, and the legacy var syntax can all use destructuring.

```js
const letters = ['a', 'b', 'c', 'd'];
let [a, b, c] = letters;
[a, b, c];
RESULT:
['a', 'b', 'c']

const letters = ['a', 'b', 'c', 'd'];
var [a, b, c] = letters;
[a, b, c];
RESULT:
['a', 'b', 'c']
```

We can skip array indexes by leaving them out of the destructuring syntax entirely.

4.
```js
const letters = ['a', 'b', 'c', 'd'];
const [a, , c] = letters;
[a, c];
RESULT:
['a', 'c']
```

When we skip indexes like this, it's called "sparse array destructuring". (The dictionary definition of "sparse" is "thinly dispersed or scattered". We use "sparse" in many programming situations where a structure has empty spots.)

If we try to destructure a value that doesn't have structure, like null or undefined, we'll get an error. (You can type error when a code example will throw an error.)

5.
```js
const [a, b, c] = null;
RESULT:
TypeError: null is not iterable
```

6.
```js
const [a, b, c] = 5;
RESULT:
TypeError: 5 is not iterable
```

If the object has too few indexes, we'll get undefined for those indexes, but no error occurs.

7.
```js
const letters = ['a', 'b', 'c'];
const [a, b, c, d] = letters;
[c, d];
RESULT:
['c', undefined]
```

We can provide defaults when destructuring. If the array has no value at that key, we'll get the default instead. This is useful to avoid getting undefined, like we did in the example above. (Note the new = syntax in the destructuring below.)

8.
```js
const letters = ['a', 'b', 'c'];
const [a, b, c, d='dee'] = letters;
[c, d];
RESULT:
['c', 'dee']
```

9.
```js
const letters = ['a', 'b', 'c', 'd'];
const [a, b, c, d='dee'] = letters;
[c, d];
RESULT:
['c', 'd']
```

Elements with and without defaults can be mixed. In the example below, d has a default but e doesn't. The matched array has no value for e, so it will get the value undefined.

10.
```js
const letters = ['a', 'b', 'c'];
const [a, b, c, d='dee', e] = letters;
[d, e];
RESULT:
['dee', undefined]
```

We can collect any remaining elements using ..., similar to how we used "rest parameters" in function definitions. The ... in the next example collects all of the array elements that weren't matched in the destructuring.

11.
```js
const letters = ['a', 'b', 'c'];
const [a, ...others] = letters;
others;
RESULT:
['b', 'c']
```

Rest parameters are always returned to us in an array, even if there's only one of them.

12.
```js
const letters = ['a', 'b', 'c'];
const [a, b, ...others] = letters;
others;
RESULT:
['c']
```

Use a single array destructuring operation to:

Put the first and second user names into firstUser and secondUser variables.
Put the rest of the user names into an otherUsers variable.

13.
```js
const names = ['Amir', 'Betty', 'Cindy', 'Dalili'];
const [firstUser, secondUser, ...otherUsers] = names;
const users = [firstUser, secondUser, otherUsers];
users;
GOAL:
['Amir', 'Betty', ['Cindy', 'Dalili']]
YOURS:
['Amir', 'Betty', ['Cindy', 'Dalili']]
```

What about destructuring with [...others, c]? It seems like that should mean "put the last element in c and all other elements before it in others." However, that's not supported and will throw an error.

14.
```js
const letters = ['a', 'b', 'c']
const [...others, a] = letters
others
RESULT:
SyntaxError: on line 2: Rest element must be last element.
```

Strings can also be destructured like arrays. When we do that, we get the string's individual characters. (JavaScript has no dedicated character type, so the characters will be represented as strings with length 1, like 'a'.)

15.
```js
const letters = 'abc';
const [a, b, c] = letters;
b;
RESULT:
'b'
```

We can use a rest element to get all of a string's characters, regardless of the string's length.

16.
```js
const s = 'this is a long string';
const [...chars] = s;
chars[2];
RESULT:
'i'
```

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

## Lesson 10 Modern JavaScript: Tagged template literals

Template literals can be "tagged", where we provide a function along with the template string. The tag can modify or replace the template literal string. It looks like replaceWithHello`the numbers ${1} and ${1 + 1}`. (replaceWithHello is the function being used as a template literal tag.)

```js
function replaceWithHello() {
  return 'hello';
}
RESULT:
undefined
```

When we tag a template literal, the return value of the tagging function replaces the entire template string.

```js
function replaceWithHello() {
  return 'hello';
}
RESULT:
undefined
```

When we tag a template literal, the return value of the tagging function replaces the entire template string.

```js
replaceWithHello`goodbye`;
RESULT:
'hello'
```

1.
```js
replaceWithHello`the numbers ${1} and ${1 + 1}`;
RESULT:
'hello'
```

In that case, replaceWithHello`the numbers ${1} and ${1 + 1}` behaved the same as a normal function call. We could have written replaceWithHello('an ordinary string') and gotten the same result.

However, template literals can do more than a normal function call. The JavaScript virtual machine splits the template literal into pieces, passing them to our tag function as arguments.

We'll define a function returnsItsArguments. Then we'll use it as a template literal tag. That will show us what arguments the tag function gets.


```js
function returnsItsArguments(strings, ...values) {
  return {
    firstArg: strings,
    secondArg: values,
  };
}
RESULT:
undefined

returnsItsArguments`1${2}3`;
RESULT:
{firstArg: ['1', '3'], secondArg: [2]}
```

The first argument contains all of the literal strings in the template literal (everything not in a ${...}). The second argument contains all of the interpolated values (everything inside a ${...}). Literal strings are passed as an array argument; interpolated values are passed as rest parameters.

There will always be one more literal string than there are interpolated values. If there are 7 interpolated values, there will be 8 literal strings. If necessary, the last literal string will be an empty string, '', but it will still be there. Here's an example:

```js
returnsItsArguments`1${2}`;
RESULT:
{firstArg: ['1', ''], secondArg: [2]}
```

Watch out for spaces in the literal strings. In the next example, the literal strings are ['the numbers ', ' and ', '']. The interpolated values are [1, 2].

2.
```js
returnsItsArguments`the numbers ${1} and ${2}`;
RESULT:
{firstArg: ['the numbers ', ' and ', ''], secondArg: [1, 2]}
```

The tag function can do anything with those two arrays. For example, it can mimic regular variable interpolation. (That's the kind that we've already seen, where `1 + 1 = ${1 + 1}` evaluates to '1 + 1 = 2'.)

In the next example, note the if (i < values.length). It's working around the fact that there's always one final string literal with no corresponding interpolated value.

3.
```js
function interpolate(strings, ...values) {
  let reconstructed = '';
  for (let i=0; i<strings.length; i++) {
    reconstructed += strings[i];
    if (i < values.length) {
      reconstructed += values[i];
    }
  }
  return reconstructed;
}
interpolate`the numbers ${1} and ${2}`;
RESULT:
'the numbers 1 and 2'
```

This next example is similar to the one above, but we modify the interpolated values right before we insert them into the final string.

Write a doubleNumbers template literal tag. It mimics normal string interpolation, but doubles all of the interpolated values as they're inserted.

4.
```js
function doubleNumbers(strings, ...values) {
  let result = '';
  for (let i=0; i<strings.length; i++) {
    result += strings[i];
    if (i < values.length) {
      result += values[i] + values[i];
    }
  }
    return result;
 }
doubleNumbers`the numbers ${1} and ${2}`;
GOAL:
'the numbers 2 and 4'
YOURS:
'the numbers 2 and 4'
```

Now for a more realistic example: escaping values in HTML. We want to write template strings like <p>${user.name}</p>. If the user's name contains a '<', then it should be escaped to '&lt;'. Likewise for escaping '>' to '&gt;'. For example, if the user's name is 'Amir >_<', the final HTML-safe string should be <p>Amir &gt;_&lt;</p>.

Write a template tag function escapeHTML that escapes angle brackets in all interpolated values. An escapeOneHTMLValue function is provided for you. You'll need to call it on each interpolated value. Remember that strings is always longer than values by exactly 1 element.

5.
```js
function escapeOneHTMLValue(value) {
  return value
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');
}
​
function escapeHTML(strings, ...values) {
  let result = '';
  for (let i=0; i<strings.length; i++) {
    result += strings[i];
    if (values[i] !== undefined) {
      result += escapeOneHTMLValue(values[i]);
    }
  }
  return result;
}
​
const user = {
  name: 'Amir >_<',
};
​
escapeHTML`<p>${user.name}</p>`;
GOAL:
'<p>Amir &gt;_&lt;</p>'
YOURS:
'<p>Amir &gt;_&lt;</p>'
```

The approach above is used in the popular common-tags library's safeHtml tagged template literal. (That library also provides many other pre-made template tags.)

At first, it seems strange that the literal strings are passed in an array, with the interpolated values passed separately. Why not combine them into one array?

Because it would be confusing to mix literal values and interpolated values together, especially when the interpolated values are strings. For example, consider the call someFunction`the string ${'a'}` . The tag function would get an array mixing literal strings and interpolated values: ['the string ', 'a', '']. Passing the strings and interpolated values separately makes it clear which is which.

One final note about tagged template literals. If you don't use semicolons in your JavaScript, you might encounter some strange failures.

```js
const obj = {a: 1}
`an object: ${obj}`;
RESULT:
ReferenceError: Cannot access 'obj' before initialization
```

This error message seems completely wrong: obj is clearly defined before it's used! The problem is that JavaScript isn't parsing this code in the way we think it should. It thinks that the object {a: 1} is being used as a tag function for the template literal. Here's the same thing reformatted to make it more clear:

```js
const obj = (
  ({a: 1})`an object: ${obj}`
);
RESULT:
ReferenceError: Cannot access 'obj' before initialization
```

We can fix this by using semicolons all the time. Or we can insert one semicolon here so that the virtual machine parses the code in the way that we expect.

```js
const obj = {a: 1};
`an object: ${obj}`;
RESULT:
'an object: [object Object]'
```

## Lesson 12 Modern JavaScript: Shorthand properties

Historically, we'd often have to use the pattern below, where the key of an object refers to a variable of the same name:

1.
```js
const name = 'Amir';
const user = {name: name};
user.name;
RESULT:
'Amir'
```

It gets the job done, but it can cause a lot of code bloat when there are many properties.

2.
```js
const name = 'Amir';
const catName = 'Ms. Fluff';
const city = 'Paris';
const age = 36;

const user = {
  name: name,
  catName: catName,
  city: city,
  age: age,
};
[user.name, user.age];
RESULT:
['Amir', 36]
```

In modern JavaScript, we can shorten this significantly. When the object's key refers to a variable of the same name, we only have to specify that name once. Instead of {name: name}, we can just write {name}.

3.
```js
const name = 'Amir';
const catName = 'Ms. Fluff';
const city = 'Paris';
const age = 36;

const user = {name, catName, city, age};
[user.name, user.age];
RESULT:
['Amir', 36]
```

We can't use this to refer to a variable that doesn't exist. As with the older syntax, that will cause an error. (You can type error when a code example will throw an error.)

4.
```js
const user = {age};
RESULT:
ReferenceError: age is not defined
```

## Lesson 13 Modern JavaScript: Shorthand methods

Modern JavaScript allows the definition of methods right inside an object literal.

```js
const user = {
  name() { return 'Amir'; }
};
user.name();
RESULT:
'Amir'
```

Methods defined in this way are called "shorthand methods". (We've already seen "shorthand properties", which let us say {name}. This feature is related to that one in that they're both shorthands, but there's no deeper relation between them than that.)

Shorthand methods have a this that refers to the parent object. We can access its properties by doing this.somePropertyName:

5.
```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() { return `${this.city}, ${this.country}`; },
};
address.addressString();
RESULT:
'Paris, France'
```

Shorthand methods can also call other shorthand methods.

Add a new volume method to this object using JavaScript's shorthand method syntax. (The volume is the base area times the height. We've provided an existing baseArea method for you to call.)

6.
```js
const rectangle3D = {
  width: 3,
  depth: 4,
  height: 5,
  baseArea() { return this.width * this.depth; },
  volume() { return this.baseArea() * this.height }
};
rectangle3D.volume();
GOAL:
60
YOURS:
60
```

As always, JavaScript's this has some sharp edges, so this isn't the full story. We'll see details of that in later lessons.

## Lesson 14 Modern JavaScript: Basic object destructuring

We've seen destructuring in arrays:

1.
```js
const names = ['Amir', 'Betty'];
const [firstName] = names;
firstName;
RESULT:
'Amir'
```

We can also destructure objects. For objects, we pick out specific keys, rather than picking out specific indexes like we did with arrays.

2.
```js
const user = {name: 'Amir', email: 'amir@example.com', age: 36};
const {name, age} = user;
[name, age];
RESULT:
['Amir', 36]
```

As with arrays, trying to destructure null or undefined will throw an error. (You can type error when a code example will throw an error.)

3.
```js
const {name} = null;
RESULT:
TypeError: Cannot destructure property 'name' of 'null' as it is null.

const {name} = undefined;
RESULT:
TypeError: Cannot destructure property 'name' of 'undefined' as it is undefined.
```

If the key doesn't exist in the object, we'll get undefined.

4.
```js
const user = {name: 'Amir', email: 'amir@example.com'};
const {age} = user;
age;
RESULT:
undefined
```

We can supply default values when destructuring. If the key doesn't exist, we'll get the default value instead.

5.
```js
const user = {name: 'Amir', email: 'amir@example.com'};
const {age=36} = user;
age;
RESULT:
36

const user = {name: 'Amir', email: 'amir@example.com', age: 37};
const {age=36} = user;
age;
RESULT:
37
```

We can collect the rest of the keys with ..., which will give us an object with every key that wasn't matched explicitly. In the next example, ...rest will give us an object with the email key and its value.

6.
```js
const user = {name: 'Amir', email: 'amir@example.com'};
const {name, ...rest} = user;
rest;
RESULT:
{email: 'amir@example.com'}
```

Use a single object destructuring operation to:
- Extract the user's name into the name variable.
- Put all of the other keys and values into a rest variable.

7.
```js
const user = {name: 'Amir', email: 'amir@example.com', age: 36};
const {name, ...rest} = user;
const nameAndRest = [name, rest];
nameAndRest;
GOAL:
['Amir', {age: 36, email: 'amir@example.com'}]
YOURS:
['Amir', {age: 36, email: 'amir@example.com'}]
```

When necessary, we can destructure a key into a variable with a different name. We separate the object key and the variable name with a :.

8.
```js
const {name: userName} = {name: 'Amir', email: 'amir@example.com'};
userName;
RESULT:
'Amir'
```

What if we want to destructure an object, but we don't know the name of the key in advance? We can do that using syntax that's similar to the computed properties that we've already seen.

```js
const key = 'name';
const {[key]: value} = {name: 'Amir'};
value;
RESULT:
'Amir'
```

[key] is the key, whose name is computed at runtime by looking at the variable key. That variable contains 'name', so we're looking up the name key. (Don't let the [square brackets] fool you: there are no arrays involved here.)

value is the name of the variable that will hold the value in that key. If key is 'name', that means that value will hold 'Amir'.

As with regular computed properties, we can put any expression inside the [square brackets].

9.
```js
const key = 'NaMe';
const {[key.toLowerCase()]: value} = {name: 'Amir'};
value;
RESULT:
'Amir'
```

Destructuring interacts nicely with getters. If an object's key is a getter, we can destructure that key as usual. We'll get the value returned by the getter.

10.
```js
const user = {get name() { return 'Be' + 'tty'; }};
const {name} = user;
name;
RESULT:
'Betty'
```

## Lesson 15 Modern JavaScript: Bind
 
this refers to the object that a function belongs to. The rules get complex, but here's a simple example with shorthand methods.

In the example below, this inside userName refers to the object that userName belongs to: user.

1.
```js
const user = {
  name: 'Amir',
  userName() { return this.name; }
};
user.userName();
RESULT:
'Amir'
```

The scoping rules for JavaScript's this are notorious. We'll see examples throughout this course, but we'll also see ways to avoid the problems entirely.

Sometimes, when writing JavaScript code, we'll get backed into a corner where we need to force a function to have a certain this. We can do exactly that with the bind method.

bind is a method on functions themselves, so we say someFunction.bind(someNewThis). That gives us a new version of the function, leaving the original function unchanged. When we call the new function, this will be bound to our specified value. Here's a before-and-after:

```js
const user = {name: 'Amir'};
function userName() {
  return this.name;
}
userName();
RESULT:
TypeError: Cannot read property 'name' of undefined
```

2.
```js
const user = {name: 'Amir'};
function userName() {
  return this.name;
}
const userNameBound = userName.bind(user);
userNameBound();
RESULT:
'Amir'
```

We don't have to introduce a temporary variable to hold the bound function; we can do it as a single expression.

3.
```js
const user = {name: 'Amir'};
function userName() {
  return this.name;
}
userName.bind(user)();
RESULT:
'Amir'
```

Use bind to call the getAddressString function with address bound to this.

4.
```js
function getAddressString() {
  return `${this.city}, ${this.country}`;
}
const address = {
  city: 'Paris',
  country: 'France',
};
getAddressString.bind(address)();
GOAL:
'Paris, France'
YOURS:
'Paris, France'
```

Let's see an example of where we might use bind. We've seen shorthand methods defined using the syntax functionName() { ... }. They have a this that refers to the parent object:

5.
```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() { return `${this.city}, ${this.country}`; },
};
address.addressString();
RESULT:
'Paris, France'
```

What happens if addressString returns a function instead of returning the string directly? It won't work, because the inner function has a different this. In the inner function, this will be undefined.

```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() {
    return function() {
      return `${this.city}, ${this.country}`;
    };
  },
};
address.addressString()();
RESULT:
TypeError: Cannot read property 'city' of undefined
```

Depending on where you run that example – in a browser, at a Node REPL, etc. – you may see a different error message. But you can be sure that this won't refer to the containing address object. It might be window or undefined or something else.

We can use JavaScript's bind method to manually set our inner function's this:

6.
```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() {
    const f = function() {
      return `${this.city}, ${this.country}`;
    };
    return f.bind(this);
  },
};
address.addressString()();
RESULT:
'Paris, France'
```

This particular bind example is a bit weird, because we'll soon see a better way to do this using a feature called arrow functions. But that also shows us a general lesson: bind should be used sparingly. Usually, there's a better way to solve scoping problems. When you're tempted to use bind, we recommend searching for another solution, then using bind only as a last resort.

## Lesson 16 Modern JavaScript: Generators

JavaScript's old for-in loop worked with arrays, objects, and strings. The for-of loop is newer, but at first glance it seems to be more limited:

```js
const user = {
  name: 'Amir',
  age: 36,
};
for (const key of user) {
}
RESULT:
TypeError: user is not iterable
```

However, new features in modern JavaScript allow us to write new functions and objects that work with for loops. The easiest way is with generators. We'll look at a working example first, then examine it in detail.

Here's a generator that returns three numbers:

```js
function* numbers() {
  yield 1;
  yield 2;
  yield 3;
}
RESULT:
undefined
```

And here's a for-of loop over it, where the loop sees each of those three numbers:

```js
const results = [];
for (const n of numbers()) {
  results.push(n);
}
results;
RESULT:
[1, 2, 3]
```

Now the details:

The * in function* marks our function as a generator.
When we call a generator function, we get an object that a for-of loop can loop over.
Each yield in the generator provides one value to the loop.
The yield part is a bit weird. One way to think about it is: it's like return, but we can do it multiple times. Each time we yield, the generator function stops running temporarily, and the code inside the for-of loop runs once, consuming the yielded value. Then the generator picks up where it left off to yield another value; then the loop body runs again to consume that value. And so on until the generator function ends.

Writing for-of loops to demonstrate our generators is tedious, so we'll switch to Array.from() to convert the generators into arrays. That makes our examples shorter.


```js
function* letters() {
  yield 'a';
  yield 'b';
}
Array.from(letters());
RESULT:
['a', 'b']
```

Generator functions are still functions, so they can contain any code that is allowed in a function.


1.
```js
function* numbers() {
  let x = 1;
  yield x;
  x += 1;
  yield x;
}
Array.from(numbers());
RESULT:
[1, 2]
```

2.
```js
function* numbersBelow(maximum) {
  for (let i=0; i<maximum; i++) {
    yield i;
  }
}
Array.from(numbersBelow(3));
RESULT:
[0, 1, 2]
```

3.
```js
Array.from(numbersBelow(4));
RESULT:
[0, 1, 2, 3]
```

Write a numbersInRange(min, max) generator that iterates over all numbers from min up to, but not including, max.

4.
```js
function* numbersInRange(min, max) {
  for(let i = min; i < max; i ++) {
    yield i;
  }
}
const results = [
  Array.from(numbersInRange(0, 2)),
  Array.from(numbersInRange(1, 3)),
  // This checks that you actually wrote a generator. No cheating!
  numbersInRange.constructor.name,
];
results;
GOAL:
[[0, 1], [1, 2], 'GeneratorFunction']
YOURS:
[[0, 1], [1, 2], 'GeneratorFunction']
```

## Lesson 17 Modern JavaScript: Places where destructuring is allowed

We've already seen some examples of destructuring. In this lesson, we'll step back to see them in context: how they interact with each other and with other parts of the language.

Destructuring works in function definitions. The destructuring rules are the same as when we destructure in a const or let statement.

1.
```js
function getFirstArrayElement(array) {
  return array[0];
}
getFirstArrayElement(['cat', 'dog', 'horse']);
RESULT:
'cat'
```

2.
```js
function getFirstArrayElement([first]) {
  return first;
}
getFirstArrayElement(['cat', 'dog', 'horse']);
RESULT:
'cat'
```

3.
```js
function getUserName({name}) {
  return name;
}
getUserName({name: 'Ebony'});
RESULT:
'Ebony'
```

Finish this function that turns cat objects (like {name: 'Ms. Fluff', age: 4}) into cat summary strings (like Ms. Fluff (4)). Use destructuring to extract name and age from the cat object.

4.
```js
function catDescription({name, age}) {
  return `${name} (${age})`;
}
[
  catDescription({name: 'Ms. Fluff', age: 4}),
  catDescription({name: 'Keanu', age: 2}),
];
GOAL:
['Ms. Fluff (4)', 'Keanu (2)']
YOURS:
['Ms. Fluff (4)', 'Keanu (2)']
```

Destructuring can also be used in for-of loops.

5.
```js
const users = [{name: 'Amir'}, {name: 'Betty'}];
const names = [];
for (const {name} of users) {
  names.push(name);
}
names;
RESULT:
['Amir', 'Betty']
```

Destructuring can even be used in catch blocks!

6.
```js
let userName = undefined;
try {
  throw {name: 'Amir'};
} catch ({name}) {
  userName = name;
}
userName;
RESULT:
'Amir'
```

There's a parallel here with "rest" parameters. Here are two ways to write a function named rest returning an array with the first element removed. One of them takes the array argument directly; the other takes multiple positional arguments. Both use destructuring to access the values. (Watch out for the lonely , that skips the first array element.)

7.
```js
function rest([, ...rest]) {
  return rest;
}
rest([1, 2, 3]);
RESULT:
[2, 3]
```

8.
```js
function rest(_, ...rest) {
  return rest;
}
rest(1, 2, 3);
RESULT:
[2, 3]
```

## Lesson 18 Modern JavaScript: Nested destructuring

We can destructure nested data, like nested arrays (arrays inside other arrays).

```js
const dataPoints = [
  [10, 20],
  [30, 40]
];
const [[x1, y1], [x2, y2]] = dataPoints;
x1;
RESULT:
10
```

1.
```js
const dataPoints = [
  [10, 20],
  [30, 40]
];
const [[x1]] = dataPoints;
x1;
RESULT:
10

const dataPoints = [
  [10, 20],
  [30, 40]
];
const [[, y1]] = dataPoints;
y1;
RESULT:
20

const dataPoints = [
  [10, 20],
  [30, 40]
];
const [, [x2]] = dataPoints;
x2;
RESULT:
30
```
Use destructuring to extract the value 40 from the nested array below. Destructure it into a variable named y2.

2.
```js
const dataPoints = [
  [10, 20],
  [30, 40]
];
const [[], [, y2]] = dataPoints;
y2;
GOAL:
40
YOURS:
40
```

We can also destructure nested objects.

```js
const user = {
  name: 'Amir',
  address: {
    city: 'Paris',
  },
};
const {address: {city}} = user;
city;
RESULT:
'Paris'
```

In the last example, the address: part only exists so that we can access parts of the address. It doesn't define an address variable. Let's double-check that by re-running the code above, then trying to access the address variable.

(The next example causes an error. You can type error when code will result in an error.)

3.
```js
const user = {
  name: 'Amir',
  address: {
    city: 'Paris',
  },
};
const {address: {city}} = user;
address;
RESULT:
ReferenceError: address is not defined
```

If we want to get the entire address object and also get the city, we can ask for that explicitly, as in the next example. Pay close attention to the destructuring syntax here!

4.
```js
const user = {
  name: 'Amir',
  address: {
    city: 'Paris',
  },
};
const {address, address: {city}} = user;
[city, address];
RESULT:
['Paris', {city: 'Paris'}]
```

Use destructuring to extract Betty's cat's name, which is stored in a nested object. Store it in the name variable. (That variable name matches the name key in cat, which makes the destructuring easier than it would be if it didn't match.)

5.
```js
const user = {
  name: 'Betty',
  cat: {
    name: 'Keanu',
    age: 2,
  },
};
const {cat: {name}} = user;
name;
GOAL:
'Keanu'
YOURS:
'Keanu'
```

Destructuring works on arrays and objects nested inside each other. (Be careful of the , below. It skips the first array element, so we're only extracting the second one.)

6.
```js
const users = [{name: 'Amir'}, {name: 'Betty'}];
const [, {name}] = users;
name;
RESULT:
'Betty'
```

What about when we want to destructure multiple different source variables? We could write one const (or let) per object. However, there's also a trick to doing it in one line. We can destructure multiple values by wrapping them in an array, then immediately destructuring it.

7.
```js
const user = {name: 'Amir'};
const address = {city: 'Paris'};
const [{name}, {city}] = [user, address];
[name, city];
RESULT:
['Amir', 'Paris']
```

Finally, let's consider why destructuring is useful. The most obvious benefit is that it makes code shorter. But merely shortening code isn't inherently good. Many methods for shortening code also make it less readable. In the case of destructuring, there's another, more subtle benefit: predictability.

Some of JavaScript's array and object methods change the array or object, but others don't. For example, the array methods filter and slice don't change the array that they're called on, but the sort and reverse methods do change the array. To use these methods effectively, we have to memorize which methods work in which ways. (We have a separate JavaScript Arrays course that can help with that!)

Destructuring can never modify the objects that we destructure, so we never have to ask "will this modify the array or not?" When someone else reads our destructuring code later, that's one less thing that they have to think about.

(There is technically one exception: if we're destructuring a getter property with side effects that change some object, then destructuring will trigger those side effects. But getters shouldn't have side effects in the first place, so hopefully you'll never encounter that!)

## Lesson 19 Modern JavaScript: Arrow functions

JavaScript has always supported function definitions with and without names:

1.
```js
function f() {
  return 1;
}
f();
RESULT:
1

const f = function() {
  return 1;
};
f();
RESULT:
1

(function() {
  return 1;
})();
RESULT:
1
```

We can use the function syntax to define callbacks, but it's awkward:

```js
[1, 2, 3].map(function (x) { return x * 2; });
RESULT:
[2, 4, 6]
```

Modern JavaScript supports another syntax for function definition called "arrow functions" (because the operator looks like an arrow). This one takes an argument x, then returns x * 2.

2.
```js
[1, 2, 3].map(x => x * 2);
RESULT:
[2, 4, 6]
```

The arrow function above took exactly one argument, so we could write it in the shortest arrow function form, x => x * 2. When a function takes any other number of arguments (including zero), we have to put parentheses around them.

3.
```js
[1, 2, 3].map(() => 5);
RESULT:
[5, 5, 5]
```

(In this next example, the index argument gets the index of each array element: 0, then 1, then 2.)


4.
```js
[1, 2, 3].map((x, index) => index);
RESULT:
[0, 1, 2]
```

Arrow functions support rest parameters and destructuring, just like normal functions do. Using those features requires parentheses around the argument list, even if there's only one argument.

5.
```js
const user = {name: 'Amir'};
const getName = ({name}) => name;
getName(user);
RESULT:
'Amir'

const rest = (first, ...rest) => rest;
rest(1, 2, 3, 4);
RESULT:
[2, 3, 4]
```

So far, we've only seen arrow functions on one line:

6.
```js
const f = x => x * 2;
f(3);
RESULT:
6
```

We can also write multi-line arrow functions by wrapping the function body in {curly braces}. This longer function does the same thing as the short version above:

7.
```js
const f = x => {
  const result = x * 2;
  return result;
};
f(3);
RESULT:
6
```

One-line arrow functions implicitly return the value of that one line; there's no return statement. With multi-line arrow functions, we have to explicitly return a value.

8.
```js
const f = x => {
  if (x > 10) {
    return 'many';
  } else {
    return 'few';
  }
};
f(3);
RESULT:
'few'
```

If we forget the return in a multi-line arrow function, it will return undefined by default.

9.
```js
const f = x => {
  if (x > 10) {
    'many';
  } else {
    'few';
  }
};
f(3);
RESULT:
undefined
```

There's a problem related to these multi-line arrow functions. When an arrow function needs to return an object, you may be tempted to write it like the example below, but it won't work.

```js
const getUser = () => {
  name: 'Amir',
  age: 36,
}
RESULT:
SyntaxError: on line 3: Missing semicolon.
```

We meant to return an object, but the { in that example is beginning a multi-line function body, not an object. The solution is to wrap the object in (parentheses). That's awkward, but it's the only way to clearly distinguish between multi-line functions and objects.

10.
```js
const getUser = () => ({
  name: 'Amir',
  age: 36,
});
getUser().name;
RESULT:
'Amir'
```

The terseness of arrow functions is nice. But they have a second big benefit: their scoping rules are easier to think about than regular functions.

We've seen situations where we're tempted to use bind to force a certain value into this. Here's a before and after showing that:

```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() {
    return function() {
      return `${this.city}, ${this.country}`;
    };
  },
};
address.addressString()();
RESULT:
TypeError: Cannot read property 'city' of undefined

const address = {
  city: 'Paris',
  country: 'France',
  addressString() {
    const f = function() {
      return `${this.city}, ${this.country}`;
    };
    return f.bind(this);
  },
};
address.addressString()();
RESULT:
'Paris, France'
```

11.
```js
const address = {
  city: 'Paris',
  country: 'France',
  addressString() {
    return () => `${this.city}, ${this.country}`;
  },
};
address.addressString()();
RESULT:
'Paris, France'
```

Add a volumeFunction method to the 3D rectangle below. It should return a function that returns the 3D rectangle's volume. The function that you return should be an arrow function, which will ensure that it can see the rectangle's this.

12.
```js
const rectangle3D = {
  width: 3,
  depth: 4,
  height: 5,
  baseArea() { return this.width * this.depth; },
this.
volumeFunction() {
  return () => this.baseArea() * this.height;
}
};
rectangle3D.volumeFunction()();
GOAL:
60
YOURS:
60
```

The rules for JavaScript's scoping are complicated, especially the rules that concern this. Fortunately, the rule for arrow functions is simple: arrow functions always inherit the scope they were defined in. This makes them ideal for use with callback functions, where we often want to reference variables in the function or object that created the callback function.

## Lesson 20 Modern JavaScript: Classes

JavaScript gained a new class syntax in 2015, making it more familiar to programmers who already know common object-oriented languages like Java, C#, Python, or Ruby.

If you already know an object-oriented language, this first lesson on classes might be very easy. But we'll soon turn to issues that are specific to JavaScript's version of classes.

Classes describe the shape of an object: what properties and methods (functions) it has. Class method definitions look like the shorthand method syntax that we already saw in literal objects: methodName() { ... }.

We construct an instance of a class with new: new MyClass(). The instance is an object with all of the properties and methods (functions) specified by the class.

1.
```js
class MsFluff {
  name() {
    return 'Ms. Fluff';
  }
}
new MsFluff().name();
RESULT:
'Ms. Fluff'
```

Classes can have constructors, which are special methods that initialize the object.

2.
```js
class MsFluff {
  constructor() {
    this.name = 'Ms. Fluff';
  }
}
new MsFluff().name;
RESULT:
'Ms. Fluff'
```

But why define a MsFluff class when we can define a more generic Cat? Then we can use that class to make multiple different cats.

3.
```js
class Cat {
  constructor(name) {
    this.name = name;
  }
}
[new Cat('Ms. Fluff').name, new Cat('Keanu').name];
RESULT:
['Ms. Fluff', 'Keanu']
```

Define a Cat class whose constructor takes both a name and an age. Store the name and age as properties on this.

4.
```js
class Cat {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
[
  new Cat('Ms. Fluff', 4),
  new Cat('Keanu', 2),
];
GOAL:
[{age: 4, name: 'Ms. Fluff'}, {age: 2, name: 'Keanu'}]
YOURS:
[{age: 4, name: 'Ms. Fluff'}, {age: 2, name: 'Keanu'}]
```

Ms. Fluff and Keanu are "instances" of the Cat class. When we create them with new Cat(...), we're "instantiating" the class into a specific instance.

Use new to instantiate an array of two cats: 'Ms. Fluff' and 'Keanu'.

5.
```js
class Cat {
  constructor(name) {
    this.name = name;
  }
}
'Keanu')];
const cats = [new Cat('Ms. Fluff'), new Cat('Keanu')];
cats.map(cat => cat.name);
GOAL:
['Ms. Fluff', 'Keanu']
YOURS:
['Ms. Fluff', 'Keanu']
```

Classes can contain methods, which are functions that show up on the individual instances. Methods can access the object's properties by doing this.somePropertyName.

6.
```js
class Cat {
  constructor(name, vaccinated) {
    this.name = name;
    this.vaccinated = vaccinated;
  }

  needsVaccination() {
    return !this.vaccinated;
  }
}
[
  new Cat('Ms. Fluff', true).needsVaccination(),
  new Cat('Keanu', false).needsVaccination(),
];
RESULT:
[false, true]
```

Define an area method on the Rectangle class.

7.
```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
​
return this.width * this.height;
  area() {
    return this.width * this.height;
  }
}
​
[new Rectangle(3, 4).area(), new Rectangle(5, 6).area()];
GOAL:
[12, 30]
YOURS:
[12, 30]
```

We can't call instance methods directly on the class itself. If we try, we'll get an error. (You can type error when a code example will throw an error.)

8.
```js
class Cat {
  constructor(name, vaccinated) {
    this.name = name;
    this.vaccinated = vaccinated;
  }

  needsVaccination() {
    return !this.vaccinated;
  }
}
Cat.needsVaccination();
RESULT:
TypeError: Cat.needsVaccination is not a function
```

The new keyword is required when instantiating classes: new Cat(...). If we try to create a Cat by calling the class like a function, we'll get an error.

9.
```js
class Cat { }
Cat();
RESULT:
TypeError: Class constructor Cat cannot be invoked without 'new'
```

JavaScript's native object system is prototype-based, which is unusual and therefore unfamiliar to a lot of programmers. We haven't mentioned that object system here precisely because it's so unusual. Prototypes are better viewed as an advanced topic, to be studied once the more "normal" parts of modern JavaScript are familiar. However, the class-based syntax in this lesson is only "syntactic sugar": it's a new syntax layered over the older prototype-based object system.

When possible, we recommend using classes rather than prototypes because they're more widely understood. Occasionally, you'll be forced to dig down into the prototype system, especially when dealing with older JavaScript code or very unusual kinds of objects.

## Lesson 21 Modern JavaScript: Function name property

Normally, defining a function automatically assigns it to a variable. If we declare a function f(), we can use the variable f to refer to our function.

```js
function f() { return 5; }
f();
RESULT:
5
```

Functions declared like this have a name property. As you might expect, the name of a function f() is the string 'f'.

1.
```js
function f() { return 5; }
f.name;
RESULT:
'f'
```

Functions don't have to have names, though. If a function is created without a name, its name will be the empty string, ''. These are called "anonymous" functions.

2.
```js
(
  function() { return 5; }
).name;
RESULT:
''
```

If we immediately assign an anonymous function to a variable, that variable's name will become the function's name. (Functions created like this are still called "anonymous" because there's no name specified in the function() { ... } definition.)

3.
```js
const f = function() { return 5; };
f.name;
RESULT:
'f'
```

The function remembers its original name, even if it's assigned to a different variable.

4.
```js
const f = function() { return 5; };
const f2 = f;
f2.name;
RESULT:
'f'

const g = function() { return 5; };
const functions = [g];
functions[0].name;
RESULT:
'g'
```

Anonymous functions only get a name if they're assigned directly to a variable. If we put the function in an array, for example, it won't get the array's name; instead, it will have no name.

5.
```js
const functions = [function() { return 5; }];
functions[0].name;
RESULT:
''
```

Arrow function syntax doesn't give us any place to specify the function's name. As a result, arrow functions are always anonymous. However, like normal functions, they do get a name if they're assigned directly to a variable.

6.
```js
(
  () => 1
).name;
RESULT:
''

const f = () => 1;
f.name;
RESULT:
'f'
```

## Lesson 22 Modern JavaScript: JSON stringify and parse

In the past, we had to use third-party libraries to read and write JSON. Fortunately, modern versions of JavaScript include built-in JSON support.

```js
JSON.stringify({a: 2}) === '{"a":2}';
RESULT:
true

JSON.parse('{"a": 1, "b"   :   2}');
RESULT:
{a: 1, b: 2}
```

The stringify method turns an object into a JSON string. By default, it will pack the JSON tightly. For example, there's no space after the : in the stringified object above. The parse method turns a JSON string back into an object.

By combining the two methods, we can "round-trip" an object to JSON and back, ending up with the same object that we started with.

1.
```js
JSON.parse(JSON.stringify({name: 'Amir'}));
RESULT:
{name: 'Amir'}
```

If we try to parse a string that isn't valid JSON, we'll get an error. (You can type error when a code example will throw an error.)

2.
```js
JSON.parse('*@& oh no &@*')
RESULT:
SyntaxError: Unexpected token * in JSON at position 0
```

JSON stands for "JavaScript object notation". However, not all JavaScript object syntax is valid JSON.

The full details of JSON's syntax are out of scope for this course, but we'll examine one quick example to make the point. JavaScript object keys don't have to be quoted, but JSON keys do require quotes. If we try to parse a JSON string with unquoted object keys, we'll get an error.

3.
```js
JSON.parse('{"a": 1}');
RESULT:
{a: 1}

JSON.parse('{a: 1}')
RESULT:
SyntaxError: Unexpected token a in JSON at position 1
```

The JSON methods will parse and stringify any JSON-compatible value, not just objects. They work on arrays, strings, numbers, etc.

4.
```js
JSON.parse(JSON.stringify([1, 2]));
RESULT:
[1, 2]

JSON.parse(JSON.stringify('hello'));
RESULT:
'hello'

JSON.parse(JSON.stringify([true, null]));
RESULT:
[true, null]
```

undefined is an important special case because it's not allowed in JSON. When we call stringify, any undefineds in the original object will be turned into nulls. If we then parse the resulting JSON, we'll get a null out. The JSON contains no hints that there ever was an undefined.

```js
JSON.stringify([1, undefined, 2]);
RESULT:
'[1,null,2]'
```

5.
```js
JSON.parse(JSON.stringify([1, undefined, 2]));
RESULT:
[1, null, 2]
```

JSON can't represent circular objects (objects that reference themselves). Here's an example of a circular object.

```js
const circularObject = {};
circularObject.someKey = circularObject;
circularObject;
RESULT:
{someKey: (circular reference)}
```

6.
```js
const circularObject = {};
circularObject.someKey = circularObject;
JSON.stringify(circularObject);
RESULT:
TypeError: Converting circular structure to JSON
    --> starting at object with constructor 'Object'
    --- property 'someKey' closes the circle
```

Sometimes, we'll want an object to specify how it should be serialized to JSON. We can do that by putting a function in its toJSON property. JSON.stringify will call that function and use its result rather than the original object.

```js
const user = {
  name: 'Amir',
  toJSON: () => 'This is Amir!'
};
JSON.parse(JSON.stringify(user));
RESULT:
'This is Amir!'
```

The toJSON function isn't responsible for actually converting to JSON; stringify will still do that part. Instead, toJSON returns a new JavaScript object to be serialized in place of the original.

7.
```js
JSON.parse(
  JSON.stringify(
    {
      name: 'Amir',
      toJSON: () => ({thisWas: 'Amir'})
    }
  )
);
RESULT:
{thisWas: 'Amir'}
```

## Lesson 23 Modern JavaScript: Spread

We've seen ... used to provide variable numbers of arguments to functions:

1.
```js
function add(x, y) {
  return x + y;
}
const numbers = [1, 2];
add(...numbers);
RESULT:
3
```

We can also use ... when constructing an array. There, ... means "include that array's elements into this array".

```js
const numbers = [
  1,
  ...[2, 3],
  4,
];
numbers;
RESULT:
[1, 2, 3, 4]
```

2.
```js
const middleNumbers = [6, 7];
const numbers = [5, ...middleNumbers, 8];
numbers;
RESULT:
[5, 6, 7, 8]

```

When we use ... in this way, it's called the "spread" operator. (We're "spreading" one array's elements across another array.)

We can spread multiple arrays in a single expression. The elements will occur in the order they were specified.

3.
```js
const numbers1 = [1, 2];
const numbers2 = [3, 4];
[...numbers1, ...numbers2];
RESULT:
[1, 2, 3, 4]

const numbers1 = [1, 2];
const numbers2 = [3, 4];
[...numbers2, ...numbers1];
RESULT:
[3, 4, 1, 2]
```

In the past, we'd merge arrays using their concat method.

```js
const nonAdmins = [
  {name: 'Amir'},
];
const admins = [
  {name: 'Betty'},
];
const allUsers = nonAdmins.concat(admins);
allUsers.map(user => user.name);
RESULT:
['Amir', 'Betty']
```

Spreading is generally considered a more readable alternative:

4.
```js
const nonAdmins = [
  {name: 'Amir'},
];
const admins = [
  {name: 'Betty'},
];
const allUsers = [...nonAdmins, ...admins];
allUsers.map(user => user.name);
RESULT:
['Amir', 'Betty']
```

Spreading also works with objects. There, it means "include all of that object's properties into this object".

```js
const amir = {
  name: 'Amir',
  age: 36,
};
const amirWithEmail = {
  ...amir,
  email: 'amir@example.com'
};
amirWithEmail;
RESULT:
{age: 36, email: 'amir@example.com', name: 'Amir'}
```

Order matters when setting object keys. When defining an object literal, the last value assigned to a key wins:

5.
```js
const user = {
  name: 'Amir',
  age: 36,
  name: 'Betty',
};
user.name;
RESULT:
'Betty'
```

Order of object keys also matters with spread. The last instance of a key wins, even if it comes from a spread.

6.
```js
const user1 = {
  name: 'Amir',
  age: 36
};
const user2 = {
  name: 'Betty',
};
({...user1, ...user2});
RESULT:
{age: 36, name: 'Betty'}

const user1 = {
  name: 'Amir',
  age: 36
};
const user2 = {
  name: 'Betty',
};
({...user2, ...user1});
RESULT:
{age: 36, name: 'Amir'}
```

Use spread syntax to write a mergeObjects function that combines two objects.

7.
```js
function mergeObjects(obj1, obj2) {
  return {...obj1, ...obj2};
}
mergeObjects(
  {name: 'Amir', age: 36},
  {name: 'Betty'}
);
GOAL:
{age: 36, name: 'Betty'}
YOURS:
{age: 36, name: 'Betty'}
```

## Lesson 24 Modern JavaScript: Sets

Sometimes, we want to know whether a value is in a list of other values. We can use an array:

1.
```js
const names = ['Amir', 'Betty', 'Cindy'];
names.includes('Betty');
RESULT:
true
```

This works, but there's a potential performance problem hiding here. The includes method loops through the entire array, checking each element. If there are 10,000 elements, it will loop up to 10,000 times.

(The technical way to say that is: includes is O(n). If there are n elements, then includes has to do n comparisons while checking each one. In some cases it will find the element early in the array and stop, but in other cases it will have to check every element.)

JavaScript's Set data type is a better solution to this problem. A JavaScript set is a collection, it's ordered, and it contains only unique values. We'll look at each of those in order.

First, sets are collections: they contain a bunch of JavaScript values. We can give a set an initial set of values in its constructor. Then we can add() more values later. We can ask a set whether it has() a given value.

2.
```js
const names = new Set(['Amir', 'Betty', 'Cindy']);
names.has('Amir');
RESULT:
true

const names = new Set(['Amir', 'Betty', 'Cindy']);
names.has('Dalili');
RESULT:
false

const names = new Set(['Amir', 'Betty', 'Cindy']);
names.add('Dalili');
names.has('Dalili');
RESULT:
true
```

Second, JavaScript sets are ordered. If we examine the elements in the set, they'll always come back in the order that they were inserted.

To get the elements out of the set, we can use the values method. It returns an iterator, which we haven't covered yet. We can ignore the iterator for now by converting it into an array with Array.from().

```js
const names = new Set(['Amir', 'Betty']);
Array.from(names.values());
RESULT:
['Amir', 'Betty']
```

3.
```js
const names = new Set(['Betty', 'Amir']);
Array.from(names.values());
RESULT:
['Betty', 'Amir']
```

Sets are mathematical objects studied in set theory, the modern form of which dates to the 1870s. Most programming languages, including JavaScript, provide a set type with operations that would've been familiar to the set theorist Georg Cantor (1845 - 1918).

Unlike mathematical sets, and unlike other programming languages' sets, JavaScript's sets are ordered, as we saw above. This isn't a case of JavaScript being weird in a way that's confusing. If anything, preserving order is desirable; it's one less uncertainty to track in our code.

It's important to keep this difference in mind if you already know other languages, or if you learn other languages in the future. In JavaScript, you can count on a set to remember the elements' insertion order. In other languages, you can't count on that, and elements can come back in any order.

The third property of sets is: they contain only unique values. If we add a value that already exists in the set, nothing happens; the set is unchanged.

4.
```js
const names = new Set(['Amir']);
names.add('Betty');
names.add('Betty');
Array.from(names.values());
RESULT:
['Amir', 'Betty']
```

Elements can be deleted from a set with the delete method. And the entire set can be cleared with clear.

5.
```js
const names = new Set(['Amir', 'Betty']);
names.delete('Betty');
Array.from(names.values());
RESULT:
['Amir']

const names = new Set(['Amir', 'Betty']);
names.clear();
Array.from(names.values());
RESULT:
[]
```

Sets have a size property that returns the number of items in the set.

6.
```js
const names = new Set(['Amir', 'Betty']);
names.size;
RESULT:
2
```

A set's size reflects the number of unique values that it holds. Duplicates passed to the constructor or added with .add don't contribute to the size.

7.
```js
const names = new Set(['Amir', 'Betty', 'Amir']);
names.size;
RESULT:
2

const names = new Set(['Amir', 'Betty']);
names.add('Betty');
names.size;
RESULT:
2
```

That's everything that a set can do! But the examples so far have been a bit deceptive: we keep converting the set into an array. The main power of sets isn't in converting them back into arrays; it's that calling has is very fast.

An array's includes method slows down as the array gets larger. But sets don't have that problem! A set's has() method is O(1): it always takes the same amount of time regardless of how many elements there are.

(As is often the case, that's true in practice, but it's false in the most literal sense. There are situations where has() could theoretically perform as badly as an array does. But they're so vanishingly rare that few of us will ever encounter them in our entire careers.)

Let's run some benchmarks to see the performance difference in reality:

- We'll use a benchmark function that calls another function a million times, then returns how many milliseconds those calls took. (We call the function a million times to make sure that it takes a measurable amount of time.)
- We'll benchmark an array and a set, each with about a million elements.
- We won't benchmark the cost of building the array and the set; we only care about the cost of calling the includes and has methods.

```js
function benchmark(f) {
  const start = new Date().getTime();
  for (let i=0; i<1000000; i++) {
    f();
  }
  const end = new Date().getTime();
  return end - start;
}

/* This is a JavaScript trick that means "make an array of a million random
 * numbers". These array details are covered in our "JavaScript Arrays"
 * course. */
const largeArray = new Array(1000000).fill(undefined).map(() => Math.random());
const largeSet = new Set(largeArray);

// Benchmark the array's includes method vs. the set's has method.
[
  benchmark(() => largeArray.includes(5)),
  benchmark(() => largeSet.has(5)),
];
RESULT:
[9969, 16]
```

Unlike the other examples in this course, we didn't actually run the one above in your browser. If we did that, then we (as the authors of this lesson) couldn't know exactly what numbers would come out.

But those numbers, 9969 and 16, are the actual benchmarked times, in milliseconds, that we got on our machine! It took 623 times as long to call includes vs. has. If you run the above code in your browser's developer console, you should see a similar ratio. (You'll need to put a console.log(...) around the final array of times.)

Now let's prove that a set's has method is always fast, regardless of the number of elements. The next example reuses the benchmark function definition above, as well as the largeSet data. We compare the performance of has called on a 1-element lonelySet to the performance on the million-element largeSet.

```js
const lonelySet = new Set([1]);
[
  benchmark(() => lonelySet.has(5)),
  benchmark(() => largeSet.has(5)),
];
RESULT:
[11, 12]
```

Calling has took the same amount of time for a single-element set that it took for a million-element set! Again, those are the numbers that we got, but you should see similar results if you try it yourself.

Internally, Execute Program uses sets in a few different places. All of them could be replaced with arrays for a moderate performance penalty, but why take the penalty when sets have such a simple interface?

For example, every code example in Execute Program has an ID, and all example IDs must be unique. To check for that uniqueness, we loop over the example IDs, adding them to a set. If we ever encounter an ID that's already in the set, then we know that we're seeing it for a second time, so it's a duplicate. In that case, we throw an exception to signal the error to ourselves.

Write a hasDuplicates function. It should use a set to decide whether an array of numbers has any duplicates. It should return true if there are duplicates; otherwise it should return false.

There are multiple ways to solve this problem with sets. Here's one possible solution. Begin with an empty set. Then, for each number, check for whether the number is in the set. If it's in the set then we must have seen it earlier in the array, so it's a duplicate, so we can return true immediately. If we get to the end of the array then we never saw any number twice, so we can return false.

8.
```js
function hasDuplicates(numbers) {
  const set = new Set();
  for (const n of numbers) {
    if (set.has(n)) {
      return true;
    }
    set.add(n);
  }
  return false;
}
[
  hasDuplicates([]),
  hasDuplicates([1, 2, 3]),
  hasDuplicates([10, 20, 20]),
  hasDuplicates([100, 200, 300, 100]),
];
GOAL:
[false, false, true, true]
YOURS:
[false, false, true, true]
```

## Lesson 25 Modern JavaScript: Extending classes

JavaScript classes can extend (inherit from) other classes: class MySubclass extends MySuperclass. The subclass will have all of the properties and methods of the superclass, plus any properties and methods that it defines itself.

1.
```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
}

new Cat('Ms. Fluff').name;
RESULT:
'Ms. Fluff'
```

The superclass and subclass can define their own separate constructors. That lets us centralize shared setup code in the superclass, avoiding duplication in its subclasses. The subclass' constructor calls the superclass' constructor with super(), passing whatever arguments the superclass' constructor requires.

2.
```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name + ' the cat');
  }
}

new Cat('Ms. Fluff').name;
RESULT:
'Ms. Fluff the cat'

class Animal {
  constructor(legCount) {
    this.legCount = legCount;
  }

  canWalk() {
    return this.legCount > 0;
  }
}

class Worm extends Animal {
  constructor() {
    super(0);
  }
}

class Dog extends Animal {
  constructor() {
    super(4);
  }
}

[new Worm().canWalk(), new Dog().canWalk()];
RESULT:
[false, true]
```

Calling super is mandatory: if the subclass has a constructor, it has to call super at some point during the constructor. Otherwise, newing the subclass will cause an error. This rule holds even when the superclass doesn't actually define a constructor; we still have to call super!

```js
class Animal { }
class Cat extends Animal {
  constructor(name) {
    this.name = name;
  }
}
new Cat('Ms. Fluff').name;
RESULT:
ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```

We can use instanceof to check for whether an object is an instance of a given class.

3.
```js
class Cat {
}
const cat = new Cat();
cat instanceof Cat;
RESULT:
true

class Cat {
}
'cat' instanceof Cat;
RESULT:
false

class Cat {
}
true instanceof Cat;
RESULT:
false
```

instanceof understands subclasses: x instanceof Y is true when x is an instance of any subclass of Y.

4.
```js
class Animal { }
class Cat extends Animal { }
const cat = new Cat();
cat instanceof Animal;
RESULT:
true
```

We can inherit from classes that inherited from other classes, and so on; there's no limit.

5.
```js
class Animal {
  constructor(legCount) {
    this.legCount = legCount;
  }

  canWalk() {
    return this.legCount > 0;
  }
}

class Dog extends Animal {
  constructor(runSpeedKmPerHour) {
    super(4);
    this.runSpeedKmPerHour = runSpeedKmPerHour;
  }
}

class Greyhound extends Dog {
  constructor() {
    super(65);
  }
}

class Pug extends Dog {
  constructor() {
    super(5);
  }
}

[
  new Greyhound().canWalk(),
  new Greyhound().runSpeedKmPerHour,
  new Pug().canWalk(),
  new Pug().runSpeedKmPerHour,
];
RESULT:
[true, 65, true, 5]

new Pug() instanceof Dog;
RESULT:
true

new Pug() instanceof Animal;
RESULT:
true
```

Subclasses can define properties or methods that already exist on the superclass. When that happens, the subclass' version "overrides", or replaces, the superclass' version.

6.
```js
class Bird {
  speak() {
    return 'chirp';
  }
}
class Crow extends Bird {
  speak() {
    return 'CAW';
  }
}
[new Bird().speak(), new Crow().speak()];
RESULT:
['chirp', 'CAW']
```

Define an Admin class that inherits from User. Its constructor should call super, passing along the relevant arguments. It should also set the admin's isAdmin flag to true.

7.
```js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
    this.isAdmin = false;
  }
}
class Admin extends User {
  constructor(name, email) {
    super(name, email);
    this.isAdmin = true;
  }
}
const admin = new Admin('Amir', 'amir@example.com');
[admin.name, admin.email, admin.isAdmin];
GOAL:
['Amir', 'amir@example.com', true]
YOURS:
['Amir', 'amir@example.com', true]
```

Some languages have "multiple inheritance": they allow a class to inherit from multiple other classes at once. Python and C++ are examples. In JavaScript, there is no syntax for multiple inheritance, so we won't discuss it here.

```js
class Alive { }
class Animal { }
class Dog extends Alive, Animal { }
RESULT:
SyntaxError: on line 3: Unexpected token, expected "{"
```

Finally, a note on when inheritance is appropriate. Object-oriented programming was very popular in the 1990s, sometimes promoted with an almost religious fervor. That fervor has died down now. We recommend viewing classes, and especially inheritance, as just another tool. Like any tool, it's important not to reach for them when they're not needed. Here's an example from React JS, a popular JavaScript UI library.

In older versions of React, components were defined using classes: class MyComponent extends React.Component. This made a lot of sense, because React.Component defines many properties and methods that our components will want to use. It sets this.props so a component can easily see the data sent to it from parent components. It defines this.setState so a component can change its state easily, and so on.

That class syntax has since been superseded by a very clever mechanism called React hooks. With hooks, the classes disappear and we just write functions. Components written with hooks are noticeably shorter, and the consensus is that hooks also lead to components that are easier to read and reason about.

React hooks' success isn't an argument that "classes are bad". But it does show us that, even in cases where classes model a problem well, there might be an even better model. Execute Program itself contains a number of classes: User, Graph (a graph data structure), Parser (a parser for our course definition markup language), etc. But most of the application logic is contained in regular functions, not in classes.

## lesson 26 Modern JavaScript: String keyed methods

We've seen shorthand methods in object literals:

1.
```js
const user = {
  name() { return 'Amir'; }
};
user.name();
RESULT:
'Amir'
```

When we use that syntax, the method name is subject to the normal limitations on JavaScript variable names. It can't contain spaces, for example.

```js
const user = {
  name of the user() { return 'Amir' }
}
user.name()
RESULT:
SyntaxError: on line 2: Unexpected token, expected ","
```

However, if we put quotes around the method's name, then it can contain spaces and even punctuation!

If a method name contains spaces or punctuation, then we can't call it with the usual someObject.methodName syntax. We'll have to use someObject['methodName'].

2.
```js
const user = {
  'name of the ~user~'() { return 'Betty'; }
};
user['name of the ~user~']();
RESULT:
'Betty'
```

This method definition syntax is called "string keyed methods" because we're using the normal JavaScript string syntax to define the method's name (its key in the object).

Modern JavaScript has good Unicode support, so ordinary methods work with non-English characters as well. For example, we can name our method "名前", the Japanese word for "name".

3.
```js
const user = {
  名前() { return 'Betty'; }
};
user.名前();
RESULT:
'Betty'
```

However, we can't use normal syntax to name the method 名前。. The "。" character is a Japanese period, which is punctuation, and punctuation isn't allowed in JavaScript identifiers. Including that character will result in an error. (You can type error when a code example will throw an error.)

```js
const user = {
  名前。() { return 'Betty' }
}
user.名前。()
RESULT:
SyntaxError: on line 2: Unexpected character '。'.
```

With string keyed methods, we can define that "名前。" method.

4.
```js
const user = {
  '名前。'() { return 'Betty'; }
};
user['名前。']();
RESULT:
'Betty'
```

String keyed methods also work in class definitions. This is a recurring theme: syntax that works in object literal methods will generally work inside class definitions as well.

5.
```js
class User {
  constructor(name) {
    this.name = name;
  }

  'name of the user'() {
    return this.name;
  }
}

new User('Marie')['name of the user']();
RESULT:
'Marie'

class User {
  constructor(名前) {
    this.名前 = 名前;
  }

  '名前。'() {
    return this.名前;
  }
}

new User('Marie')['名前。']();
RESULT:
'Marie'
```

Define a User class with a method named "is admin", including the space. Your class should accept an isAdmin argument to the constructor, which is then returned by the "is admin" method.

6.
```js
class User {
  constructor(isAdmin) {
    this.isAdmin = isAdmin;
  }
  
  'is admin'() {
    return this.isAdmin;
  }
}
const adminFlags = [
  new User(false)['is admin'](),
  new User(true)['is admin'](),
];
adminFlags;
GOAL:
[false, true]
YOURS:
[false, true]
```

## Lesson 27 Modern JavaScript: Class scoping

We usually define classes at the top level of a module. However, classes follow the same scoping rules as regular variables. We can define them inside a function, or even inside a conditional.

1.
```js
function createKoko() {
  class Gorilla {
    constructor(name) {
      this.name = name;
    }
  }
  return new Gorilla('Koko');
}
createKoko().name;
RESULT:
'Koko'

function createGorilla(name) {
  if (name === undefined) {
    return undefined;
  } else {
    class Gorilla {
      constructor(name) {
        this.name = name;
      }
    }
    return new Gorilla(name);
  }
}
[createGorilla(undefined), createGorilla('Koko').name];
RESULT:
[undefined, 'Koko']
```

Just to be clear: there are very few cases where a class should be defined in a function. There are even fewer (and maybe none) where a class should be defined inside a conditional. But JavaScript allows us to do these things!

Here's a much better version of the previous code example, with the Gorilla class defined at the top level of the module. Notice how much easier it is to read now that the Gorilla is defined separately.

2.
```js
class Gorilla {
  constructor(name) {
    this.name = name;
  }
}

function createGorilla(name) {
  if (name === undefined) {
    return undefined;
  } else {
    return new Gorilla(name);
  }
}

[createGorilla(undefined), createGorilla('Koko').name];
RESULT:
[undefined, 'Koko']
```

When classes are defined dynamically, like inside a function, the class definition itself can see all of the variables that are currently in scope. For example, a class defined inside a function can reference the function's arguments.

In the next example, notice that the class's constructor doesn't take a name argument. Instead, the class is "capturing" the containing function's name argument!

3.
```js
function createGorilla(name) {
  class Gorilla {
    constructor() {
      this.name = name;
    }
  }
  return new Gorilla();
}
createGorilla('Michael').name;
RESULT:
'Michael'
```

There's one scoping rule that will surprise programmers who know other object-oriented languages: we can't define a class inside another class. That will cause an error, whether we use the class or not.

4.
```js
class Zoo {
  class Gorilla {
    constructor(name) {
      this.name = name
    }
  }
}
RESULT:
SyntaxError: on line 2: Unexpected token
```

Classes are only visible inside the scope where they were defined, just like variables defined with let or const. If we try to reference a class outside of its scope, we'll get an error.

5.
```js
if (true) {
  const x = 1;
}
x;
RESULT:
ReferenceError: x is not defined

function createGorilla() {
  class Gorilla {
    constructor(name) {
      this.name = name;
    }
  }
  return new Gorilla('Koko');
}
new Gorilla('Koko');
RESULT:
ReferenceError: Gorilla is not defined

if (true) {
  class Cat { }
}
new Cat();
RESULT:
ReferenceError: Cat is not defined
```

## Lesson 28

## Lesson 29

## Lesson 30