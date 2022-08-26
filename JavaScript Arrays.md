# JavaScript Arrays

## Lesson 1 JavaScript Arrays: Basics

This course assumes that you know some basic JavaScript concepts. (You can also look them up as you go; there aren't many!) Here's a list of concepts that we use but don't explain:

- null and undefined.
- Variable definitions with let and const.
- Conditionals (if) and ternary conditionals (a ? y : b).
- C-style for loops: for (let i=0; i<10; i++) { ... }.
- Regular functions: function f() { ... }.
- Arrow functions: const f = () => { ... }.

This course covers all of the methods on JavaScript arrays. Arrays are sequences of values that have a specific order and a length.

1.
```js
['a', 'b', 'c'].length;
RESULT:
3

[].length;
RESULT:
0
```

Values can be retrieved from an array using []. Indexes start at 0.

2.
```js
['a', 'b', 'c'][0];
RESULT:
'a'

['a', 'b', 'c'][2];
RESULT:
'c'
```

If we try to access an index that's not in the array, we get undefined.

```js
['a', 'b', 'c'][14];
RESULT:
undefined
```

We can replace array elements with [] and =.

3.
```js
const strings = ['a', 'b', 'c'];
strings[2] = 'x';
strings;
RESULT:
['a', 'b', 'x']
```

## Lesson 2

## Lesson 3

## Lesson 4

## Lesson 5

