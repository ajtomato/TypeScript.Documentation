# Documentation

## Tutorials

### TypeScript in 5 minutes

#### Installing TypeScript

    npm install -g typescript

#### Building your first TypeScript file

In your editor, type the following JavaScript code in *greeter.ts*:

    function greeter(person) {
        return "Hello, " + person;
    }

    let user = "Jane User";

    document.body.innerHTML = greeter(user);

#### Compiling your code

    tsc greeter.ts

The result will be a file *greeter.js*.

#### Type annotations

Type annotations in TypeScript are lightweight ways to record the intended contract of the function or variable.

Notice that although there were errors, the js file is still created.

#### Interfaces

In TypeScript, two types are compatible if their internal structure is compatible. This allows us to implement an interface just by having the shape the interface requires, without an explicit *implements* clause.

#### Classes

Notice that classes and interfaces play well together, letting the programmer decide on the right level of abstraction.

Also of note, the use of *public* on arguments to the constructor is a shorthand that allows us to automatically create properties with that name.

Classes in TypeScript are just a shorthand for the same prototype-based OO that is frequently used in JavaScript.

#### Running your TypeScript web app

## Handbook

### Basic Types

#### boolean

true/false

#### number

All numbers in TypeScript are floating point values.

#### string

TypeScript uses double quotes (") or single quotes (') to surround string data.

You can also use *template strings*, which can span multiple lines and have embedded expressions. These strings are surrounded by the backtick/backquote (`) character, and embedded expressions are of the form ${ expr }.

    let fullName: string = `Bob Bobbington`;
    let age: number = 37;
    // The new line is also included into this variable
    let sentence: string = `Hello, my name is ${ fullName }.
            I'll be ${ age + 1 } years old next month.`;

#### Array

Array types can be written in one of two ways.

* In the first, you use the type of the elements followed by [] to denote an array of that element type: `let list: number[] = [1, 2, 3];`
* The second way uses a generic array type, Array<elemType>: `let list: Array<number> = [1, 2, 3];`

#### Tuple

Tuple types allow you to express an array where the type of a fixed number of elements is known, but need not be the same.

    // Declare a tuple type
    let x: [string, number];
    // Initialize it
    x = ["hello", 10]; // OK
    // Initialize it incorrectly
    x = [10, "hello"]; // Error

When accessing an element with a known index, the correct type is retrieved:

    console.log(x[0].substr(1)); // OK
    console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'

When accessing an element outside the set of known indices, a *union type* is used instead:

    x[3] = "world";                 // OK, 'string' can be assigned to 'string | number'
    console.log(x[5].toString());   // OK, 'string' and 'number' both have 'toString'
    x[6] = true;                    // Error, 'boolean' isn't 'string | number'

#### enum

    enum Color {Red, Green, Blue}
    let c: Color = Color.Green;

A handy feature of enums is that you can also go from a numeric value to the name of that value in the enum. For example, if we had the value 2 but weren’t sure what that mapped to in the Color enum above, we could look up the corresponding name:

    enum Color {Red = 1, Green, Blue}
    let colorName: string = Color[2];
    alert(colorName); // Displays 'Green' as it's value is 2 above

#### any

We may need to describe the type of variables that we do not know when we are writing an application.

    let notSure: any = 4;
    notSure = "maybe a string instead";
    notSure = false; // okay, definitely a boolean

 Variables of type *Object* only allow you to assign any value to them - you can’t call arbitrary methods on them, even ones that actually exist.

    let notSure: any = 4;
    notSure.ifItExists(); // okay, ifItExists might exist at runtime
    notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

    let prettySure: Object = 4;
    prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.

#### void

You may commonly see *void* as the return type of functions that do not return a value.

Declaring variables of type *void* is not useful because you can only assign undefined or null to them.

#### null and undefined

In TypeScript, both *undefined* and *null* actually have their own types named *undefined* and *null* respectively.

However, when using the *--strictNullChecks* flag, *null* and *undefined* are only assignable to *void* and their respective types. This helps avoid many common errors. In cases where you want to pass in either a *string* or *null* or *undefined*, you can use the union type *string | null | undefined*.

#### never

The *never* type represents the type of values that never occur. For instance, *never* is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns; Variables also acquire the type *never* when narrowed by any type guards that can never be true.

    // Function returning never must have unreachable end point
    function error(message: string): never {
        throw new Error(message);
    }
    // Inferred return type is never
    function fail() {
        return error("Something failed");
    }
    // Function returning never must have unreachable end point
    function infiniteLoop(): never {
        while (true) {
        }
    }

#### Type assertions

*Type assertions* are a way to tell the compiler “trust me, I know what I’m doing.” A *type assertion* is like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler.

Type assertions have two forms.

* `let someValue: any = "this is a string"; let strLength: number = (<string>someValue).length;`
* `let someValue: any = "this is a string"; let strLength: number = (someValue as string).length;`

When using TypeScript with *JSX*, only as-style assertions are allowed.

#### A note about let

You should use *let* instead of *var* whenever possible.

### Variable Declarations

### Interfaces

### Classes

### Functions

### Generics

### Enums

### Type Inference

### Type Compatibility

### Advanced Types

### Symbols

### Iterators and Generators

### Modules

### Namespaces

### Namespaces and Modules

### Module Resolution

### Declaration Merging

### JSX

### Decorators

### Mixins

### Triple-Slash Directives

### Type Checking JavaScript Files
