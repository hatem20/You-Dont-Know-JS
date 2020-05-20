# You Don't Know JS Yet: Get Started - 2nd Edition
# Chapter 4: The Bigger Picture

JS has 3 main pillars:

1. Scope and Closure
2. Prototypes
3. Types and Coercion

## Pillar 1: Scope and Closure

Scope: The organization of variables into units of scope (functions, blocks).

**Lexical scope**: Scopes nest inside each other, and for any given expression or statement, only variables at that level of scope nesting, or in higher/outer scopes, are accessible; variables from lower/inner scopes are hidden and inaccessible.

The scope unit boundaries, and how variables are organized in them, is determined at the time the program is parsed (compiled). 

JS is lexically scoped, though many claim it isn't, because of two particular characteristics of its model that are not present in other lexically scoped languages.

1. is commonly called *hoisting*: when all variables declared anywhere in a scope are treated as if they're declared at the beginning of the scope.
2. The other is that `var`-declared variables are function scoped, even if they appear inside a block.

Neither hoisting nor function-scoped `var` are sufficient to back the claim that JS is not lexically scoped. 

`let`/`const` declarations have a peculiar error behavior called the "Temporal Dead Zone" (TDZ) which results in observable but unusable variables.
Though TDZ can be strange to encounter, it's *also* not an invalidation of lexical scoping. 
All of these are just unique parts of the language that should be learned and understood by all JS developers.

Closure is a natural result of lexical scope when the language has functions as first-class values, as JS does. 
When a function makes reference to variables from an outer scope, and that function is passed around as a value and executed in other scopes, it maintains access to its original scope variables; this is closure.

Across all programming, but especially in JS, closure drives many of the most important programming patterns, including modules. As I see it, modules are as *with the grain* as you can get, when it comes to code organization in JS.

## Pillar 2: Prototypes

The second pillar of the language is the prototypes system. We covered this topic in-depth in Chapter 3 ("Prototypes"), but I just want to make a few more comments about its importance.

JS is one of very few languages where you have the option to create objects directly and explicitly, without first defining their structure in a class.

For many years, people implemented the class design pattern on top of prototypes—so-called "prototypal inheritance" (see Appendix A, "Prototypal 'Classes'")—and then with the advent of ES6's `class` keyword, the language doubled-down on its inclination toward OO/class-style programming.

But I think that focus has obscured the beauty and power of the prototype system: 
the ability for two objects to simply connect with each other and cooperate dynamically 
(during function/method execution) through sharing a `this` context.

Classes are just one pattern you can build on top of such power. 
But another approach, in a very different direction, is to simply embrace objects as objects, 
forget classes altogether, and let objects cooperate through the prototype chain. 
This is called *behavior delegation*. 
I think delegation is more powerful than class inheritance, as a means for organizing behavior and data in our programs.

But class inheritance gets almost all the attention. 
And the rest goes to functional programming (FP), as the sort of "anti-class" way of designing programs.
This saddens me, because it snuffs out any chance for exploration of delegation as a viable alternative.

I encourage you to spend plenty of time deep in Book 3, *Objects & Classes*, to see how object delegation holds far more potential than we've perhaps realized. This isn't an anti-`class` message, but it is intentionally a "classes aren't the only way to use objects" message that I want more JS developers to consider.

Object delegation is, I would argue, far more *with the grain* of JS, than classes (more on *grains* in a bit).

## Pillar 3: Types and Coercion

JS is good in types if you understand it well, you may not need TypesScript/Flow.
## With the Grain

- If you ever want to break out from the crowd, you're going to have to break from how the crowd does it!
- I also dose out quite a bit of my opinions on how you can interpret and use JS to the best benefit in your programs. 
- Don't try to make js to be like any other lang. just embrace JS.

## In Order

1. Get started with a solid foundation of JS from *Get Started* (Book 1) -- good news, you've already almost finished this book!

2. In *Scope & Closures* (Book 2), dig into the first pillar of JS: lexical scope, how that supports closure, and how the module pattern organizes code.

3. In *Objects & Classes* (Book 3), focus on the second pillar of JS: how JS's `this` works, how object prototypes support delegation, and how prototypes enable the `class` mechanism for OO-style code organization.

4. In *Types & Grammar* (Book 4), tackle the third and final pillar of JS: types and type coercion, as well as how JS's syntax and grammar define how we write our code.

5. With the **three pillars** solidly in place, *Sync & Async* (Book 5) then explores how we use flow control to model state change in our programs, both synchronously (right away) and asynchronously (over time).

6. The series concludes with *ES.Next & Beyond* (Book 6), a forward look at the near- and mid-term future of JS, including a variety of features likely coming to your JS programs before too long.

That's the intended order to read this book series.

However, Books 2, 3, and 4 can generally be read in any order, depending on which topic you feel most curious about and comfortable exploring first. But I don't recommend you skip any of these three books—not even *Types & Grammar*, as some of you will be tempted to do!—even if you think you already have that topic down.

Book 5 (*Sync & Async*) is crucial for deeply understanding JS, but if you start digging in and find it's too intimidating, this book can be deferred until you're more experienced with the language. The more JS you've written (and struggled with!), the more you'll come to appreciate this book. So don't be afraid to come back to it at a later time.

The final book in the series, *ES.Next & Beyond*, in some respects stands alone. It can be read at the end, as I suggest, or right after *Getting Started* if you're looking for a shortcut to broaden your radar of what JS is all about. This book will also be more likely to receive updates in the future, so you'll probably want to re-visit it occasionally.

However you choose to proceed with YDKJSY, check out the appendices of this book first, especially practicing the snippets in Appendix B, "Practice, Practice, Practice!" Did I mention you should go practice!? There's no better way to learn code than to write it.
