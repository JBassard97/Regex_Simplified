# Regex Simplified

Have you ever tried to create an account on a website, and when you put in your desired username and password, you receive a message reading: 'Must be a Valid Email?' Oh, you must have mistyped `..` before the 'com'. But how does the website know that what you put doesn't meet the specific formatting needed to proceed? And what enables it to recognize and allow a variety of email addresses, like 'support@email.com', 'help.desk@hcps.net', and 'john_doe@gmail.com'? This is possible through a 'regex', and the best way to describe it is by giving an example and breaking it all down.

## Summary

This scary looking line of code is called a 'Regular Expression,' or a 'regex':

### **^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$**

Fear not! This allows for emails to contain alphanumeric characters (letters and numbers), and ensures it has the correct punctuations in the right places. This is called a 'regular expression', or 'regex', and it defines a search pattern within a string. Here's a look at how this regex is broken down:

- `^` // Start of the string
- `([a-z0-9_\.-]+)` // Group 1: Matching one or more of the characters inside the brackets
- `@` // Matching the literal '@' symbol
- `([\da-z\.-]+)` // Group 2: Matching one or more of the characters inside the brackets
- `.` // Matching the literal '.' symbol
- `([a-z\.]{2,6})` // Group 3: Matching between 2 and 6 characters inside the brackets
- `$` // End of the string

What this means is that if the string you've provided and the regex matches, it'll usually be used to return a boolean, or an array of the matches, that can give the 'go-ahead' to other processes in your application. But how can you infer so much from this mess? It's not too difficult, but we'll want to review how to use a 'regex' in a JavaScript/NodeJS environment first:

## How to Use

#### 1. Simple pattern matching with a `Regex Literal` (pattern encased in `/.../`)

```JavaScript
    const myRegex = /peanut/;
    const foodText = 'peanut butter';
    console.log(myRegex.test(foodText));
    // true
```

#### 2. Using the `RegExp` Object to create one, (pattern as a **string**)

```JavaScript
const myRegex = new RegExp('nop');
const alphabet = 'abcdefghijklmnopqrstuvwxyz';
console.log(alphabet.match(myRegex));
// ['nop']
```

##### The `test()` method returns a boolean if the pattern matches in the text. It is _used on the Regex itself_ and the text you want to test is passed in as an argument

##### The `match()` method returns an array of the matches, or `null` if nothing is found. It is _used on the string_, as it is a string method, and not on the Regex itself

