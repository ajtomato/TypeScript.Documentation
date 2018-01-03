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

#### Our First Interface

One of TypeScript's core principles is that type-checking focuses on the *shape* that values have. This is sometimes called "duck typing" or "structural subtyping". In TypeScript, interfaces fill the role of naming these types, and are a powerful way of defining contracts within your code as well as contracts with code outside of your project.

    function printLabel(labelledObj: { label: string }) {
        console.log(labelledObj.label);
    }

    let myObj = {size: 10, label: "Size 10 Object"};
    printLabel(myObj);

The type-checker checks the call to *printLabel*. The *printLabel* function has a single parameter that requires that the object passed in has a property called *label* of type string. Notice that our object actually has more properties than this, but the compiler only checks that *at least* the ones required are present and match the types required.

    interface LabelledValue {
        label: string;
    }

    function printLabel(labelledObj: LabelledValue) {
        console.log(labelledObj.label);
    }

    let myObj = {size: 10, label: "Size 10 Object"};
    printLabel(myObj);

The interface *LabelledValue* is a name we can now use to describe the requirement in the previous example. It still represents having a single property called *label* that is of type string. Notice we didn't have to explicitly say that the object we pass to *printLabel* implements this interface like we might have to in other languages. Here, it's only the shape that matters. If the object we pass to the function meets the requirements listed, then it's allowed.

It's worth pointing out that the type-checker does not require that these properties come in any sort of order, only that the properties the interface requires are present and have the required type.

#### Optional Properties

Not all properties of an interface may be required.

These optional properties are popular when creating patterns like "option bags" where you pass an object to a function that only has a couple of properties filled in.

    interface SquareConfig {
        color?: string;
        width?: number;
    }

    function createSquare(config: SquareConfig): {color: string; area: number} {
        let newSquare = {color: "white", area: 100};
        if (config.color) {
            newSquare.color = config.color;
        }
        if (config.width) {
            newSquare.area = config.width * config.width;
        }
        return newSquare;
    }

    let mySquare = createSquare({color: "black"});

Interfaces with optional properties are written similar to other interfaces, with each optional property denoted by a ? at the end of the property name in the declaration.

#### Readonly properties

Some properties should only be modifiable when an object is first created. You can specify this by putting *readonly* before the name of the property:

    interface Point {
        readonly x: number;
        readonly y: number;
    }

You can construct a *Point* by assigning an object literal. After the assignment, *x* and *y* can't be changed.

    let p1: Point = { x: 10, y: 20 };
    p1.x = 5; // error!

TypeScript comes with a *ReadonlyArray<T>* type that is the same as *Array<T>* with all mutating methods removed, so you can make sure you don't change your arrays after creation:

    let a: number[] = [1, 2, 3, 4];
    let ro: ReadonlyArray<number> = a;
    ro[0] = 12;         // error!
    ro.push(5);         // error!
    ro.length = 100;    // error!
    a = ro;             // error!

On the last line of the snippet you can see that even assigning the entire *ReadonlyArray* back to a normal array is illegal. You can still override it with a type assertion, though:

    a = ro as number[];

The easiest way to remember whether to use *readonly* or *const* is to ask whether you're using it on a variable or a property. Variables use *const* whereas properties use *readonly*.

#### Excess Property Checks

If an object literal has any properties that the "target type" doesn't have, you'll get an error.

    // error: 'colour' not expected in type 'SquareConfig'
    let mySquare = createSquare({ colour: "red", width: 100 });

If you're sure that the object can have some extra properties that are used in some special way. If *SquareConfig*s can have *color* and *width* properties with the above types, but could *also* have any number of other properties, then we could define it like so:

    interface SquareConfig {
        color?: string;
        width?: number;
        [propName: string]: any;
    }

We'll discuss index signatures in a bit, but here we're saying a *SquareConfig* can have any number of properties, and as long as they aren't *color* or *width*, their types don't matter.

#### Function Types

In addition to describing an object with properties, interfaces are also capable of describing function types.

