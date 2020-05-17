# You Don't Know JS Yet: Get Started - 2nd Edition
# Chapter 2: Surveying JS

## Each File is a Program

In JS, each standalone file is its own separate program.

The reason this matters is primarily around error handling. 

- one file may fail (during parse/compile or execution) and that will not necessarily prevent the next file from being processed. 
-It's important to ensure that each file works properly, and that to whatever extent possible, they handle failure in other files as gracefully as possible.

| NOTE: |
| :--- |
| Many projects use build process tools that end up combining separate files from the project into a single file to be delivered to a web page. When this happens, JS treats this single combined file as the entire program. |

The only way multiple standalone .js files act as a single program is by sharing their state (and access to their public functionality) via the "global scope." They mix together in this global scope namespace, so at runtime they act as as whole.

Since ES6, JS has also supported a module format in addition to the typical standalone JS program format. Modules are also file-based. If a file is loaded via module-loading mechanism such as an `import` statement or a `<script type=module>` tag, all its code is treated as a single module.

Though you wouldn't typically think about a module—a collection of state and publicly exposed methods to operate on that state—as a standalone program, JS does in fact still treat each module separately. Similar to how "global scope" allows standalone files to mix together at runtime, importing a module into another allows runtime interoperation between them.

Regardless of which code organization pattern (and loading mechanism) is used for a file (standalone or module), you should still think of each file as its own (mini) program, which may then cooperate with other (mini) programs to perform the functions of your overall application.

## Values

Values come in two forms in JS: **primitive** and **object**.

primitive values 

strings, numbers, and booleans, `null` and `undefined`, symbol (used internally )

### Arrays And Objects

Besides primitives, the other value type in JS is an object value.

Arrays are a special type of object that's comprised of an ordered and numerically indexed list of data:

JS arrays can hold any value type, either primitive or object (including other arrays). 

| NOTE: |
| :--- |
| Functions, like arrays, are a special kind (aka, sub-type) of object. We'll cover functions in more detail in a bit. |

Objects are more general: an unordered, keyed collection of any various values. 
In other words, you access the element by a string location name (aka "key" or "property") rather than by its numeric position (as with arrays).

### Value Type Determination

For distinguishing values, the `typeof` operator tells you its built-in type, if primitive, or `"object"` otherwise:

```js
typeof 42;                  // "number"
typeof "abc";               // "string"
typeof true;                // "boolean"
typeof undefined;           // "undefined"
typeof null;                // "object" -- oops, bug!
typeof { "a": 1 };          // "object"
typeof [1,2,3];             // "object"
typeof function hello(){};  // "function"
```

| WARNING: |
| :--- |
| `typeof null` unfortunately returns `"object"` instead of the expected `"null"`. Also, `typeof` returns the specific `"function"` for functions, but not the expected `"array"` for arrays. |

Primitive values and object values behave differently when they're assigned or passed around. We'll cover these details in Appendix A, "Values vs References."

## Declaring and Using Variables

- Variables have to be declared.
- There are various syntax forms that declare variables (aka, "identifiers"), and each form has different implied behaviors.

to declare identifiers use: 

var, let, const

| NOTE: |
| :--- |
| It's very common to suggest that `var` should be avoided in favor of `let` (or `const`!), generally because of perceived confusion over how the scoping behavior of `var` has worked since the beginning of JS. I believe this to be overly restrictive advice and ultimately unhelpful. It's assuming you are unable to learn and use a feature properly in combination with other features. I believe you *can* and *should* learn any features available, and use them where appropriate! |

const:
1. It must be given a value at the moment it's declared.
2. and cannot be re-assigned a different value later.
3. can be modified though if is object like x.name = "new value", so don't use it with objects, use it with primitives only.

Besides `var` / `let` / `const`, there are other syntactic forms that declare identifiers (variables) in various scopes. For example:

```js
function hello(myName) {
    console.log(`Hello, ${ myName }.`);
}

hello("Kyle");
```

The identifier `hello` is created in the outer scope, 
and it's also automatically associated so that it references the function. 
`hello` and `myName` generally behave as `var`-declared.

Another syntax that declares a variable is a `catch` clause:

```js
try {
    someError();
}
catch (err) {
    console.log(err);
}
```

The `err` is a block-scoped variable that exists only inside the `catch` clause, as if it had been declared with `let`.

