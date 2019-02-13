---
title: "ES6: Destructuring for Easier Data Access"
date: "2019-02-13"
---

# JavaScript Interview Prep

## Destructuring for Easier Data Access

From _Understanding ECMAScript6_ by Nicholas Zakas (Chapter 5)

Object and array literals are two of the most frequently used notations in JavaScript, and thanks to the popular JSON data format, they have become a particularly important part of the language.

- It’s quite common to define objects and arrays, and then systematically pull out relevant pieces of information from those structures.
  - ECMAScript 6 simplifies this task by adding _destructuring_, which is the process of breaking down a data structure into smaller parts.
  - This chapter shows you how to harness destructuring for objects and arrays.

### Why Is Destructuring Useful?

In ECMAScript 5 and earlier, the need to fetch information from objects and arrays could result in a lot of duplicate code to get certain data into local variables.

- For example:

```js
let options = {
  repeat: true,
  save: false
};

// extract data from the object
let repeat = options.repeat,
  save = options.save;
```

This code extracts the values of `repeat` and `save` from the `options` object, and stores that data in local variables with the same names.

- Although this code looks simple, imagine if you had a large number of variables to assign: you would have to assign them all one by one.
- And if you had to traverse a nested data structure to find the information instead, you might have to dig through the entire structure just to find one piece of data.

That’s why ECMAScript 6 adds destructuring for objects _and_ arrays.

- When you break down a data structure into smaller parts, getting the information you need from it becomes much easier.

Many languages implement destructuring with a minimal amount of syntax to make the process simpler to use.

- The ECMAScript 6 implementation actually uses syntax you’re already familiar with: the syntax for object and array literals.

### Object Destructuring

Object destructuring syntax uses an object literal on the left side of an assignment operation.

- For example:

```js
let node = {
  type: "Identifier",
  name: "foo"
};

let { type, name } = node;

console.log(type); // "Identifier"
console.log(name); // "foo"
```

In this code, the value of `node.type` is stored in a variable called `type`, and the value of `node.name` is stored in a variable called `name`.

- This syntax is the same as the object literal property initializer shorthand introduced in Chapter 4.
- The identifiers `type` and `name` are both declarations of local variables and the properties to read the value from on the `node` object.

> **Don't Forget The Initializer**
> When you’re using destructuring to declare variables using `var`, `let`, or `const`, you must supply an initializer (the value after the equal sign). The following lines of code will all throw syntax errors due to a missing initializer:
>
> ```js
> // syntax error!
> var { type, name };
>
> // syntax error!
> let { type, name };
>
> // syntax error!
> const { type, name };
> ```
>
> Although `const` always requires an initializer, even when you’re using non-destructured variables, `var` and `let` require initializers only when you’re using destructuring.

#### _Destructuring Assignment_

The object destructuring examples you’ve seen so far have used variable declarations.

- However, it’s also possible to use destructuring in assignments
  - For instance, you might decide to change the values of variables after they’re defined, as follows:

```js
let node = {
    type: "Identifier",
    name: "foo"
  },
  type = "Literal",
  name = 5;

// assign different values using destructuring
({ type, name } = node);

console.log(type); // "Identifier"
console.log(name); // "foo"
```

In this example, `type` and `name` are initialized with values when declared, and then two variables with the same names are initialized with different values.

- The next line uses destructuring assignments to change those values by reading from the `node` object.
  - Note that you must put parentheses around a destructuring assignment statement.
    - The reason is that an opening curly brace is expected to be a block statement, and a block statement cannot appear on the left side of an assignment.
    - The parentheses signal that the next curly brace is not a block statement and should be interpreted as an expression, allowing the assignment to complete.

A destructuring assignment expression evaluates to the right side of the expression (after the `=`).

- That means you can use a destructuring assignment expression anywhere a value is expected.
  - For instance, consider this example, which passes a value to a function:

```js
let node = {
    type: "Identifier",
    name: "foo"
  },
  type = "Literal",
  name = 5;

function outputInfo(value) {
  console.log(value === node); // true
}

outputInfo(({ type, name } = node));

console.log(type); // "Identifier"
console.log(name); // "foo"
```

The `outputInfo()` function is called with a destructuring assignment expression.

- The expression evaluates to `node` because that is the value of the right side of the expression.
- The assignments to `type` and `name` behave normally, and `node` is passed to the `outputInfo()` function.

> **Note**: An error is thrown when the right side of the destructuring assignment expression (the expression after `=`) evaluates to `null` or `undefined`. This happens because any attempt to read a property of `null` or `undefined` results in a runtime error.

#### _Default Values_

When you use a destructuring assignment statement and you specify a local variable with a property name that doesn’t exist on the object, that local variable is assigned a value of undefined.

- For example:

```js
let node = {
  type: "Identifier",
  name: "foo"
};

let { type, name, value } = node;

console.log(type); // "Identifier"
console.log(name); // "foo"
console.log(value); // undefined
```

This code defines an additional local variable called `value` and attempts to assign it a value.

- However, no corresponding value property is on the `node` object, so the variable is assigned the value of `undefined` as expected.

You can optionally define a default value to use when a specified property doesn’t exist.

- To do so, insert an equal sign (`=`) after the property name and specify the default value, like this:

```js
let node = {
  type: "Identifier",
  name: "foo"
};

let { type, name, value = true } = node;

console.log(type); // "Identifier"
console.log(name); // "foo"
console.log(value); // true
```

In this example, the variable `value` is given `true` as a default value.

- The default value is only used if the property is missing on `node` or has a value of `undefined`.
- Because no `node.value` property exists, the variable `value` uses the default value.
  - This works similarly to the default parameter values for functions, as discussed in Chapter 3.

#### _Assigning to Different Local Variable Names_

Up to this point, each destructuring assignment example has used the object property name as the local variable name; for example, the value of `node.type` was stored in a `type` variable.

- That works well when you want to use the same name, but what if you don’t?

  - ECMAScript 6 has an extended syntax that allows you to assign to a local variable with a different name, and that syntax looks like the object literal non-shorthand property initializer syntax.

  - Here’s an example:

```js
let node = {
  type: "Identifier",
  name: "foo"
};

let { type: localType, name: localName } = node;

console.log(localType); // "Identifier"
console.log(localName); // "foo"
```

This code uses destructuring assignments to declare the `localType` and `localName` variables, which contain the values from the `node.type` and `node.name` properties, respectively.

- The syntax `type: localType` reads the property named `type` and stores its value in the `localType` variable.
  - This syntax is effectively the opposite of traditional object literal syntax, where the name is on the left of the colon and the value is on the right.
  - In this case, the name is on the right of the colon and the location of the value to read is on the left.

You can add default values when you’re using a different variable name, as well.

- The equal sign and default value are still placed after the local variable name.
  - For example:

```js
let node = {
  type: "Identifier"
};

let { type: localType, name: localName = "bar" } = node;

console.log(localType); // "Identifier"
console.log(localName); // "bar"
```

Here, the `localName` variable has a default value of `"bar"`.

- The variable is assigned its default value because no `node.name` property exists.

So far, you’ve learned how to use object destructuring on an object whose properties are primitive values.

- You can also use object destructuring to retrieve values in nested object structures.

#### _Nested Object Destructuring_

By using syntax similar to that of object literals, you can navigate into a nested object structure to retrieve just the information you want.

- Here’s an example:

```js
let node = {
  type: "Identifier",
  name: "foo",
  loc: {
    start: {
      line: 1,
      column: 1
    },
    end: {
      line: 1,
      column: 4
    }
  }
};

let {
  loc: { start }
} = node;

console.log(start.line); // 1
console.log(start.column); // 1
```

The destructuring pattern in this example uses curly braces to indicate that the pattern should descend into the property named `loc` on `node` and look for the `start` property.