To describe a function type with an interface, we give the interface a call signature. This is like a function declaration with only the parameter list and return type given. Each parameter in the parameter list requires both name and type.

    interface SearchFunc {
        (source: string, subString: string): boolean;
    }

Once defined, we can use this function type interface like we would other interfaces. Here, we show how you can create a variable of a function type and assign it a function value of the same type.

    let mySearch: SearchFunc;
    mySearch = function(source: string, subString: string) {
        let result = source.search(subString);
        return result > -1;
    }

For function types to correctly type-check, the names of the parameters do not need to match.

Function parameters are checked one at a time, with the type in each corresponding parameter position checked against each other. If you do not want to specify types at all, TypeScript's contextual typing can infer the argument types since the function value is assigned directly to a variable of type *SearchFunc*. Here, also, the return type of our function expression is implied by the values it returns (here *false* and *true*). Had the function expression returned numbers or strings, the type-checker would have warned us that return type doesn't match the return type described in the *SearchFunc* interface.

    let mySearch: SearchFunc;
    mySearch = function(src, sub) {
        let result = src.search(sub);
        return result > -1;
    }

#### Indexable Types

Indexable types have an *index signature* that describes the types we can use to index into the object, along with the corresponding return types when indexing.

    interface StringArray {
        [index: number]: string;
    }

    let myArray: StringArray;
    myArray = ["Bob", "Fred"];

    let myStr: string = myArray[0];

There are two types of supported index signatures: string and number. It is possible to support both types of indexers, but the type returned from a numeric indexer must be a subtype of the type returned from the string indexer. This is because when indexing with a *number*, JavaScript will actually convert that to a *string* before indexing into an object. That means that indexing with 100 (a *number*) is the same thing as indexing with "100" (a *string*), so the two need to be consistent.

    class Animal {
        name: string;
    }
    class Dog extends Animal {
        breed: string;
    }

    // Error: indexing with a 'string' will sometimes get you an Animal!
    interface NotOkay {
        [x: number]: Animal;
        [x: string]: Dog;
    }

While string index signatures are a powerful way to describe the "dictionary" pattern, they also enforce that all properties match their return type. This is because a string index declares that *obj.property* is also available as *obj["property"]*.

    interface NumberDictionary {
        [index: string]: number;
        length: number;    // ok, length is a number
        name: string;      // error, the type of 'name' is not a subtype of the indexer
    }

Finally, you can make index signatures readonly in order to prevent assignment to their indices:

    interface ReadonlyStringArray {
        readonly [index: number]: string;
    }
    let myArray: ReadonlyStringArray = ["Alice", "Bob"];
    myArray[2] = "Mallory"; // error!

#### Class Types

Explicitly enforcing that a class meets a particular contract, is also possible in TypeScript.

    interface ClockInterface {
        currentTime: Date;
    }

    class Clock implements ClockInterface {
        currentTime: Date;
        constructor(h: number, m: number) { }
    }

You can also describe methods in an interface that are implemented in the class.

    interface ClockInterface {
        currentTime: Date;
        setTime(d: Date);
    }

    class Clock implements ClockInterface {
        currentTime: Date;
        setTime(d: Date) {
            this.currentTime = d;
        }
        constructor(h: number, m: number) { }
    }

If you create an interface with a construct signature and try to create a class that implements this interface you get an error. This is because when a class implements an interface, only the instance side of the class is checked. Since the constructor sits in the static side, it is not included in this check.

    interface ClockConstructor {
        new (hour: number, minute: number): ClockInterface;
    }
    interface ClockInterface {
        tick();
    }

    function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
        return new ctor(hour, minute);
    }

    class DigitalClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("beep beep");
        }
    }
    class AnalogClock implements ClockInterface {
        constructor(h: number, m: number) { }
        tick() {
            console.log("tick tock");
        }
    }

    let digital = createClock(DigitalClock, 12, 17);
    let analog = createClock(AnalogClock, 7, 32);

#### Extending Interfaces

    interface Shape {
        color: string;
    }

    interface Square extends Shape {
        sideLength: number;
    }

    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;