## Functions

From the early days of JS, function definition looked like:

- function declaration 
```js
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}
```

This is called a function declaration because it appears as a statement by itself, not as an expression in another statement. 
The association between the identifier `awesomeFunction` and the function value happens during the compile phase of the code, 
before that code is executed.

- function declaration statement, a function expression can be defined and assigned like this:

```js
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

This function is an expression that is assigned to the variable `awesomeFunction`. 
- a function expression is not associated with its identifier until that statement during runtime.

It's extremely important to note that in JS, 

functions are

1. values that can be assigned and passed around. (unlike many programming languages)
2. special type of the object value type like array.
ie: js value types are (primitives and object (array, function)). 

Since functions are values, they can be assigned as properties on objects:

```js
var whatToSay = {
    greeting() {
        console.log("Hello!");
    },
    question() {
        console.log("What's your name?");
    },
    answer() {
        console.log("My name is Kyle.");
    }
};

whatToSay.greeting();
// Hello!
```

There are many varied forms that `function`s take in JS. 
We dig into these variations in Appendix A, "So Many Function Forms."

expression function vs statement function in js

An expression 
- produces a value 
- can be written wherever a value is expected.
eg:
an argument in a function call

A statement 
- performs an action

eg: Loops and if

Wherever JavaScript expects a statement, you can also write an expression but not vice versa

eg: an if statement cannot become the argument of a function.

var x = (y >= 0 ? y : -y); //stmt has exprn;

function expn can be anonymous or named

function expn: evaluate then forget about it.
and that's its value, you do not pollute global namespace.
you can not call it anywhere in the code (in case of named fun expn).

## Comparisons

### Equal...ish

**equality** comparison and an **equivalence** comparison are not the same.

Not *exact*ly.

Yes, most values participating in an `===` equality comparison will fit with that *exact same* intuition. Consider some examples:

```js
3 === 3.0;              // true
"yes" === "yes";        // true
null === null;          // true
false === false;        // true

42 === "42";            // false
"hello" === "Hello";    // false
true === 1;             // false
0 === null;             // false
"" === null;            // false
null === undefined;     // false
```

But strange behaviour of `===` operator.

```js
NaN === NaN;            // false
0 === -0;               // true
```

Since the *lying* about such comparisons can be bothersome, it's best to avoid using `===` for them.
For `NaN` comparisons, use the `Number.isNaN(..)` utility, which does not *lie*.
For `-0` comparison, use the `Object.is(..)` utility, which also does not *lie*. 
`Object.is(..)` can also be used for non-*lying* `NaN` checks, if you prefer. 
Humorously, you could think of `Object.is(..)` as the "quadruple-equals" `====`, the really-really-strict comparison!

There are deeper historical and technical reasons for these *lies*, 
but that doesn't change the fact that `===` is not actually *strictly exactly equal* comparison, in the *strictest* sense.

The story gets even more complicated when we consider comparisons of object values (non-primitives). Consider:

```js
[ 1, 2, 3 ] === [ 1, 2, 3 ];    // false
{ a: 42 } === { a: 42 }         // false
(x => x * 2) === (x => x * 2)   // false
```

What's going on here?

It may seem reasonable to assume that an equality check considers the *nature* or *contents* of the value. 
But when it comes to objects, a content-aware comparison is generally referred to as "structural equality."

JS does not define `===` as *structural equality* for object values. 
Instead, `===`         uses *identity equality* for object values.

In JS, all object values are 
1. held by reference.
2. assigned and passed by reference-copy, 

to our current discussion, are compared by reference (identity) equality. 

Consider:

```js
var x = [ 1, 2, 3 ];

// assignment is by reference-copy, so
// y references the *same* array as x,
// not another copy of it.
var y = x;

