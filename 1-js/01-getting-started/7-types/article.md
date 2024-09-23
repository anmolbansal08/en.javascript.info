# Data types

A value in JavaScript is always of a certain type. For example, a string or a number.

There are 8 basic data types in JavaScript. Here we'll introduce them, and in the next chapters we'll talk about each of them in detail.

JavaScript is a "dynamically typed" language. It means that a variable can store a value of any type, and it can change dynamically during execition.

For example, `message` can store a string, and then be reassigned to a number:

```js
// no error
let message = "hello";
message = 123456;
```

## Number

```js
let n = 123;
n = 12.345;
```

The *number* type represents both integer and floating point numbers.

There are many operations for numbers, e.g. multiplication `*`, division `/`, addition `+`, subtraction `-`, and so on.

Besides regular numbers, there are so-called "special numeric values" which also belong to this data type: `Infinity`, `-Infinity` and `NaN`.

- `Infinity` represents the mathematical [Infinity](https://en.wikipedia.org/wiki/Infinity) ∞. It is a special value that's greater than any number.

    We can get it as a result of division by zero:

    ```js run
    console.log( 1 / 0 ); // Infinity
    ```

    Or just reference it directly:

    ```js run
    console.log( Infinity ); // Infinity
    ```
- `NaN` represents a computational error. It is a result of an incorrect or an undefined mathematical operation, for instance:

    ```js run
    console.log( "hello" / 2 ); // NaN, such division is erroneous
    ```

    `NaN` is "sticky". Any further mathematical operation on `NaN` returns `NaN`:

    ```js run
    console.log( NaN + 1 ); // NaN
    console.log( 3 * NaN ); // NaN
    console.log( "hello" / 2 - 1 ); // NaN

    // The only exception to this rule is:
    console.log( NaN ** 0 ); // 1
    // the double star ** is a "raising to power" operation
    ```

    This is natural, because `NaN` value represents an error, and there's no sense in doing computations with an error. So, if there's a `NaN` somewhere in a mathematical expression, it propagates to the whole result.

```smart header="Mathematical operations are safe"
Doing maths is "safe" in JavaScript. We can do anything: divide by zero, treat non-numeric strings as numbers, etc.

The script will never stop with a fatal error ("die"). At worst, we'll get `NaN` as the result.
```

Special numeric values formally belong to the "number" type. Of course they are not numbers in the common sense of this word.

## BigInt [#bigint-type]

In JavaScript, the "number" type cannot safely represent integer values larger than <code>(2<sup>53</sup>-1)</code> (that's `9007199254740991`), or less than <code>-(2<sup>53</sup>-1)</code> for negatives. There's a similar limitation for floating type values as well.

Technically, we can assigns any numeric value, and there won't be an error. However, if the number is outside of this range, then some digits "won't fit" and will be lost, so we'll see an "approximate" value when we try to read from it.

For example, let's try to store these two numbers, which are right above the safe range:

```js
console.log(9007199254740991 + 1); // 9007199254740992
console.log(9007199254740991 + 2); // 9007199254740992
```

As you can see, both outputs are identical. This is exactly for this reason – they don't fit and a tiny bit of precision is lost.

For most real-life purposes <code>±(2<sup>53</sup>-1)</code> range is quite enough. However, sometimes we need to work with really big numbers, e.g. for cryptography purposes or to store a time with microsecond precision.

`BigInt` type can represent integers of arbitrary length.

A `BigInt` value is created by appending `n` to the end of an integer:

```js
// the "n" at the end means it's a BigInt
const bigInt = 1234567890123456789012345678901234567890*!*n*/!*;
```

## String

A string in JavaScript must be enclosed into quotes, like this:

```js
let str = "Hello";
```

In JavaScript, there are 3 types of quotes.

1. Double quotes: `"Hello"`.
2. Single quotes: `'Hello'`.
3. Backticks: <code>&#96;Hello&#96;</code>.

Double and single quotes are "simple" quotes. There's no difference between them in JavaScript, besides being different characters.

Backticks are "extended functionality" quotes. They allow us to embed variables and expressions into a string by wrapping them in `${…}`, for example:

```js run
let name = "John";

// embed a variable
console.log( `Hello, *!*${name}*/!*!` ); // Hello, John!

// embed an expression
console.log( `the result is *!*${1 + 2}*/!*` ); // the result is 3
```

The expression inside `${…}` is evaluated and the result becomes a part of the string. We can put anything in there: a variable like `name` or an arithmetical expression like `1 + 2` or something more complex.

Please note that this can only be done in backticks. Other quotes don't have this embedding functionality!
```js run
console.log( "the result is ${1 + 2}" ); // the result is ${1 + 2} (double quotes do nothing)
```

We'll cover strings more thoroughly in the chapter <info:string>.

```smart header="There is no *character* type."
In some languages, there is a special "character" type for a single character. For example, in the C language and in Java it is called "char".

In JavaScript, there is no such type. There's only one type: `string`. A string may consist of zero characters (be empty), one character or many of them.
```

## Boolean (logical type)

The boolean type has only two values: `true` and `false`.

This type is commonly used to store yes/no values: `true` means "yes, correct", and `false` means "no, incorrect".

For instance:

```js
let nameFieldChecked = true; // yes, name field is checked
let ageFieldChecked = false; // no, age field is not checked
```

Boolean values also come as a result of comparisons:

```js run
let isGreater = 4 > 1;

console.log( isGreater ); // true (the comparison result is "yes")
```

We'll cover booleans more deeply in the chapter <info:logical-operators>.

## The "null" value

The special `null` value does not belong to any of the types described above.

It forms a separate type of its own which contains only the `null` value:

```js
let age = null;
```

In JavaScript, `null` is not a "reference to a non-existing object" or a "null pointer" like in some other languages.

It's just a special value which represents "nothing", "empty" or "value unknown".

The code above states that `age` is unknown.

## The "undefined" value

The special value `undefined` also stands apart. It makes a type of its own, just like `null`.

The meaning of `undefined` is "value is not assigned".

If a variable is declared, but not assigned, then its value is `undefined`:

```js run
let age;

console.log(age); // shows "undefined"
```

Technically, it is possible to explicitly assign `undefined` to a variable:

```js run
let age = 100;

// change the value to undefined
age = undefined;

console.log(age); // "undefined"
```

...But we don't recommend doing that. Normally, one uses `null` to assign an "empty" or "unknown" value to a variable, while `undefined` is reserved as a default initial value for unassigned things.

## Objects and Symbols

The `object` type is special.

All other types are called "primitive" because their values can contain only a single thing (be it a string or a number or whatever). In contrast, objects are used to store collections of data and more complex entities.

Being that important, objects deserve a special treatment. We'll deal with them later in the chapter <info:object>, after we learn more about primitives.

The `symbol` type is used to create unique identifiers for objects. We have to mention it here for the sake of completeness, but also postpone the details till we know objects.

## The typeof operator [#type-typeof]

The `typeof` operator returns the type of the operand. It's useful when we want to process values of different types differently or just want to do a quick check.

A call to `typeof x` returns a string with the type name:

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

*!*
typeof Math // "object"  (1)
*/!*

*!*
typeof null // "object"  (2)
*/!*

*!*
typeof console.log // "function"  (3)
*/!*
```

The last three lines may need additional explanation:

1. `Math` is a built-in object that provides mathematical operations. We will learn it in the chapter <info:number>. Here, it serves just as an example of an object.
2. The result of `typeof null` is `"object"`. That's an officially recognized error in `typeof`, coming from very early days of JavaScript and kept for compatibility. Definitely, `null` is not an object. It is a special value with a separate type of its own. The behavior of `typeof` is wrong here.
3. The result of `typeof console.log` is `"function"`, because `console.log` is a function. We'll study functions in the next chapters where we'll also see that there's no special "function" type in JavaScript. Functions belong to the object type. But `typeof` treats them differently, returning `"function"`. That also comes from the early days of JavaScript. Technically, such behavior isn't correct, but can be convenient in practice.

```smart header="The `typeof(x)` syntax"
You may also come across another syntax: `typeof(x)`. It's the same as `typeof x`.

To put it clear: `typeof` is an operator, not a function. The parentheses here aren't a part of `typeof`. It's the kind of parentheses used for mathematical grouping.

Usually, such parentheses contain a mathematical expression, such as `(2 + 2)`, but here they contain only one argument `(x)`. Syntactically, they allow to avoid a space between the `typeof` operator and its argument, and some people like it.

Some people prefer `typeof(x)`, although the `typeof x` syntax is much more common.
```

## Summary

There are 8 basic data types in JavaScript.

- Seven primitive data types:
    - `number` for numbers of any kind: integer or floating-point, integers are limited by <code>±(2<sup>53</sup>-1)</code>.
    - `bigint` for integer numbers of arbitrary length.
    - `string` for strings. A string may have zero or more characters, there's no separate single-character type.
    - `boolean` for `true`/`false`.
    - `null` for unknown values -- a standalone type that has a single value `null`.
    - `undefined` for unassigned values -- a standalone type that has a single value `undefined`.
    - `symbol` for unique identifiers.
- And one non-primitive data type:
    - `object` for more complex data structures.

The `typeof` operator allows us to see which type is stored in a variable.

- Usually used as `typeof x`, but `typeof(x)` is also possible.
- Returns a string with the name of the type, like `"string"`.
- For `null` returns `"object"` -- this is an error in the language, it's not actually an object.

In the next chapters, we'll concentrate on primitive values and once we're familiar with them, we'll move on to objects.