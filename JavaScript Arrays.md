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

## Lesson 9 JavaScript Arrays: Arrays are objects

Most programming languages have an array data type that's separate from the language's other data types. JavaScript is unusual here: in JavaScript, arrays are a special type of object. We can see this by looking at typeof someArray, which will return 'object', just like it does for regular objects.

1.
```js
typeof {};
RESULT:
'object'

typeof [];
RESULT:
'object'

typeof ['a', 'b', 'c'];
RESULT:
'object'
```

Fortunately, this doesn't affect normal uses of arrays. When we store values at the array's indexes and read them back, everything works normally.

However, there are some weird effects if we look closely. Most notably, we can assign to arbitary properties of the array.

2.
```js
const arr = ['a', 'b', 'c'];
arr['five'] = 5;
arr.five;
RESULT:
5

const arr = ['a', 'b', 'c'];
arr.six = 6;
arr['six'];
RESULT:
6
```

Extra properties assigned to an array don't change its length.

3.
```js
const arr = ['a', 'b', 'c'];
arr['five'] = 5;
arr.length;
RESULT:
3
```

In most cases, adding extra properties to arrays in this way is a mistake. When other programmers read the code and see an array, they'll expect it to act as a normal array. They won't expect it to have these extra properties.

Here's an example. Imagine that we're building a service where one account can have multiple team members. The more team members a customer has, the more money we charge them, so we need to keep track of both the members and the limit.

4.
```js
const teamMembers = [
  'ebony@example.com',
  'fang@example.com',
];
teamMembers.limit = 3;
teamMembers.length < teamMembers.limit;
RESULT:
true
```

This works, but it's not a good idea because it's surprising. 999 out of 1,000 arrays are just arrays. When 1 in 1,000 of them also has extra properties like this, other programmers won't expect it, which can lead them to make mistakes. For example, they might build a new array and pass it to a function, not realizing that the function expects the array to have this unusual limit property.

It's better to use an object in this case.

5.
```js
const team = {
  members: [
    'ebony@example.com',
    'fang@example.com',
  ],
  memberLimit: 2,
};
team.members.length < team.memberLimit;
RESULT:
false
```

false
Now we have two nicely separate concepts: there's a list of team members, and there's a numerical limit on the number of members. Together, the members and the member limit form a team object. If a function needs the full team, we can pass that object; but if it only needs the members, we can pass only the array. Nothing about this is surprising or unusual.

A couple more details about extra properties on arrays. First, looping with forEach will ignore extra array properties.

6.
```js
const arr1 = ['a', 'b'];
const arr2 = [];
arr1.five = 5;
arr1.forEach(i => arr2.push(i));
arr2;
RESULT:
['a', 'b']
```

Second, extra properties will show up if we treat the array like an object. For example, we can use Object.keys to list the properties in an object. We'll get all of the array indexes (not the values), plus any extra properties that we added.

```js
const arr1 = ['a', 'b'];
arr1.five = 5;
Object.keys(arr1);
RESULT:
['0', '1', 'five']
```

(Object.keys is out of scope for this array course, so that example won't show up in reviews, but it's still worth seeing!)

## Lesson 10

JavaScript Arrays: Slice with negative arguments

We can slice from the end of the array by using negative indexes. You can think of a negative index -2 as array.length - 2. Or you can think of it as "two away from the end".

This example means "slice from the second-to-last element to the end".

1.
```js
const nums = [10, 20, 30, 40, 50];
nums.slice(nums.length - 2);
RESULT:
[40, 50]

[10, 20, 30, 40, 50].slice(-2);
RESULT:
[40, 50]

[10, 20].slice(-2);
RESULT:
[10, 20]
```

In an earlier lesson, we saw what happens when we slice past the end of the array with a positive argument. Here's a reminder: in the next example, there's no element at index 4, so we get an empty array.

2.
```js
[10, 20].slice(4, 5);
RESULT:
[]
```

With negative indexes, we can also slice before the beginning of the array. The result will only include elements in the original array. It won't invent additional elements to satisfy our out-of-bounds index.

3.
```js
[10, 20].slice(-100);
RESULT:
[10, 20]
```

Both begin and end can be negative. Remember that the end element isn't included in the slice.

4.
```js
[10, 20, 30].slice(-3, -1);
RESULT:
[10, 20]

[10, 20, 30, 40, 50].slice(-3, -1);
RESULT:
[30, 40]
```

We can mix positive and negative begin and end indexes.

5.
```js
[10, 20, 30, 40, 50].slice(1, -1);
RESULT:
[20, 30, 40]

[10, 20, 30, 40, 50].slice(-3, 4);
RESULT:
[30, 40]
```

## Lesson 11 JavaScript Arrays: Join