y === x;              // true
y === [ 1, 2, 3 ];    // false
x === [ 1, 2, 3 ];    // false
```

In this snippet, `y === x` is true because both variables hold a reference to the same initial array. But the `=== [1,2,3]` comparisons both fail because `y` and `x`, respectively, are being compared to new *different* arrays `[1,2,3]`. The array structure and contents don't matter in this comparison, only the **reference identity**.

JS does not provide a mechanism for structural equality comparison of object values, only reference identity comparison. To do structural equality comparison, you'll need to implement the checks yourself.

But beware, it's more complicated than you'll assume. For example, how might you determine if two function references are "structurally equivalent"? Even stringifying to compare their source code text wouldn't take into account things like closure. JS doesn't provide structural equality comparison because it's almost intractable to handle all the corner cases!

### Coercive Comparisons

- The `==` operator, AKA the "loose equality" operator. The creator of the language himself, has lamented how it was designed as a big mistake.

- Most of this frustration comes from a pretty short list of confusing corner cases, but a deeper problem is the extremely widespread misconception that it performs its comparisons without considering the types of its compared values.

Consider:

```js
42 == "42";             // true
1 == true;              // true
```

Just being aware of this nature of `==`—that it prefers primitive numeric comparisons—helps you avoid most of the troublesome corner cases, such as staying away from a gotchas like `"" == 0` or `0 == false`.

You may be thinking, "Oh, well, I will just always avoid any coercive equality comparison (using `===` instead) to avoid those corner cases"! Eh, sorry, that's not quite as likely as you would hope.

There's a pretty good chance that you'll use relational comparison operators like `<`, `>` (and even `<=` and `>=`).

Just like `==`, these operators will perform as if they're "strict" if the types being relationally compared already match, but they'll allow coercion first (generally, to numbers) if the types differ.

Consider:

```js
var arr = [ "1", "10", "100", "1000" ];
for (let i = 0; i < arr.length && arr[i] < 500; i++) {
    // will run 3 times
}
```

These relational operators typically use numeric comparisons, except in the case where **both** values being compared are already strings; in this case, they use alphabetical (dictionary-like) comparison of the strings:

```js
var x = "10";
var y = "9";

x < y;      // true, watch out!
```

There's no way to get these relational operators to avoid coercion, other than to just never use mismatched types in the comparisons. That's perhaps admirable as a goal, but it's still pretty likely you're going to run into a case where the types *may* differ.

The wiser approach is not to avoid coercive comparisons, but to embrace and learn their ins and outs.

Coercive comparisons crop up in other places in JS, such as conditionals (`if`, etc.), which we'll revisit in Appendix A, "Coercive Conditional Comparison."

## How We Organize in JS

Two major patterns for organizing code (data and behavior) are used broadly across the JS ecosystem: classes and modules. 
These patterns are not mutually exclusive; many programs can and do use both. 
Other programs will stick with just one pattern, or even neither!

In some respects, these patterns are very different. But interestingly, in other ways, they're just different sides of the same coin. 
Being proficient in JS requires understanding both patterns and where they are appropriate (and not!).

### Classes

#### Class Inheritance

Another aspect inherent to traditional "class-oriented" design, though a bit less commonly used in JS, is "inheritance" (and "polymorphism"). Consider:
### Modules

The module pattern has essentially the same goal as the class pattern, which is to group data and behavior together into logical units. 
Also like classes, modules can "include" or "access" the data and behaviors of other modules, for cooperation sake.

But modules have some important differences from classes. Most notably, the syntax is entirely different.

#### Classic Modules

ES6 added a module syntax form to native JS syntax. 
But from the early days of JS, modules was an important and common pattern that was leveraged in countless JS programs, 
even without a dedicated syntax.

The key hallmarks of a *classic module* are an outer function (that runs at least once), 
which returns an "instance" of the module with one or more functions exposed that can operate on the module instance's internal (hidden) data.

Because a module of this form is *just a function*, and calling it produces an "instance" of the module, 
another description for these functions is "module factories".

Consider the classic module form of the earlier `Publication`, `Book`, and `BlogPost` classes:

```js
function Publication(title,author,pubDate) {
    var publicAPI = {
        print() {
            console.log(`
                Title: ${ title }
                By: ${ author }
                ${ pubDate }
            `);
        }
    };

    return publicAPI;
}

function Book(bookDetails) {
    var pub = Publication(
        bookDetails.title,
        bookDetails.author,
        bookDetails.publishedOn
    );

    var publicAPI = {
        print() {
            pub.print();
            console.log(`
                Publisher: ${ bookDetails.publisher }
                ISBN: ${ bookDetails.ISBN }
            `);
        }
    };

    return publicAPI;
}