An interface can extend multiple interfaces, creating a combination of all of the interfaces.

    interface Shape {
        color: string;
    }

    interface PenStroke {
        penWidth: number;
    }

    interface Square extends Shape, PenStroke {
        sideLength: number;
    }

    let square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;
    square.penWidth = 5.0;

#### Hybrid Types

You may occasionally encounter an object that works as a combination of some of the types described above.

    interface Counter {
        (start: number): string;
        interval: number;
        reset(): void;
    }

    function getCounter(): Counter {
        let counter = <Counter>function (start: number) { };
        counter.interval = 123;
        counter.reset = function () { };
        return counter;
    }

    let c = getCounter();
    c(10);
    c.reset();
    c.interval = 5.0;

#### Interfaces Extending Classes

When an interface type extends a class type it inherits the members of the class but not their implementations. Interfaces inherit even the private and protected members of a base class. This means that when you create an interface that extends a class with private or protected members, that interface type can only be implemented by that class or a subclass of it.

This is useful when you have a large inheritance hierarchy, but want to specify that your code works with only subclasses that have certain properties. The subclasses don’t have to be related besides inheriting from the base class.

    class Control {
        private state: any;
    }

    interface SelectableControl extends Control {
        select(): void;
    }

    class Button extends Control implements SelectableControl {
        select() { }
    }

    class TextBox extends Control {
    }

    // Error: Property 'state' is missing in type 'Image'.
    class Image implements SelectableControl {
        select() { }
    }

    class Location {
    }

### Classes

#### Classes

    class Greeter {
        greeting: string;
        constructor(message: string) {
            this.greeting = message;
        }
        greet() {
            return "Hello, " + this.greeting;
        }
    }

let greeter = new Greeter("world");

#### Inheritance

Classes inherit properties and methods from base classes.

    class Animal {
        name: string;
        constructor(theName: string) { this.name = theName; }
        move(distanceInMeters: number = 0) {
            console.log(`${this.name} moved ${distanceInMeters}m.`);
        }
    }

    class Snake extends Animal {
        constructor(name: string) { super(name); }
        move(distanceInMeters = 5) {
            console.log("Slithering...");
            super.move(distanceInMeters);
        }
    }

    class Horse extends Animal {
        constructor(name: string) { super(name); }
        move(distanceInMeters = 45) {
            console.log("Galloping...");
            super.move(distanceInMeters);
        }
    }

    let sam = new Snake("Sammy the Python");
    let tom: Animal = new Horse("Tommy the Palomino");

    sam.move();
    tom.move(34);

Each derived class that contains a constructor function *must* call *super()* which will execute the constructor of the base class. What's more, before we *ever* access a property on *this* in a constructor body, we *have* to call *super()*.

#### Public, private, and protected modifiers

In TypeScript, each member is *public* by default.

When a member is marked *private*, it cannot be accessed from outside of its containing class.

When we compare two different types, regardless of where they came from, if the types of all members are compatible, then we say the types themselves are compatible.

However, when comparing types that have *private* and *protected* members, we treat these types differently. For two types to be considered compatible, if one of them has a *private* member, then the other must have a *private* member that originated in the same declaration. The same applies to *protected* members.

    class Animal {
        private name: string;
        constructor(theName: string) { this.name = theName; }
    }

    class Rhino extends Animal {
        constructor() { super("Rhino"); }
    }

    class Employee {
        private name: string;
        constructor(theName: string) { this.name = theName; }
    }

    let animal = new Animal("Goat");
    let rhino = new Rhino();
    let employee = new Employee("Bob");

    animal = rhino;
    animal = employee; // Error: 'Animal' and 'Employee' are not compatible

A constructor may also be marked *protected*. This means that the class cannot be instantiated outside of its containing class, but can be extended.

#### Readonly modifier

You can make properties readonly by using the *readonly* keyword. Readonly properties must be initialized at their declaration or in the constructor.

    class Octopus {
        readonly name: string;
        readonly numberOfLegs: number = 8;
        constructor (theName: string) {
            this.name = theName;
        }
    }
    let dad = new Octopus("Man with the 8 strong legs");
    dad.name = "Man with the 3-piece suit"; // error! name is readonly.

