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

## Lesson 5 Regular Expressions: Or

Sometimes we need to allow multiple alternatives. We can separate them with a pipe character, |, pronounced "or".

1.
```js
/a|b/.test('a');
RESULT:
true

/a|b/.test('b');
RESULT:
true

/a|b/.test('c');
RESULT:
false

/at|og/.test('cat');
RESULT:
true

/at|og/.test('dog');
RESULT:
true

/at|og/.test('bat');
RESULT:
true

/at|og/.test('horse');
RESULT:
false
```

## Lesson 6 Regular Expressions: Hex codes

Computers internally store text as numbers. As a shorthand, we usually write those numbers out as hexadecimal codes.


1.
```js
/\x41/.test('A'); // 'A' is x41
RESULT:
true
```

For example, capital letter "A" has the hex code x41. Regular expressions and most programming languages allow this "x" syntax. To match "A" in a regex, we can write \x41.

2.
```js
/\x41/.test('an apple');
RESULT:
false

/\x41/.test('CATS ARE GOOD');
RESULT:
true
```

x42 is "B".

3.
```js
/\x42/.test('An apple');
RESULT:
false

/\x42/.test('Both apples');
RESULT:
true
```

Each digit can be a number, or a letter between "a" and "f". For example, "M" is x4d in hex. (The x isn't a hex digit; it's telling us that this is a hex code.)

4.
```js
/\x4d/.test('M');
RESULT:
true
```

There's a hex code for each character, even ones that aren't letters. "?" is x3f and "!" is x21.

5.
```js
/\x3f/.test('?');
RESULT:
true

/\x21/.test('!');
RESULT:
true
```

We can use hex codes to match symbols that you may not be able to type.

The letter "ø" doesn't appear on an US-English keyboard. To match that letter, we can use its hex code \xf8.

6.
```js
/\xf8/.test('søt katt');
RESULT:
true
```

If we have a keyboard that can type "ø", we can put it directly in a regex.

7.
```js
/ø/.test('smørbrød');
RESULT:
true
```

Be careful: the syntax \x can only be followed by exactly two digits. Anything after the two digits will be a different part of the regex.

8.
```js
/\x41d/.test('A');
RESULT:
false

/\x5Ad/.test('Zd'); // "Z" is x5A
RESULT:
true
```

If we write an \x with only one digit, it's no longer a character code. Instead, the \x matches literal "x" characters.

9.
```js
/\x4/.test('x4'); // This is probably a mistake with \x!
RESULT:
true
```

Hexadecimal digits can be any character from 0-9, a-f, or A-F. If we use the wrong characters, the \x will match literal "x" again.

10.
```js
/\xfg/.test('xfg');
RESULT:
true
```

## Lesson 7 Regular Expressions: Parens

This regex seems to match strings of exactly one "a" or exactly one "b". But that's not all it matches!

1.
```js
/^a|b$/.test('a');
RESULT:
true

/^a|b$/.test('b');
RESULT:
true

/^a|b$/.test('xa');
RESULT:
false

/^a|b$/.test('ax');
RESULT:
true
```

Why did /^a|b$/ accept the string "ax"? It makes more sense if we parenthesize the regex. (Parentheses group operators together, like in other programming languages.)

2.
```js
/(^a)|(b$)/.test('xa');
RESULT:
false

/(^a)|(b$)/.test('ax');
RESULT:
true
```

When we say /^a|b$/, it means the same as /(^a)|(b$)/. We might prefer it to mean /^(a|b)$/, but it is the way it is. Fortunately, adding those parentheses ourselves will help.

3.
```js
/^(a|b)$/.test('a');
RESULT:
true

/^(a|b)$/.test('xa');
RESULT:
false

/^(a|b)$/.test('ax');
RESULT:
false

/^(a|b)$/.test('xb');
RESULT:
false

/^(a|b)$/.test('b');
RESULT:
true

/^(a|b)$/.test('bx');
RESULT:
false
```

Imagine that we have a system that can read images. But it can only read JPGs, PNGs, and PDFs; anything else makes it crash. We want a regex that will accept images based on their filename.

4.
```js
/^(jpg|png|pdf)$/.test('jpg');
RESULT:
true

/^(jpg|png|pdf)$/.test('jpeg');
RESULT:
false

/^(jpg|png|pdf)$/.test('png');
RESULT:
true

/^(jpg|png|pdf)$/.test('pdf');
RESULT:
true
```

As in math or normal programming, parentheses can "factor out" common elements.

5.
```js
/^(jpg|p(ng|df))$/.test('jpg');
RESULT:
true

/^(jpg|p(ng|df))$/.test('jpeg');
RESULT:
false

/^(jpg|p(ng|df))$/.test('pdf');
RESULT:
true

/^(jpg|p(ng|df))$/.test('png');
RESULT:
true

/^(jpg|p(ng|df))$/.test('p');
RESULT:
false
```

We can use operators on parenthesized groups. In this example, the + matches strings of one or more characters. The (a|b) requires each of those characters to be an "a" or a "b".

6.
```js
/^(a|b)+$/.test('aaaaab');
RESULT:
true

/^(a|b)+$/.test('bababa');
RESULT:
true

/^(a|b)+$/.test('');
RESULT:
false
```

Either side of a | can be empty. This regex means "exactly the letter a, or the empty string".

7.
```js
/^(a|)$/.test('a');
RESULT:
true

/^(a|)$/.test('');
RESULT:
true

/^(a|)$/.test('aa');
RESULT:
false
```

## Lesson 8

## Lesson 9

## Lesson 10