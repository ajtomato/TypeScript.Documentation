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
