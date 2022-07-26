# Modern JavaScript

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
