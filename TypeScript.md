TypeScript Basics: Basic types

This course covers the TypeScript programming language. TypeScript is a superset of JavaScript, which means that it has all of JavaScript's syntax, plus some new syntax as well.

You don't need any previous experience with statically typed languages to take this course. However, we do assume that you know some basic JavaScript concepts. (You can also look them up as you go; there aren't many!) Here's a list of concepts that we use but don't explain:

- null and undefined.
- Variable definitions with var, let, and const.
- Conditionals (if) and ternary conditionals (a ? y : b).
- Regular functions: function f() { ... }.
- Arrow functions: const f = () => { ... }.
- Loops, using for or map.
- Some common array methods like push and pop.

Unlike JavaScript, variables in TypeScript have static types. Types come after a :.

1.
```js
let sum: number = 1 + 1;
sum;
RESULT:
2

let anything: string = 'any' + 'thing';
anything;
RESULT:
'anything'
```

The examples above compiled successfully because the types were correct. We said that sum was a number, then we assigned a number to it. We said that anything was a string, then we assigned a string to it. When the compiler accepts a program's types, we say that that program type checks.

The compiler forces us to use types correctly. If we use them incorrectly, it's a type error and the program won't compile.

In this course, we'll often encounter type errors. When that happens, you'll write "type error" (without the double quotes). The code below tries to assign a string to a number. That doesn't make sense, so it type errors. Write "type error" in the box.

2.
```js
let sum: number = 'any' + 'thing';
sum;
RESULT:
type error: Type 'string' is not assignable to type 'number'.
```

When you write "type error", we'll show you the full type error message. You don't need to type the whole error message, though. Just "type error" is enough.

Types always have to match. Every variable has a type. Every value we assign must match the variable's type.


3.
```js
let b: boolean = true || false;
b;
RESULT:
true

let b: boolean = 1 + 1;
b;
RESULT:
type error: Type 'number' is not assignable to type 'boolean'.
```

We can't assign a variable of one type to a variable of another type. That causes a type error.

4.
```js
let n: number = 1 + 1;
n;
RESULT:
2

let n: number = 1 + 1;
let s: string = n;
s;
RESULT:
type error: Type 'number' is not assignable to type 'string'.
```