- Recall from the previous section that a colon in a destructuring pattern means the identifier before the colon is giving a location to inspect, and the right side assigns a value.
- A curly brace after the colon indicates that the destination is nested another level into the object.

You can go one step further and use a different name for the local variable as well:

```js
let node = {
  type: "Identifier",
  name: "foo",
  loc: {
    start: {
      line: 1,
      column: 1
    },
    end: {
      line: 1,
      column: 4
    }
  }
};

// extract node.loc.start
let {
  loc: { start: localStart }
} = node;

console.log(localStart.line); // 1
console.log(localStart.column); // 1
```

In this version of the code, `node.loc.start` is stored in a new local variable called `localStart`.

- Destructuring patterns can be nested to an arbitrary level of depth, and all capabilities will be available at each level.

Object destructuring is very powerful because it provides you with lots of options, but array destructuring offers some unique capabilities that allow you to extract information from arrays.

> **Syntax Gotcha**
>
> Be careful when you’re using nested destructuring because you can inadvertently create a statement that has no effect.
>
> - Empty curly braces are legal in object destructuring; however, they don’t do anything. For example:
>
> ```js
> // no variables declared!
> let {
>   loc: {}
> } = node;
> ```
>
> No bindings are declared in this statement.
>
> - Due to the curly braces on the right, `loc` is used as a location to inspect rather than a binding to create.
>
> - In such cases, it’s likely that the intent was to use `=` to define a default value rather than `:` to define a location.
>
> - It’s possible that this syntax will be made illegal in the future, but for now, this is a gotcha to look out for.

### Array Destructuring

Array destructuring syntax is very similar to object destructuring: it just uses array literal syntax instead of object literal syntax.

- The destructuring operates on positions within an array rather than the named properties that are available in objects.
  - For example:

```js
let colors = ["red", "green", "blue"];

let [firstColor, secondColor] = colors;

console.log(firstColor); // "red"
console.log(secondColor); // "green"
```

Here, array destructuring pulls out the values `"red"` and `"green"` from the `colors` array, and stores them in the `firstColor` and `secondColor` variables.

- Those values are chosen because of their position in the array; the actual variable names could be anything.
- Any items not explicitly mentioned in the destructuring pattern are ignored.
- Keep in mind that the array isn’t changed in any way.

You can also omit items in the destructuring pattern and only provide variable names for the items you’re interested in.

- If, for example, you just want the third value of an array, you don’t need to supply variable names for the first and second items.
  - Here’s how that works:

```js
let colors = ["red", "green", "blue"];

let [, , thirdColor] = colors;

console.log(thirdColor); // "blue"
```

This code uses a destructuring assignment to retrieve the third item in `colors`.

- The commas preceding `thirdColor` in the pattern are placeholders for the array items that come before it.
- By using this approach, you can easily pick out values from any number of slots in the middle of an array without needing to provide variable names for them.

> **Note**: Similar to object destructuring, you must always provide an initializer when using array destructuring with `var`, `let`, or `const`.

#### _Destructuring Assignment_

You can use array destructuring in the context of an assignment, but unlike object destructuring, there is no need to wrap the expression in parentheses.

- Consider the following example:

```js
let colors = ["red", "green", "blue"],
  firstColor = "black",
  secondColor = "purple";

[firstColor, secondColor] = colors;

console.log(firstColor); // "red"
console.log(secondColor); // "green"
```

The destructured assignment in this code works in a similar manner to the previous array destructuring example.

- The only difference is that `firstColor` and `secondColor` have already been defined.

Most of the time, what you know now about array destructuring assignment is all you’ll need to know, but there’s a bit more to it that you’ll probably find useful.

Array destructuring assignment has a very unique use case that makes it easier to swap the values of two variables.

- Value swapping is a common operation in sorting algorithms, and the ECMAScript 5 way of swapping variables involves a third, temporary variable, as in this example:

```js
// swapping variables in ECMAScript 5
let a = 1,
  b = 2,
  tmp;

tmp = a;
a = b;
b = tmp;

console.log(a); // 2
console.log(b); // 1
```

