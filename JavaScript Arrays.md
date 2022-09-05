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

## Lesson 3 JavaScript Arrays: For each

forEach executes a function once for each element in an array. Let's use it to sum an array of numbers.

1.
```js
const nums = [1, 2, 3];
let sum = 0;
nums.forEach(num => {
  sum = sum + num;
});
sum;
RESULT:
6
```

In the next example, we want to build a list of peoples' names. We use a for loop to add each name to an array. That requires looking elements up by their indexes i.

2.
```js
const people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
const names = [];
for (let i=0; i<people.length; i++) {
  names.push(people[i].name);
}
names;
RESULT:
['Amir', 'Betty']
```

forEach lets us write the same code without the index variable i. We pass a function to forEach, which runs the function on each element.

3.
```js
const people = [
  {name: 'Cindy'},
  {name: 'Dalili'},
];
const names = [];
people.forEach(person => {
  names.push(person.name);
});
names;
RESULT:
['Cindy', 'Dalili']
```

We can modify the array's elements during the forEach.

4.
```js
const people = [
  {name: 'Ebony'},
  {name: 'Fang'},
];
people.forEach(person => {
  person.name = person.name.toUpperCase();
});
people[0].name;
RESULT:
'EBONY'
```

The callback function can reference, and even modify, variables defined in outer scopes. In the next example, the callback function modifies the result variable.

5.
```js
const names = ['Gabriel', 'Hana'];
let result = '';
names.forEach(name => {
  result += name;
});
result;
RESULT:
'GabrielHana'
```

The second argument to forEach's callback is the item's index.

6.
```js
const names = ['Gabriel', 'Hana'];
const userIDs = [10, 11];
let result = '';
names.forEach((name, index) => {
  result += name + userIDs[index];
});
result;
RESULT:
'Gabriel10Hana11'
```

The examples above defined the callback functions inline, at the point where we called forEach. Functions are values in JavaScript, so we can pass them in other ways as well. For example, we can put the function in a variable, then pass the variable to forEach. The following examples define our forEach callback function in different ways, but they all have the same effect.

7.
```js
let sum = 0;
[1, 2, 3, 4].forEach(n => {
  sum += n;
});
sum;
RESULT:
10

let sum = 0;
function addToSum(n) {
  sum += n;
}
[1, 2, 3, 4].forEach(addToSum);
sum;
RESULT:
10

let sum = 0;
const addToSum = n => sum += n;
[1, 2, 3, 4].forEach(addToSum);
sum;
RESULT:
10
```

This function should return true when some of the numbers are positive. Currently it's broken: it always returns false. That's because we tried to return true inside the forEach callback. But the callback's return value is simply thrown away, so that doesn't work.

Modify the function by using the sawPositiveNumber variable that we've already declared for you. Set it to true when a positive number is encountered. Then return it at the end of the hasPositiveNumbers function.

8.
```js
function hasPositiveNumbers(numbers) {
  let sawPositiveNumber = false;
  numbers.forEach(n => {
    if (n > 0) {
      sawPositiveNumber = true;
    }
  });
  return sawPositiveNumber;
}
[
  hasPositiveNumbers([]),
  hasPositiveNumbers([-2, -1, 0]),
  hasPositiveNumbers([-1, 0, 100]),
  hasPositiveNumbers([50]),
];
GOAL:
[false, false, true, true]
YOURS:
[false, false, true, true]
```

Keep that bug in mind! It's easy to make that mistake when using forEach, as well as other array functions that we'll see later like map and reduce.

## Lesson 4 JavaScript Arrays: Map

map calls a function on each element of an array. It returns a new array of the values returned from those function calls.

1.
```js
[1, 2, 3].map(num => num * 10);
RESULT:
[10, 20, 30]

['a', 'b', 'c'].map(x => x.toUpperCase());
RESULT:
['A', 'B', 'C']
```

map doesn't change the original array.

2.
```js
const nums = [1, 2, 3];
nums.map(num => num * 10);
nums[0];
RESULT:
1
```

In a previous lesson, we built an array of people's names using forEach. Here's that example again.

3.
```js
const people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
const names = [];
people.forEach(person => {
  names.push(person.name);
});
names;
RESULT:
['Amir', 'Betty']
```

There's an easier way. With map, we don't need to create and modify a new array. Instead, we can build the new array directly.

4.
```js
const people = [
  {name: 'Amir'},
  {name: 'Betty'},
];
people.map(person => person.name);
RESULT:
['Amir', 'Betty']
```

## Lesson 5

