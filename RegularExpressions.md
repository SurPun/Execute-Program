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

## Lesson Regular Expressions: Maybe

The ? operator matches a character zero or one times, but not more than one.

1.
```js
/^a?$/.test('a');
RESULT:
true

/^a?$/.test('');
RESULT:
true

/^a?$/.test('b');
RESULT:
false

/^a?$/.test('aa');
RESULT:
false
```

The ? operator affects whatever is immediately before it. For example, in ab?, the ? operators only affects "b", not "a". We say that it binds tightly.

2.
```js
/^ab?$/.test('a');
RESULT:
true

/^ab?$/.test('b');
RESULT:
false

/^ab?$/.test('ab');
RESULT:
true
```

Often, part of a string is optional. For example, US telephone numbers sometimes have area codes. But for local calls, no area code is necessary. For optional strings like this, we can use the ? operator.

3.
```js
/^555-?555-5555$/.test('555-555-5555');
RESULT:
true
```

Unfortunately, this regex is too permissive. The ? only applies to the "-" character before it, so only that character is optional. All of the other characters are required.

4.
```js
/^555-?555-5555$/.test('555555-5555');
RESULT:
true
```

To make ? include more characters, we can group them using parentheses. Then we apply the ? to the whole group.

5.
```js
/^(555-)?555-5555$/.test('555-555-5555');
RESULT:
true

/^(555-)?555-5555$/.test('555-5555');
RESULT:
true

/^(555-)?555-5555$/.test('555555-5555');
RESULT:
false
```

## Lesson 9 Regular Expressions: Escaping

In most programming languages, strings are delimited with "double quotes". To put a literal double quote in a string, we escape it. The string "\"" contains exactly one double quote. (It's the same as '"'.)

In regexes, + normally repeats whatever comes before it. But we can escape the + as \+ to match a literal "+". Likewise for other operators like ..

1.
```js
/\./.test('That is a cat.');
RESULT:
true

/\./.test('Is that a cat?');
RESULT:
false

/.\+./.test('111');
RESULT:
false

/.\+./.test('1+1');
RESULT:
true

/\++/.test('+');
RESULT:
true

/\++/.test('++');
RESULT:
true

/\+\+/.test('++');
RESULT:
true

/\+\+/.test('+');
RESULT:
false
```

Escaping can be used when you need a literal ^, $, etc. (It works for any operator.)

2.
```js
/\^/.test('^');
RESULT:
true

/\$/.test('$');
RESULT:
true

/\$$/.test('$a');
RESULT:
false

/\$$/.test('a$');
RESULT:
true

/a|b/.test('|');
RESULT:
false

/a\|b/.test('a|b');
RESULT:
true
```

## Lesson 10 Regular Expressions: Basic character classes

Some sets of characters occur together frequently. For example, we often need to recognize numbers (the digits 0-9). We can use \d for that. It recognizes 0, 1, ..., 8, 9.

1.
```js
/\s/.test(' ');
RESULT:
true

/\s/.test('\n');
RESULT:
true

/\s/.test('');
RESULT:
false

/\s/.test('x\ty'); /* \t is a tab character */
RESULT:
true

/\s/.test('cat');
RESULT:
false

/\d/.test('0');
RESULT:
true

/\d/.test('9');
RESULT:
true

/\d/.test('a');
RESULT:
false
```

Making these character classes upper-case negates them. For example, \d is all digits. But \D is any character that isn't a digit.

2.
```js
/\D/.test('0');
RESULT:
false

/\S/.test(' ');
RESULT:
false

/\S/.test('0');
RESULT:
true
```

A character class matches only one character in the string. If we want to match multiple characters, we can use + or *.

3.
```js
/^\d$/.test('1000');
RESULT:
false

/^\d*$/.test('3000');
RESULT:
true

/^\d*$/.test('1000 cats');
RESULT:
false

/^\d*$/.test('cats');
RESULT:
false

/^\d*$/.test('');
RESULT:
true
```

## Lesson 11 Regular Expressions: Basic character sets

With "or" expressions, we can recognize a whole set of characters.

1.
```js
/^c(a|o|u)t$/.test('cat');
RESULT:
true
```

This gets tiresome if we need many options in the "or". Fortunately, we can use a character set to simplify it. The set [aou] is equivalent to (a|o|u).

2.
```js
/^c[aou]t$/.test('cat');
RESULT:
true

/^c[aou]t$/.test('cot');
RESULT:
true

/^c[aou]t$/.test('cet');
RESULT:
false
```

What if we want to allow any string of lower case letters? We'd have to write /(a|b|c|d|e| and so on. Instead, we can write another character set.

3.
```js
/[abcdefghijklmnopqrstuvwxyz]/.test('a');
RESULT:
true

/[abcdefghijklmnopqrstuvwxyz]/.test('g');
RESULT:
true
```

That was shorter, but still wordy. We can specify an entire range of characters by using -.

4.
```js
/[a-z]/.test('g');
RESULT:
true

/[1-3]/.test('1');
RESULT:
true

/[1-3]/.test('a');
RESULT:
false

/[1-3]/.test('2');
RESULT:
true
```

As usual, we escape special characters when we want them to be literal. This range contains only one character, an escaped ] written as \].