Parameter properties are declared by prefixing a constructor parameter with an accessibility modifier or *readonly*, or both. Using *private* for a parameter property declares and initializes a private member; likewise, the same is done for *public*, *protected*, and *readonly*.

    class Octopus {
        readonly numberOfLegs: number = 8;
        constructor(readonly name: string) {
        }
    }

#### Accessors

TypeScript supports getters/setters as a way of intercepting accesses to a member of an object.

    let passcode = "secret passcode";

    class Employee {
        private _fullName: string;

        get fullName(): string {
            return this._fullName;
        }

        set fullName(newName: string) {
            if (passcode && passcode == "secret passcode") {
                this._fullName = newName;
            }
            else {
                console.log("Error: Unauthorized update of employee!");
            }
        }
    }

    let employee = new Employee();
    employee.fullName = "Bob Smith";
    if (employee.fullName) {
        console.log(employee.fullName);
    }

Accessors with a *get* and no *set* are automatically inferred to be *readonly*.

#### Static Properties

Each instance accesses the static properties through prepending the name of the class.

    class Grid {
        static origin = {x: 0, y: 0};
        calculateDistanceFromOrigin(point: {x: number; y: number;}) {
            let xDist = (point.x - Grid.origin.x);
            let yDist = (point.y - Grid.origin.y);
            return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
        }
        constructor (public scale: number) { }
    }

    let grid1 = new Grid(1.0);  // 1x scale
    let grid2 = new Grid(5.0);  // 5x scale

    console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
    console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));

#### Abstract Classes

    abstract class Animal {
        abstract makeSound(): void;
        move(): void {
            console.log("roaming the earth...");
        }
    }

#### Advanced Techniques

    class Greeter {
        static standardGreeting = "Hello, there";
        greeting: string;
        greet() {
            if (this.greeting) {
                return "Hello, " + this.greeting;
            }
            else {
                return Greeter.standardGreeting;
            }
        }
    }

    let greeter1: Greeter;
    greeter1 = new Greeter();
    console.log(greeter1.greet());

    let greeterMaker: typeof Greeter = Greeter;
    greeterMaker.standardGreeting = "Hey there!";

    let greeter2: Greeter = new greeterMaker();
    console.log(greeter2.greet());

Here we use *typeof Greeter*, that is "give me the type of the *Greeter* class itself" rather than the instance type. This type will contain all of the static members of Greeter along with the constructor that creates instances of the *Greeter* class. We show this by using *new* on *greeterMaker*, creating new instances of *Greeter* and invoking them as before.

### Functions

To begin, just as in JavaScript, TypeScript functions can be created both as a named function or as an anonymous function. This allows you to choose the most appropriate approach for your application, whether you're building a list of functions in an API or a one-off function to hand off to another function.

    // Named function
    function add(x, y) {
        return x + y;
    }

    // Anonymous function
    let myAdd = function(x, y) { return x + y; };

#### Function Types

TypeScript can figure the return type out by looking at the return statements, so we can also optionally leave this off in many cases.

    let myAdd = function(x: number, y: number): number { return x + y; };
    // Equals to the following statement
    let myAdd: (x: number, y: number) => number =
        function(x: number, y: number): number { return x + y; };

A function's type has the same two parts: the type of the arguments and the return type. When writing out the whole function type, both parts are required. We write out the parameter types just like a parameter list, giving each parameter a name and a type. This name is just to help with readability. As long as the parameter types line up, it's considered a valid type for the function, regardless of the names you give the parameters in the function type.

We make it clear which is the return type by using a fat arrow (`=>`) between the parameters and the return type. As mentioned before, the return type is a required part of the function type, so if the function doesn't return a value, you would use *void* instead of leaving it off.

#### Optional and Default Parameters

In TypeScript, adding a `?` to the end of parameters we want to be optional.

    function buildName(firstName: string, lastName?: string) {
        if (lastName)
            return firstName + " " + lastName;
        else
            return firstName;
    }

    let result1 = buildName("Bob");                  // works correctly now
    let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
    let result3 = buildName("Bob", "Adams");         // ah, just right

