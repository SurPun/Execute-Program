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