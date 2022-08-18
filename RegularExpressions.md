# Regular Expressions

## Lesson 1 Regular Expressions: Literals

Regular expressions (regexes) are patterns that describe strings. We might write a regex for filenames ending in ".jpg". Or we might write one that recognizes phone numbers. We'll use JavaScript's regexes, but it's OK if you don't know JavaScript.

We can ask whether a regex matches a given string: /a/.test('cat'). The regex /a/ means "any string containing an a". We asked about 'cat', which contains an "a", so we'll get true back.

1.
```js
/a/.test('a');
RESULT:
true

/a/.test('b');
RESULT:
false

/a/.test('xax');
RESULT:
true
```

Regexes are case-sensitive, so "a" and "A" are different characters.

2.
```js
/A/.test('a');
RESULT:
false

/A/.test('A');
RESULT:
true
```

Multi-character regexes test that the characters appear together. They have to be adjacent, with no other characters between them.

3.
```js
/cat/.test('cart');
RESULT:
false

/cat/.test('cat');
RESULT:
true

/cat/.test('the cat there');
RESULT:
true
```

Spaces are treated as normal characters. If they're in the regex, they'll have to be in the matched string too. Likewise for _, -, and some other punctuation. (But some punctuation has special meaning, which we'll see soon.)

4.
```js
/a cat/.test('that is a cat');
RESULT:
true

/_/.test('happy-cat');
RESULT:
false

/_/.test('happy_cat');
RESULT:
true
```

## Lesson 2 Regular Expressions: Wildcard

Regexes like /a/ are literal: they specify exact characters to match. The real power in regexes is in the various operators. The most basic is ., the wildcard operator. It matches any character. But the character must be present; . won't match the empty string.

1.
```js
/./.test('a');
RESULT:
true

/./.test('b');
RESULT:
true

/./.test('>');
RESULT:
true

/./.test('');
RESULT:
false
```

There's one important special case for /./: it doesn't match newlines!

2.
```js
/./.test('\n');
RESULT:
false
```

Putting . next to another character means that they occur consecutively. For example, /a./ matches an "a" followed by any character. And /.a/ matches any character followed by an "a".

3.
```js
/x.z/.test('xyz');
RESULT:
true

/x.z/.test('xaz');
RESULT:
true

/x.z/.test('xyyz');
RESULT:
false

/x.z/.test('x_z');
RESULT:
true
```

When there are multiple .s, they can match different characters.

4.
```js
/x..z/.test('xaaz');
RESULT:
true

/x..z/.test('xaz');
RESULT:
false

/x..z/.test('xabz');
RESULT:
true
```

## Lesson 3 Regular Expressions: Boundaries

Often, we want to match text at the beginning and end of strings. We'll use boundaries for that. The first is ^, which means beginning of string.

1.
```js
/^cat/.test('cat');
RESULT:
true

/^cat/.test('cats are cute');
RESULT:
true

/^cat/.test('I like cats');
RESULT:
false

/^cat/.test('cats like dogs');
RESULT:
true

/^cat/.test('dogs like cats');
RESULT:
false
```

The second boundary is $, which means end of string.

2.
```js
/cat$/.test('a dog');
RESULT:
false

/cat$/.test('a cat');
RESULT:
true

/cat$/.test('cats');
RESULT:
false
```

Usually, we want to match an entire string. Most regexes should include these boundaries, ^ and $.

3.
```js
/^a$/.test('a');
RESULT:
true

/^a$/.test('the letter a');
RESULT:
false

/^a$/.test('a cat');
RESULT:
false
```

To match only the empty string, we can smash the ^ and $ together.

4.
```js
/^$/.test('a');
RESULT:
false

/^$/.test(' ');
RESULT:
false

/^$/.test('');
RESULT:
true
```

If we use the wrong boundaries, there's no error. The regex always returns false.

5.
```js
/$cat^/.test('cat');
RESULT:
false
```

Also, if we use boundaries in the wrong place, the regex will always return false.

6.
```js
/cat$s/.test('cats');
RESULT:
false

/cat^s/.test('cat');
RESULT:
false
```

## Lesson 4 Regular Expressions: Repetition

The + operator requires something to occur one or more times.

1.
```js
/a+/.test('aaa');
RESULT:
true

/a+/.test('a');
RESULT:
true

/a+/.test('');
RESULT:
false

/cat/.test('caaat');
RESULT:
false

/ca+t/.test('caaat');
RESULT:
true

/ca+t/.test('ct');
RESULT:
false
```

+ works with ., just like it does with literal characters.

2.
```js
/.+/.test('a');
RESULT:
true

/.+/.test('cat');
RESULT:
true

/.+/.test('');
RESULT:
false

/a.+z/.test('aveloz');
RESULT:
true

/a.+z/.test('az');
RESULT:
false
```

The * operator is similar to +, but means "zero or more times".

3.
```js
/a*/.test('a');
RESULT:
true

/a*/.test('aa');
RESULT:
true

/a*/.test('');
RESULT:
true

/a.*z/.test('aveloz');
RESULT:
true

/a.*z/.test('az');
RESULT:
true
```

We can use multiple + and *s in the same regex.

4.
```js
/a+b*c+/.test('aacc');
RESULT:
true

/a+b*c+/.test('aa');
RESULT:
false

/a+b*c+/.test('aabccc');
RESULT:
true

/a+b*c+/.test('abbbbc');
RESULT:
true

/a+b*c+/.test('bc');
RESULT:
false
```

## Lesson 5