Any optional parameters must follow required parameters.

In TypeScript, we can also set a value that a parameter will be assigned if the user does not provide one, or if the user passes *undefined* in its place. These are called default-initialized parameters.

    function buildName(firstName: string, lastName = "Smith") {
        return firstName + " " + lastName;
    }

    let result1 = buildName("Bob");                  // works correctly now, returns "Bob Smith"
    let result2 = buildName("Bob", undefined);       // still works, also returns "Bob Smith"
    let result3 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
    let result4 = buildName("Bob", "Adams");         // ah, just right

Default-initialized parameters that come after all required parameters are treated as optional, and just like optional parameters, can be omitted when calling their respective function.

Unlike plain optional parameters, default-initialized parameters don't *need* to occur after required parameters. If a default-initialized parameter comes before a required parameter, users need to explicitly pass *undefined* to get the default initialized value.

    function buildName(firstName = "Will", lastName: string) {
        return firstName + " " + lastName;
    }

    let result1 = buildName("Bob");                  // error, too few parameters
    let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
    let result3 = buildName("Bob", "Adams");         // okay and returns "Bob Adams"
    let result4 = buildName(undefined, "Adams");     // okay and returns "Will Adams"

#### Rest Parameters

*Rest parameters* are treated as a boundless number of optional parameters. When passing arguments for a rest parameter, you can use as many as you want; you can even pass none. The compiler will build an array of the arguments passed in with the name given after the ellipsis (`...`), allowing you to use it in your function.

    function buildName(firstName: string, ...restOfName: string[]) {
        return firstName + " " + restOfName.join(" ");
    }

    let buildNameFun: (fname: string, ...rest: string[]) => string = buildName;
    let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");

#### this

Arrow functions capture the *this* where the function is created rather than where it is invoked.

    let deck = {
        suits: ["hearts", "spades", "clubs", "diamonds"],
        cards: Array(52),
        createCardPicker: function() {
            // NOTE: the line below is now an arrow function, allowing us to capture 'this' right here
            return () => {
                let pickedCard = Math.floor(Math.random() * 52);
                let pickedSuit = Math.floor(pickedCard / 13);

                return {suit: this.suits[pickedSuit], card: pickedCard % 13};
            }
        }
    }

    let cardPicker = deck.createCardPicker();
    let pickedCard = cardPicker();

    alert("card: " + pickedCard.card + " of " + pickedCard.suit);

#### Overloads

Supply multiple function types for the same function as a list of overloads. This list is what the compiler will use to resolve function calls.

    let suits = ["hearts", "spades", "clubs", "diamonds"];

    function pickCard(x: {suit: string; card: number; }[]): number;
    function pickCard(x: number): {suit: string; card: number; };
    function pickCard(x): any {
        // Check to see if we're working with an object/array
        // if so, they gave us the deck and we'll pick the card
        if (typeof x == "object") {
            let pickedCard = Math.floor(Math.random() * x.length);
            return pickedCard;
        }
        // Otherwise just let them pick the card
        else if (typeof x == "number") {
            let pickedSuit = Math.floor(x / 13);
            return { suit: suits[pickedSuit], card: x % 13 };
        }
    }

    let myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
    let pickedCard1 = myDeck[pickCard(myDeck)];
    alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

    let pickedCard2 = pickCard(15);
    alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);

### Generics

#### Hello World of Generics

    function identity<T>(arg: T): T {
        return arg;
    }

    let output = identity<string>("myString");  // type of output will be 'string'
    let output = identity("myString");          // type of output will be 'string'

#### Working with Generic Type Variables

When you begin to use generics, you'll notice that when you create generic functions like *identity*, the compiler will enforce that you use any generically typed parameters in the body of the function correctly. That is, that you actually treat these parameters as if they could be any and all types.

#### Generic Types

    interface GenericIdentityFn<T> {
        (arg: T): T;
    }

    function identity<T>(arg: T): T {
        return arg;
    }

    let myIdentity: GenericIdentityFn<number> = identity;