The intermediate variable `tmp` is necessary to swap the values of `a` and `b`.

- However, using array destructuring assignment, there’s no need for that extra variable.
- Here’s how you can swap variables in ECMAScript 6:

```js
// swapping variables in ECMAScript 6
let a = 1,
  b = 2;

[a, b] = [b, a];

console.log(a); // 2
console.log(b); // 1
```

The array destructuring assignment in this example looks like a mirror image.

- The left side of the assignment (before the equal sign) is a destructuring pattern just like those in the other array destructuring examples.
- The right side is an array literal that is temporarily created for the swap.
- The destructuring happens on the temporary array, which has the values of b and a copied into its first and second positions.
  - The effect is that the variables have swapped values.

> **Note**: Like object destructuring assignment, an error is thrown when the right side of an array destructured assignment expression evaluates to `null` or `undefined`.

#### _Default Values_

Array destructuring assignment allows you to specify a default value for any position in the array, too.

- The default value is used when the property at the given position either doesn’t exist or has the value `undefined`.
- For example:

```js
let colors = ["red"];

let [firstColor, secondColor = "green"] = colors;

console.log(firstColor); // "red"
console.log(secondColor); // "green"
```

In this code, the `colors` array has only one item, so there is nothing for `secondColor` to match.

- Because there is a default value, `secondColor` is set to "green" instead of `undefined`.

#### _Nested Array Destructuring_

You can destructure nested arrays in a manner similar to destructuring nested objects.

- By inserting another array pattern into the overall pattern, the destructuring will descend into a nested array, like this:

```js
let colors = ["red", ["green", "lightgreen"], "blue"];

// later

let [firstColor, [secondColor]] = colors;

console.log(firstColor); // "red"
console.log(secondColor); // "green"
```

Here, the `secondColor` variable refers to the `"green"` value inside the colors array.

- That item is contained within a second array, so the extra square brackets around `secondColor` in the destructuring pattern are necessary.

As with objects, you can nest arrays arbitrarily deep.

#### _Rest Items_

Chapter 3 introduced rest parameters for functions, and array destructuring has a similar concept called _rest items_.

- Rest items use the `...` syntax to assign the remaining items in an array to a particular variable.
  - Take a look at the following example:

```js
let colors = ["red", "green", "blue"];

let [firstColor, ...restColors] = colors;

console.log(firstColor); // "red"
console.log(restColors.length); // 2
console.log(restColors[0]); // "green"
console.log(restColors[1]); // "blue"
```

The first item in `colors` is assigned to `firstColor`, and the rest are assigned into a new `restColors` array.

- Therefore, the `restColors` array has two items: `"green"` and `"blue"`.

Rest items are useful for extracting certain items from an array and keeping the rest available, but there’s another helpful use.

A glaring omission from JavaScript arrays is the ability to easily create a clone.

- In ECMAScript 5, developers frequently used the `concat()` method as an easy way to clone an array.
  - For example:

```js
// cloning an array in ECMAScript 5
var colors = ["red", "green", "blue"];
var clonedColors = colors.concat();

console.log(clonedColors); // "[red,green,blue]"
```

Although the `concat()` method is intended to concatenate two arrays, calling it without an argument returns a clone of the array.

- In ECMAScript 6, you can use rest items to achieve the same task through syntax intended to function that way.
  - It works like this:

```js
// cloning an array in ECMAScript 6
let colors = ["red", "green", "blue"];
let [...clonedColors] = colors;

console.log(clonedColors); // "[red,green,blue]"
```

In this example, rest items are used to copy values from the `colors` array into the `clonedColors` array.

- Although it’s a matter of perception as to whether this technique makes the developer’s intent clearer than using the `concat()` method, it’s still a useful approach to be aware of.

> **Note**: Rest items must be the last entry in the destructured array and cannot be followed by a comma.
>
> - Including a comma after rest items is a syntax error.