5.
```js
/[\]]/.test(']');
RESULT:
true
```

Character sets can be negated to mean "everything not in the set".

6.
```js
/[^a]/.test('a');
RESULT:
false

/[^a]/.test('5');
RESULT:
true
```

Negation applies to the entire character set. The regex /[^ab]/ means "any character other than a or b".

7.
```js
/[^ab]/.test('a');
RESULT:
false

/[^ab]/.test('c');
RESULT:
true

/[^ab]/.test('_');
RESULT:
true
```

Negation also applies to ranges.

8.
```js
/[a-z]/.test('h');
RESULT:
true

/[^a-z]/.test('h');
RESULT:
false

/[^a-z]/.test('5');
RESULT:
true
```

Character sets match exactly one character in the string. (This is like character classes, which also match only one character.) To match more than one character, we can use + or *.

9.
```js
/^[a-z]$/.test('cat');
RESULT:
false

/^[a-z]+$/.test('cat');
RESULT:
true
```

## Lesson 12 Regular Expressions: Character classes

Regexes have many special cases to solve common problems. For example, we often write regexes to process source code. Most code contains identifiers: variable names, function names, etc. In most languages, identifiers can have letters, numbers, and underscores.

We could write [A-Za-z0-9_] to mean "letters, numbers, and underscores." But that's verbose, and regexes are meant to be terse. Instead, we can use the character class \w. The "w" in \w stands for "word", which is another name for an identifier. This can be tricky: "word" has a special meaning in programming!

1.
```js
/\w/.test('a');
RESULT:
true

/\w/.test('+');
RESULT:
false

/\w/.test('F');
RESULT:
true

/\w/.test('_');
RESULT:
true

/a\wc/.test('abc');
RESULT:
true

/a\wc/.test('a-c');
RESULT:
false

/^\w$/.test('aaa');
RESULT:
false
```

We can match entire identifiers by combining \w with + and boundaries.

2.
```js
/^\w+$/.test('my_variable');
RESULT:
true

/^\w+$/.test('1+1');
RESULT:
false

/^\w+$/.test('while');
RESULT:
true

/^\w+$/.test('while (true)');
RESULT:
false
```

As with any character class, upper-casing the class negates it. (\W is the opposite of \w.)

3.
```js
/\w/.test('c');
RESULT:
true

/\W/.test('c');
RESULT:
false

/\w/.test('!');
RESULT:
false

/\W/.test('!');
RESULT:
true
```

## Lesson 13 Regular Expressions: Character sets

In a character set, characters and ranges can be mixed in any order. This regex is equivalent to /[g]|[c-e]|[a]/.

1.
```js
/[gc-ea]/.test('a');
RESULT:
true

/[gc-ea]/.test('b');
RESULT:
false

/[gc-ea]/.test('c');
RESULT:
true

/[gc-ea]/.test('d');
RESULT:
true

/[gc-ea]/.test('h');
RESULT:
false
```

We can also negate the whole character set, even if it's complex.

2.
```js
/[^gc-ea]/.test('a');
RESULT:
false

/[^gc-ea]/.test('d');
RESULT:
false

/[^gc-ea]/.test('b');
RESULT:
true
```

If a character set ever gives you trouble, you can always break it up. For example, /[hbd-fa]/ can be thought of as /(h|b|[d-f]|a)/. This trick works for anything in regexes. Most regex features are syntactic sugar for simple features like |.

Special characters aren't special inside a character set. For example, "." means a literal "." and "$" is a literal "$".

3.
```js
/[a$]/.test('$');
RESULT:
true

/[a$]/.test('a');
RESULT:
true

/[a$]/.test('^');
RESULT:
false

/^[a$]$/.test('ab');
RESULT:
false
```

The ^ is only special if it's the first character in the set. There, it means "negate this set". But a ^ anywhere else in the set is just another literal character.

4.
```js
/[^b]/.test('b');
RESULT:
false

/[b^]/.test('b');
RESULT:
true

/[b^]/.test('^');
RESULT:
true

/[^^]/.test('b');
RESULT:
true

/[^^]/.test('^');
RESULT:
false
```

## Lesson 14

## Lesson 15