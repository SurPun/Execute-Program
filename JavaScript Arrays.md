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

## Lesson 2 JavaScript Arrays: Stack

We can add elements to the end of an array with push.

1.
```js
const a = [1, 2];
a.push(3);
a;
RESULT:
[1, 2, 3]
```

push returns the array's length, including the newly-pushed element.

```js
const a = ['a', 'b'];
a.push('c');
RESULT:
3
```

2.
```js
const a = ['a', 'b', 'c', 'd'];
a.push('e');
RESULT:
5
```

pop is the opposite of push. It removes the last element from the array.

3.
```js
const a = [1, 2, 3];
a.pop();
a;
RESULT:
[1, 2]
```

pop returns the element that was removed.

4.
```js
const a = [1, 2, 3];
a.pop();
RESULT:
3
```

If the array is empty, pop returns undefined, because there's nothing to remove. This mirrors the way that array indexing works: if we ask for any index of an empty array, we get undefined.

5.
```js
const a = [];
a[0];
RESULT:
undefined

const a = [];
a.pop();
RESULT:
undefined
```

Some array methods return a new array. push and pop do not. Instead, they change the array itself each time they're called.

6.
```js
const a = [1, 2, 3];
a.pop();
a.pop();
a;
RESULT:
[1]

const a = [1, 2, 3];
a.pop();
a.push('a');
a;
RESULT:
[1, 2, 'a']
```

## Lesson 3

## Lesson 4

## Lesson 5