These functions can both have [flags](#flags) passed in as arguments to change their search behavior (such as case insensitive). We'll cover that later!

**ORMs** (Object-Relational Mappers) and many modern technologies abstract this process for developers, but deep down they still rely on regex patterns for input validation. Let's break this example down piece by piece and learn more about both how it's made and how it works.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### **Anchors**

When looking at our example, let's begin with the first character: `^`. You'll notice that at the end of the regex is a `$`. This is called an 'anchor,' and it specifies where a match has to be within a string. Common anchors include:

    ^ (Caret): Matches at the start of the string

    $ (Dollar Sign): Matches at the end of the string

### **Quantifiers**

A quantifier in a regex allows you to specify how many times an element should be matched. In our [Email Regex](#summary), we have `([a-z\.]{2,6})` before the `$` anchor. Specifically, the `{2,6}` requires between 2 and 6 characters that are lowercase letters or `.`. This ensures that emails could end with '.ed', '.com', '.org', etc. Here are the most common quantifiers:

    * (Zero or More)

    + (One or More)

    ? (Zero or One)

    {n} (Exactly n Times)

    {n,} (n or More Times)

    {n, m} (Between n and m Times)

The `+` quantifier is also used in our [example](#summary) to require that one or more characters in the string must match the preceding groupings. It is encased in square brackets `[]`.

### OR Operator

While the OR Operator doesn't make an appearance in our example, it's very useful for searching with two patterns at once. This creates flexibility by providing multiple options for a part of your pattern. OR also takes precedence, meaning it's like the 'P' in 'PEMDAS', and very often will use parentheses `()` to group elements when combining with other regex elements. Some examples:

    'men|women' Matches either 'men' or 'women'

    'gr(a|e)y' Matches either 'gray' or 'grey'

    'cat|dog\sfood' Matches either 'cat' or 'dog food'

    '(blue|red)\sballoon' Matches either 'blue balloon' or 'red balloon'

### **Character Classes**

Character classes are used to define any set of characters that you'd want to match. They are extremely prevalent and powerful. Our [Email Regex](#summary) uses them multiple times throughout. They are always enclosed in `[]`, and can have many different components, so here are the most ones you'll see in the wild:

#### Basic Character Class

Any list of characters inside square brackets

    [aeiou] Matches any vowel

#### Ranges

Specify a range of characters using a hyphen `-`

    [0-9]   Matches any digit from 0 to 9

    [A-Z]   Matches any uppercase letter from A to Z

    [a-z]   Matches any lowercase letter from a to z

    [a-zA-Z0-9]   Matches any alphanumeric character

    [a-zA-Z0-9_]    Matches any alphanumeric character and `_`

The [Email Regex](#summary) uses the range `[a-z0-9_\.-]` to match all lowercase and numeric characters, as well as the characters: `_`, `.`, and `-`.

#### Negation

Adding a caret `^` at the beginning of the character class will negate it

    [^a-zA-Z] Matches any non-alphabetic character

#### Shorthand Character Classes

The most common character classes can be expressed using shorthand, saving your eyes a little.

    \d  Matches any digit (same as [0-9])

    \D  Matches any non-digit character (same as [^0-9])

    \w  Matches any word charcter (same as [a-zA-Z0-9_])

    \W  Matches any non-word character (same as [^a-zA-Z0-9_])

    \s  Matches any whitespace character (spaces, newlines, tabs)

    \S  Matches any non-whitespace character

While our [example](#summary) doesn't use this, we can see how shorthands, [anchors](#anchors), and [quantifiers](#quantifiers) are used in the Phone Number Regex `^\d{3}-\d{3}-\d{4}$`

#### Escaping

Since a lot of the special characters we might want to compare against are already in use syntactically, we may need to 'escape' them with a backslash `\`

    [\^$.*] Matches any of the characters `^`, `$`, `.`, or `*`

### **Flags**

In regular expressions, flags can be used to change the behavior of pattern matching.

    i (Case-Insensitive) Performs a case-insensitive match

    m (Multi-line) Sets the `^` and `$` anchors to match the start/end of each line in the input string

    g (Global) Will match all occurrences in an input string, not only the first one

These can be passed in differently depending on which method [above](#summary) you're using, so here is an example of each:

#### 1. Using a flag in a `Regex Literal`

```JavaScript
    const myRegex = /xyz/g;
    const text = 'xyz xyz xyz'
    console.log(text.match(myRegex));
    // ['xyz', 'xyz', 'xyz']
```

#### 2. Passing a flag to `RegExp` Object

```JavaScript
    const myRegex = new RegExp('nop', 'i');
    const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
    console.log(myRegex.test(alphabet));
    // true
```

### **Grouping and Capturing**

#### Grouping

Grouping is done by enclosing a portion of the regular expression in parentheses `()`. These groups can apply quantifiers and other operations to that specific part of the pattern. Opting out of grouping can change the behavior of your code unexpectedly. It can also help with readability. For example, each 'word' of a typical email address in the [example](#summary) is a grouping.

#### Capturing

Capturing is the ability to save matched content within one of these groupings for later use. As covered in the [How to Use](#how-to-use) section, the `match()` method returns an array of all of its matches. Creating groupings in your regex allows for more specific saving within that returned array, giving you access to what specifically made that match work.

```JavaScript
    const regexWithCapture = /(ab\d+)/;
    const text = 'ab123 ab456';
    const match = text.match(regexWithCapture);

    console.log(match[0]);
    // Output: 'ab123' (the whole match)
    console.log(match[1]);
    // Output: 'ab123' (content captured by the group)
```

#### Non-Capturing Group

If you want to group a part of your regular expression without worrying about capturing the content, you can begin your group with `?:`.

```JavaScript
    const regexNonCapturing = /(?:ab\d+)/;
    const text = 'ab123 ab456';
    console.log(text.match(regexNonCapturing));
    // Output: [ 'ab123' ]
```

#### Nested Groups

More complex patterns can be achieved by nesting groups.

```JavaScript
    const nestedRegex = /(ab(\d+))/;
    const text = 'ab123 ab456';
    const match = text.match(nestedRegex);

    console.log(match[0]);
    // Output: 'ab123'
    console.log(match[1]);
    // Output: 'ab123'
    console.log(match[2]);
    // Output: '123' (content captured by the nested group)
```

### **Bracket Expressions**

A bracket expression is essentially another name for [Character Classes](#character-classes), and it operates in the same way. They are always enclosed in square brackets `[]` and define what you're trying to match against. They accept [ranges](#ranges), [negation](#negation), [shorthand](#shorthand-character-classes), [escape characters](#escaping), and the [OR operator](#or-operator).

### **Greedy and Lazy Match**

'Greedy' and 'Lazy' are terms that describe the behavior of [quantifiers](#quantifiers) in regular expressions. Remember how quantifiers specify how many matches there has to be? Knowing the diffence between greedy and lazy matching allows you to alter that behavior even further

#### Greedy Matching

Greedy quantifiers try to match as much as possible while still allowing the overall match to succeed. By default, quantifiers in regular expressions are greedy. Here are the greediest quantifiers:

    * (Zero or more)

    + (One or more)

    ? (Zero or one)

Here is an example of a greedy quantifier:

```JavaScript
    const greedyRegex = /a.*b/;
    console.log('aXbYb'.match(greedyRegex));
    // Output: [ 'aXbYb', index: 0, input: 'aXbYb', groups: undefined ]
```

In this example `*` is greedy because it matches as much as possible. It includes the rest of the string in the output despite only needing to find 'a' to the first 'b'.

#### Lazy Matching

Lazy or non-greedy quantifiers match as little as possible while still allowing the overall match to succeed. They are written by adding a `?` after each of the above greedy quantifiers.

    *? (Lazy Zero or more)

    +? (Lazy One or more)

    ?? (Lazy Zero or one)

Here is the above example of a greedy quantifier, but with using lazy matching instead:

```JavaScript
    const lazyRegex = /a.*?b/;
    console.log('aXbYb'.match(lazyRegex));
    // Output: [ 'aXb', index: 0, input: 'aXbYb', groups: undefined ]
```

Here it is lazy because it only returns 'a' to the first 'b'. It is very specific in its search breadth.

### **Boundaries**

In regular expressions, boundaries are used to define where in a string where a match should occur. They work similarly to [anchors](#anchors), but focus on operating within the string rather than just at the beginning or end. Boundaries do not match any characters themselves, but they are more about the transition between word and non-word characters and the positions around them.

    \B (Non-Word Boundary) Asserts where a word Character IS followed or preceded by another word

    \b (Word Bondary) Asserts where a word character IS NOT followed or preceded by another word

Here are some examples of how these boundaries could work:

```JavaScript
    const regex = /\bword/;
    console.log('word'.match(regex));
    // Output: [ 'word', index: 0, input: 'word boundary', groups: undefined ]
```

This regex is looking for the word 'word' surrounded by word boundaries. Here 'word' is found at the beginning of the string and the match includes the entire word.

```JavaScript
    const regex = /\Bword\B/;
    console.log('awordb'.match(regex));
    // Output: [ 'word', index: 1, input: 'awordb', groups: undefined ]
```

This regex looks for 'word' surrounded by non-word boundaries. Here 'word' is within 'awordb', yet we are able to match the substring and return it with `match()`.

### **Back-references**

Back-references enable you to refer to previously captured groups with the same regex pattern. You can match the same content captured in a group earlier in that pattern. The syntax for this is:

    \n (Where n is the number of the capturing group)
    \1 (Refers to content captured in 1st group)
    \2 (Refers to content captured in 2nd group)
    repeat...

```JavaScript
    const regex = /(\w+) or \1/;
    const text = 'apple or apple';
    const match = text.match(regex);
    console.log(match);
    // Output: [ 'apple and apple', 'apple', index: 0, input: 'apple and apple', groups: undefined ]
```

In this example, `(\w+)` is a capturing group that matches one or more word characters. `or` is a literal string, and the input string must contain this exactly. `\1` is the back reference that uses the first capturing group to ensure the second 'apple' is also matched.

### **Look-ahead and Look-behind**

These are advanced concepts that allow you to check whether a certain pattern is ahead of or behind the current matching position without actually including it in the match. They also go by the name 'assertions.'

    ?= (Look-ahead) Asserts that a pattern is present ahead of the current position

    ?! (Negative Look-ahead) Asserts that a pattern is NOT ahead of the current position

    ?<= (Look-behind) Asserts that a pattern is present behind the current position

    ?<! (Negative Look-behind) Asserts that a pattern is NOT present behind the current position

And here are examples and further explainations of each:

#### Look-ahead `(?=...)`

```JavaScript

    const regex = /\w+(?=\sand)/;
    const match = 'apple and orange'.match(regex);
    console.log(match);
    // Output: [ 'apple', index: 0, input: 'apple and orange', groups: undefined ]
```

Here, we are matching one or more word characters only if they're followed by a `space` and `and`. This returns 'apple' alone because it fills that condition

#### Negative Look-ahead `(?!...)`

```JavaScript

    const regex = /\w+(?!\sorange)/;
    const match = 'apple and banana'.match(regex);
    console.log(match);
    // Output: [ 'apple', index: 0, input: 'apple and banana', groups: undefined ]
```

Here, we are matching one or more word characters only if they're NOT followed by a `space` and the word `orange`. 'apple' is returned alone because banana doesn't have the unwanted pattern

#### Look-behind `(?<=...)`

```JavaScript

    const regex = /(?<=\sand\s)\w+/;
    const match = 'apple and orange'.match(regex);
    console.log(match);
    // Output: [ 'orange', index: 10, input: 'apple and orange', groups: undefined ]
```

Here, we are matching one or more word characters only if they are preceded by `and` and have spaces on both sides. In this case, the match is 'orange.'

#### Negative Look-behind `(?<!...)`

```JavaScript

    const regex = /(?<!\sapple\s)\w+/;
    const match = "banana and apple".match(regex);
    console.log(match);
    // Output: [ 'and', index: 7, input: 'banana and apple', groups: undefined ]
```

Here, we are matching one or more word characters only if they are NOT preceded by `apple` with `spaces` on both sides. 'and' is the only match that fulfills those requirements

## **Author**

_Jonathan "Johnny" Acciarito_ is a web developer based in Chapel Hill, NC. Writing detailed documentation and tutorials that anyone can follow is one of his strong suits. He is currently enrolled in UNC's Full Stack Coding Bootcamp and is loving it. He aspires to make a full career change, so he can allow his knack of creative problem-solving to flourish. He aims to escape food-service entirely.

**Find me on:**

- [GitHub](https://github.com/JBassard97)
- [LinkedIn](www.linkedin.com/in/jonathan-acciarito-46434b2aa)

**Projects I'm in:**

- [BookShelf](https://book-shelf-ec28e1e38c1b.herokuapp.com/)
- [Code-Corner](https://code-corner-d43268fe7948.herokuapp.com/)