#### Generic Classes

Generic classes have a generic type parameter list in angle brackets (`<>`) following the name of the class.

    class GenericNumber<T> {
        zeroValue: T;
        add: (x: T, y: T) => T;
    }

    let myGenericNumber = new GenericNumber<number>();
    myGenericNumber.zeroValue = 0;
    myGenericNumber.add = function(x, y) { return x + y; };

Generic classes are only generic over their instance side rather than their static side, so when working with classes, static members can not use the class's type parameter.

#### Generic Constraints

    interface Lengthwise {
        length: number;
    }

    function loggingIdentity<T extends Lengthwise>(arg: T): T {
        console.log(arg.length);  // Now we know it has a .length property, so no more error
        return arg;
    }

    loggingIdentity({length: 10, value: 3});

You can declare a type parameter that is constrained by another type parameter.

    function getProperty<T, K extends keyof T>(obj: T, key: K) {
        return obj[key];
    }

    let x = { a: 1, b: 2, c: 3, d: 4 };

    getProperty(x, "a"); // okay
    getProperty(x, "m"); // error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.

When creating factories in TypeScript using generics, it is necessary to refer to class types by their constructor functions.

    function create<T>(c: {new(): T; }): T {
        return new c();
    }

### Enums

#### Numeric enums

Numeric enums is as follows:
    
    enum Direction {
        Up = 1,
        Down,
        Left,
        Right,
    }

Enums without initializers either need to be first, or have to come after numeric enums initialized with numeric constants or other constant enum members.

#### String enums

In a *string enum*, each member has to be constant-initialized with a string literal, or with another string enum member.

    enum Direction {
        Up = "UP",
        Down = "DOWN",
        Left = "LEFT",
        Right = "RIGHT",
    }

#### Heterogeneous enums

Technically enums can be mixed with string and numeric members (Heterogeneous enums). Unless you’re really trying to take advantage of JavaScript’s runtime behavior in a clever way, it’s advised that you don’t do this.

    enum BooleanLikeHeterogeneousEnum {
        No = 0,
        Yes = "YES",
    }

#### Computed and constant members

#### Union enums and enum member types

There is a special subset of constant enum members that aren’t calculated: literal enum members. A literal enum member is a constant enum member with no initialized value, or with values that are initialized to

* any string literal (e.g. "foo", "bar, "baz")
* any numeric literal (e.g. 1, 100)
* a unary minus applied to any numeric literal (e.g. -1, -100)

Literal enum members also become types as well!

    enum ShapeKind {
        Circle,
        Square,
    }

    interface Circle {
        kind: ShapeKind.Circle;
        radius: number;
    }

    interface Square {
        kind: ShapeKind.Square;
        sideLength: number;
    }

    let c: Circle = {
        kind: ShapeKind.Square,
        //    ~~~~~~~~~~~~~~~~ Error!
        radius: 100,
    }

Literal enum themselves effectively become a union of each enum member.

#### Enums at runtime

In addition to creating an object with property names for members, numeric enums members also get a reverse mapping from enum values to enum names.

    enum Enum {
        A
    }
    let a = Enum.A;
    let nameOfA = Enum[a]; // "A"

To avoid paying the cost of extra generated code and additional indirection when accessing enum values, it’s possible to use const enums.

    const enum Enum {
        A = 1,
        B = A * 2
    }

Const enums can only use constant enum expressions and unlike regular enums they are completely removed during compilation. Const enum members are inlined at use sites.

#### Ambient enums

Ambient enums are used to describe the shape of already existing enum types.

    declare enum Enum {
        A = 1,
        B,
        C = 2
    }

One important difference between ambient and non-ambient enums is that, in regular enums, members that don't have an initializer will be considered constant if its preceding enum member is considered constant. In contrast, an ambient (and non-const) enum member that does not have initializer is always considered computed.

### Type Inference

When a type inference is made from several expressions, the types of those expressions are used to calculate a "best common type".

When no best common type is found, the resulting inference is the union array type.

If the contextually typed expression contains explicit type information, the contextual type is ignored.

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
