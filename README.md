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

#### var declarations

*var* declarations are accessible anywhere within their containing function, module, namespace, or global scope, regardless of the containing block.

#### let declarations

When a variable is declared using *let*, it uses what some call *lexical-scoping* or *block-scoping*.

Something to note is that you can still capture a *block-scoped* variable before it's declared. The only catch is that it's illegal to call that function before the declaration.

    function foo() {
        // okay to capture 'a'
        return a;
    }

    // illegal call 'foo' before 'a' is declared
    // runtimes should throw an error here
    foo();

    let a;

Each time a scope is run, it creates an "environment" of variables. That environment and its captured variables can exist even after everything within its scope has finished executing.

#### const declarations

They are like *let* declarations but, as their name implies, their value cannot be changed once they are bound. In other words, they have the same scoping rules as *let*, but you can't re-assign to them.

This should not be confused with the idea that the values they refer to are *immutable*.

    const numLivesForCat = 9;
    const kitty = {
        name: "Aurora",
        numLives: numLivesForCat,
    }

    // Error
    kitty = {
        name: "Danielle",
        numLives: numLivesForCat
    };

    // all "okay"
    kitty.name = "Rory";
    kitty.name = "Kitty";
    kitty.name = "Cat";
    kitty.numLives--;

Unless you take specific measures to avoid it, the internal state of a *const* variable is still modifiable.

#### let vs. const

All declarations other than those you plan to modify should use *const*.

#### Destructuring

The simplest form of destructuring is array destructuring assignment:

    let input = [1, 2];
    let [first, second] = input;
    console.log(first);     // outputs 1
    console.log(second);    // outputs 2

Destructuring works with already-declared variables as well:

    // swap variables
    [first, second] = [second, first];

And with parameters to a function:

    function f([first, second]: [number, number]) {
        console.log(first);
        console.log(second);
    }
    f([1, 2]);

You can create a variable for the remaining items in a list using the syntax ...:

    let [first, ...rest] = [1, 2, 3, 4];
    console.log(first); // outputs 1
    console.log(rest);  // outputs [ 2, 3, 4 ]

Of course, since this is JavaScript, you can just ignore trailing elements you don't care about:

    let [first] = [1, 2, 3, 4];
    console.log(first); // outputs 1

Or other elements:

    let [, second, , fourth] = [1, 2, 3, 4];

You can also destructure objects:

    let o = {
        a: "foo",
        b: 12,
        c: "bar"
    };
    let { a, b } = o;

This creates new variables *a* and *b* from *o.a* and *o.b*. Notice that you can skip *c* if you don't need it.

Like array destructuring, you can have assignment without declaration:

    ({ a, b } = { a: "baz", b: 101 });

Notice that we had to surround this statement with parentheses. JavaScript normally parses a `{` as the start of block.

You can create a variable for the remaining items in an object using the syntax ...:

    let { a, ...passthrough } = o;
    let total = passthrough.b + passthrough.c.length;

You can also give different names to properties:

    let { a: newName1, b: newName2 } = o;

You can read `a: newName1` as "`a` as `newName1`". The direction is left-to-right, as if you had written:

    let newName1 = o.a;
    let newName2 = o.b;

Confusingly, the colon here does **not** indicate the type.

Default values let you specify a default value in case a property is undefined:

    function keepWholeObject(wholeObject: { a: string, b?: number }) {
        let { a, b = 1001 } = wholeObject;
    }

*keepWholeObject* now has a variable for `wholeObject` as well as the properties `a` and `b`, even if `b` is undefined.

Destructuring also works in function declarations. For simple cases this is straightforward:

    type C = { a: string, b?: number }
    function f({ a, b }: C): void {
        // ...
    }

But specifying defaults is more common for parameters, and getting defaults right with destructuring can be tricky. First of all, you need to remember to put the pattern before the default value.

    function f({ a, b } = { a: "", b: 0 }): void {
        // ...
    }
    f(); // ok, default to { a: "", b: 0 }

Then, you need to remember to give a default for optional properties on the destructured property instead of the main initializer. Remember that `C` was defined with `b` optional:

    function f({ a, b = 0 } = { a: "" }): void {
        // ...
    }
    f({ a: "yes" }); // ok, default b = 0
    f(); // ok, default to { a: "" }, which then defaults b = 0
    f({}); // error, 'a' is required if you supply an argument

Use destructuring with care. Try to keep destructuring expressions small and simple. You can always write the assignments that destructuring would generate yourself.

The spread operator is the opposite of destructuring. It allows you to spread an array into another array, or an object into another object.

    let first = [1, 2];
    let second = [3, 4];
    let bothPlus = [0, ...first, ...second, 5];

This gives *bothPlus* the value `[0, 1, 2, 3, 4, 5]`.

You can also spread objects:

    let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
    let search = { ...defaults, food: "rich" };

Now `search` is `{ food: "rich", price: "$$", ambiance: "noisy" }`. Properties that come later in the spread object overwrite properties that come earlier.

Basically, that means you lose methods when you spread instances of an object:

    class C {
      p = 12;
      m() {
      }
    }
    let c = new C();
    let clone = { ...c };
    clone.p; // ok
    clone.m(); // error!

Second, the Typescript compiler doesn't allow spreads of type parameters from generic functions.

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
