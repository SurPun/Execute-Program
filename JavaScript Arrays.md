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

## Lesson 5 JavaScript Arrays: Slice

Sometimes we want to access a subsection of an array. For that, we use the slice method. It takes an argument begin, which is the index to start from.

```js
[10, 20, 30, 40, 50].slice(3);
RESULT:
[40, 50]
```

1.
```js
['a', 'b', 'c'].slice(1);
RESULT:
['b', 'c']
```

slice can take a second argument, end. It slices all elements from begin up to end, but not including end.

2.
```js
[10, 20, 30, 40, 50].slice(1, 2);
RESULT:
[20]

[10, 20, 30, 40, 50].slice(1, 3);
RESULT:
[20, 30]
```

We can slice beyond the end of the array. It gives the same result as slicing right up to the last element.

3.
```js
[10, 20].slice(0, 2);
RESULT:
[10, 20]

[10, 20].slice(0, 3);
RESULT:
[10, 20]
```

If our begin index is past the end of the array, we get an empty result.

Think of it like this. The array [10, 20] has indexes 0 and 1. So what's in indexes 2 through 3? There's nothing there. The slice of those indexes is empty.

4.
```js
[10, 20].slice(2, 3);
RESULT:
[]
```

With no arguments, slice will slice all elements of the array. This effectively copies the array. If we change the original, it won't affect the copy. Likewise, if we change the copy, it won't affect the original.

5.
```js
const orig = [10, 20, 30];
const copy = orig.slice();
copy[0] = 1;
orig;
RESULT:
[10, 20, 30]

const orig = [10, 20, 30];
const copy = orig.slice();
orig[0] = 1;
copy;
RESULT:
[10, 20, 30]
```

Slice is quite complex, but copying arrays is its most common use.

## Lesson 6 JavaScript Arrays: Concat

In many languages, + will combine two arrays. That's not true in JavaScript. Trying to do array1 + array2 is usually a mistake. JavaScript will convert the arrays into strings before adding them.

```js
[1, 2].toString();
RESULT:
'1,2'

[1, 2] + [3];
RESULT:
'1,23'
```

1.
```js
[1, 2] + [3, 4];
RESULT:
'1,23,4'
```

However, we can combine arrays properly with concat. (It stands for concatenate, which means "link together".) It creates a new array containing all of the elements from the old arrays.

2.
```js
[1, 2].concat([3, 4]);
RESULT:
[1, 2, 3, 4]
```

concat takes multiple arguments, so we can combine many arrays if needed:

3.
```js
[1, 2].concat([3, 4], [5, 6]);
RESULT:
[1, 2, 3, 4, 5, 6]
```

concat can also be used to add single elements to the end of an array. If its argument isn't an array, it will be added as a single element.

4.
```js
[1, 2].concat(3);
RESULT:
[1, 2, 3]
```

concat builds and returns a new array. The original arrays aren't changed.

5.
```js
const numbers1 = [1, 2];
const numbers2 = [3, 4];
numbers1.concat(numbers2);
numbers1;
RESULT:
[1, 2]
```

## Lesson 7 JavaScript Arrays: Includes

We can check for whether an array includes a given element.

1.
```js
['a', 'b'].includes('c');
RESULT:
false

['a', 'b'].includes('a');
RESULT:
true
```

Some methods are mercifully simple to learn.

## Lesson JavaScript Arrays: New and fill

The fill method fills an array with a given value. Any existing values will be overwritten by that value.

```js
const a = [1, 2];
a.fill(3);
RESULT:
[3, 3]
```

1.
```js
const a = ['a', 'b', 'c'];
a.fill('d');
RESULT:
['d', 'd', 'd']
```

What if we don't know how many "d"s we need in advance? First, we can create an array of a given size.

2.
```js
const size = 1 + 2;
new Array(size).length;
RESULT:
3
```

There's nothing in an array created in this way. If we ask for its elements, we'll get undefined.

```js
const size = 1 + 2;
new Array(size)[0];
RESULT:
undefined
```

We can fill this pre-sized array with values. This is a common reason to do new Array(size): to immediately fill it.

3.
```js
const size = 1 + 2;
new Array(size).fill('d');
RESULT:
['d', 'd', 'd']
```

Now we can create a dynamically-sized filled array.

4.
```js
function fillDynamically(size) {
  return new Array(size).fill('d');
}
fillDynamically(2);
RESULT:
['d', 'd']
```

The progress bar at the top of this lesson is made of many divs. We know how many code examples exist in the lesson. We create that many divs to make up the progress bar. This is done using the new Array(...).fill(...) method shown here!

## Lesson 9

## Lesson 10