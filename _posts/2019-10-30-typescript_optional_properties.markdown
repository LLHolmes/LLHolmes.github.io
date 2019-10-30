---
layout: post
title:      "Typescript Optional Properties"
date:       2019-10-30 16:34:39 +0000
permalink:  typescript_optional_properties
---

According to [TypeScript](http://www.typescriptlang.org/), it is a “typed superset of JavaScript that compiles to plain JavaScript.”  So what does that mean and why on Earth does it matter?

### The Basics
JavaScript is a [scripting language](https://en.wikipedia.org/wiki/Scripting_language).  It’s interpreted by the browser vs. being run on a machine like a programming language (for example C or Java).  Before running code in a programming language, it needs to be compiled.  Compiling processes the code into machine language allowing for greater efficiency and a host of other benefits.  

### Type Checking
One of the simple benefits of compiling is type checking - the notion of making sure a variable is assigned to the data type that is expected (an integer instead of a string), or that an argument passed into a function is as anticipated (an array of strings instead of a single object or an integer).  It's a way to make sure you're receiving the information you expect so that your program doesn't hit an error while trying to, say, do math on an object, or console.log the name property of an integer, or some other brand of nonsense if the wrong type of argument is passed into a function.

Since the browser is interpreting JavaScript however, it can’t be compiled.  But wouldn't it be great if there was a way of doing this double check?  This is where TypeScript comes in.  It's a way of making sure you and all the other developers on your team are on the same page and following the same rules.

Typescript is a superset in that it can be added on top of JavaScript in a program but isn’t required.  Check out [TypeScript in 5 minutes](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) to get an idea of how to get started.

### TypeScript Interfaces
With TypeScript, it’s simple enough to ensure the type of an argument by saying:
```
const double = (x: int) => x * 2
```

But what about type checking objects?

That’s where *interfaces* come in.  You need to set a specific object type:
```
interface Person {
    firstName: string;
    lastName: string;
}
```

But what about optional properties?  Those that sometimes exist, but not always…the inevitable, optional middle initial?  If you add it to the interface but it’s not always included, you’ll receive a type error whenever it’s not there:
```
interface Person {
    firstName: string;
    middleInitial: string;
    lastName: string;
}

let santa: Person = {firstName: "Santa", lastName: "Clause"};
console.log(santa);


=>  without-optional-properties.ts(7,5): error TS2322: Type '{ firstName: string; lastName: string; }' is not assignable to type 'Person'.
  Property 'middleInitial' is missing in type '{ firstName: string; lastName: string; }'.
```

So TypeScript’s quick and easy solution is to add a ? to the end of the property name declaration:
```
interface Person {
    firstName: string;
    middleInitial?: string;
    lastName: string;
}
```

That way, you can include the `middleInitial` or not.

It still allows for the more strict nature of type checking, with just a little flexibility.