Sometimes we want to turn an array of strings into a single string. We can join them together.

1.
```js
['a', 'b', 'c'].join('');
RESULT:
'abc'

['Amir', 'Betty'].join('');
RESULT:
'AmirBetty'
```

If we omit join's argument, the strings are joined with ',' by default.

2.
```js
['Amir', 'Betty'].join();
RESULT:
'Amir,Betty'
```

join's argument is the string to put between each array element. It can be any string.

3.
```js
['a', 'b', 'c'].join('x');
RESULT:
'axbxc'

['a', 'b', 'c'].join('AND');
RESULT:
'aANDbANDc'
```

There's nothing special about joining empty strings. If we join some empty strings together, we get a string of only commas. Think about it as: the string has all of the commas in the original array.

```js
['', ''].join();
RESULT:
','

['', '', ''].join();
RESULT:
',,'

['a', '', 'b'].join();
RESULT:
'a,,b'
```

null and undefined become empty strings when joined.

4.
```js
[null, undefined].join();
RESULT:
','

['a', null, undefined, 'b'].join();
RESULT:
'a,,,b'
```

join can be used to build a large string using short lines of code.

5.
```js
function userTag(name) {
  return [
    '<User name="',
    name,
    '">'
  ].join('');
}
userTag('Amir');
RESULT:
'<User name="Amir">'
```

We can use join to build a properly formatted English list of words.

```js
const foods = ['peas', 'carrots', 'potatoes'];
foods.slice(0, -1);
RESULT:
['peas', 'carrots']
```

6.
```js
const foods = ['peas', 'carrots', 'potatoes'];
const joined = [
  foods.slice(0, -1).join(', '),
  ', and ',
  foods[foods.length - 1],
].join('');
joined;
RESULT:
'peas, carrots, and potatoes'
```

## Lesson 12 JavaScript Arrays: Index of

We can ask an array for the index of a particular value. (Indexes start at 0, as usual.)

1.
```js
const abc = ['a', 'b', 'c'];
abc.indexOf('a');
RESULT:
0
```

If the value occurs multiple times in the array, we'll get the index of the first occurrence.

2.
```js
const abc = ['a', 'b', 'c', 'c'];
abc.indexOf('c');
RESULT:
2
```

If we ask for an element that isn't in the array, we get -1.

3.
```js
const abc = ['a', 'b', 'c'];
abc.indexOf('d');
RESULT:
-1
```

It's important to check your indexOf calls for that -1 return value! Otherwise you can introduce subtle bugs. Here's an example.

Let's try to slice an array from a certain element forward. We don't check -1 from indexOf, so that -1 might be used as an index. (A hint in case you get stuck: [1, 2, 3].slice(-1) returns [3].)

4.
```js
const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('b'));
RESULT:
['b', 'c']

const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('c'));
RESULT:
['c']

const abc = ['a', 'b', 'c'];
abc.slice(abc.indexOf('d'));
RESULT:
['c']
```

We can fix that bug by checking for -1.

5.
```js
const abc = ['a', 'b', 'c'];
function sliceFromElement(array, element) {
  const index = array.indexOf(element);
  if (index === -1) {
    return null;
  } else {
    return array.slice(index);
  }
}
sliceFromElement(abc, 'd');
RESULT:
null
```

## Lesson 13 JavaScript Arrays: Filter

In procedural languages, we often make decisions inside loops. JavaScript is a procedural language.

1.
```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];
const admins = [];
for (let i=0; i<users.length; i++) {
  const user = users[i];
  if (user.admin) {
    admins.push(user);
  }
}
admins.map(user => user.name);
RESULT:
['Amir']
```

That loop was long and difficult to read. We can simplify it by using filter.

filter calls a function on each element in an array. If the function returns true, then filter keeps that element. Otherwise, it discards the element.

2.
```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];
users.filter(
  user => user.admin
).map(
  user => user.name
);
RESULT:
['Amir']

const arrays = [
  [1, 2],
  [2, 3],
  [3, 4],
];
arrays.filter(array => array.includes(2));
RESULT:
[[1, 2], [2, 3]]
```

JavaScript provides filter for us, but it's simple to implement.

3.
```js
function filter(array, callback) {
  const results = [];
  for (let i=0; i<array.length; i++) {
    if (callback(array[i])) {
      results.push(array[i]);
    }
  }
  return results;
}
filter([1, 2, 3], x => x !== 2);
RESULT:
[1, 3]
```

filter builds a new array. The original array isn't changed.

4.
```js
const users = [
  {name: 'Amir', admin: true},
  {name: 'Betty', admin: false},
];
users.filter(user => user.admin);
users.map(user => user.name);
RESULT:
['Amir', 'Betty']
```

## Lesson 14

## Lesson15