function BlogPost(title,author,pubDate,URL) {
    var pub = Publication(title,author,pubDate);

    var publicAPI = {
        print() {
            pub.print();
            console.log(URL);
        }
    };

    return publicAPI;
}
```

Comparing these forms to the `class` forms, there are more similarities than differences.

The `class` form stores methods and data on an object instance, which must be accessed with the `this.` prefix. 
With modules, the methods and data are accessed as _identifier variables_ in scope, without any `this.` prefix.

With `class`, the "API" of an instance is implicit in the class definition—also, all data and methods are public. 
With the module factory function, you explicitly create and return an object with any publicly exposed methods, 
and any data or other unreferenced methods remain private inside the factory function.

There are other variations to this factory function form that are quite common across JS, 
even in 2020; you may run across these forms in different JS programs: 
AMD (Asynchronous Module Definition), 
UMD (Universal Module Definition), and 
CommonJS (classic Node.js-style modules). 
The variations are minor (not quite compatible). However, all of these forms rely on the same basic principles.

Consider also the usage (aka, "instantiation") of these module factory functions:

```js
var YDKJS = Book({
    title: "You Don't Know JS",
    author: "Kyle Simpson",
    publishedOn: "June 2014",
    publisher: "O'Reilly",
    ISBN: "123456-789"
});

YDKJS.print();
// Title: You Don't Know JS
// By: Kyle Simpson
// June 2014
// Publisher: O'Reilly
// ISBN: 123456-789

var forAgainstLet = BlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

The only observable difference here is the lack of using `new`, calling the module factories as normal functions.

#### ES Modules

ES modules (ESM), introduced to the JS language in ES6, are meant to serve much the same spirit and purpose as the existing *classic modules* just described, especially taking into account important variations and use cases from AMD, UMD, and CommonJS.

The implementation approach does, however, differ significantly.

1. There's no wrapping function to *define* a module. 
The wrapping context is a file. 
ESMs are always file-based; one file, one module.

2. You don't interact with a module's "API" explicitly, 
but rather use the `export` keyword to add a variable or method to its public API definition.

3. You don't "instantiate" an ES module, you just `import` it to use its single instance. 
ESMs are, in effect, "singletons," in that there's only one instance ever created, 
at first `import` in your program, and all other `import`s just receive a reference to that same single instance. 
If your module needs to support multiple instantiations, you have to provide a *classic module-style* factory function on your ESM definition for that purpose.

In our running example, we do assume multiple-instantiation, so these following snippets will mix both ESM and *classic modules*.

Consider the file `publication.js`:

```js
function printDetails(title,author,pubDate) {
    console.log(`
        Title: ${ title }
        By: ${ author }
        ${ pubDate }
    `);
}

export function create(title,author,pubDate) {
    var publicAPI = {
        print() {
            printDetails(title,author,pubDate);
        }
    };

    return publicAPI;
}
```

To import and use this module, from another ES module like `blogpost.js`:

```js
import { create as createPub } from "publication.js";

function printDetails(pub,URL) {
    pub.print();
    console.log(URL);
}

export function create(title,author,pubDate,URL) {
    var pub = createPub(title,author,pubDate);

    var publicAPI = {
        print() {
            printDetails(pub,URL);
        }
    };

    return publicAPI;
}
```

And finally, to use this module, we import into another ES module like `main.js`:

```js
import { create as newBlogPost } from "blogpost.js";

var forAgainstLet = newBlogPost(
    "For and against let",
    "Kyle Simpson",
    "October 27, 2014",
    "https://davidwalsh.name/for-and-against-let"
);

forAgainstLet.print();
// Title: For and against let
// By: Kyle Simpson
// October 27, 2014
// https://davidwalsh.name/for-and-against-let
```

| NOTE: |
| :--- |
| The `as newBlogPost` clause in the `import` statement is optional; if omitted, a top-level function just named `create(..)` would be imported. In this case, I'm renaming it for readability sake; its more generic factory name of `create(..)` becomes more semantically descriptive of its purpose as `newBlogPost(..)`. |

As shown, ES modules can use *classic modules* internally if they need to support multiple-instantiation. Alternatively, we could have exposed a `class` from our module instead of a `create(..)` factory function, with generally the same outcome. However, since you're already using ESM at that point, I'd recommend sticking with *classic modules* instead of `class`.

If your module only needs a single instance, you can skip the extra layers of complexity: `export` its public methods directly.
