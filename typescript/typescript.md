# TYPESCRIPT

## CONCEPTS

#### Primitives and types:

    string
    number
    boolean
    number[]
    string[]
    Array<number> // tuple
    any
    null
    undefined

#### Type annotations:

    let helloWorld: string = "Hello World";

#### Interfaces:

    interface User {
        name: string;
        id: number;
    }

    const user: User = {
        name: "Hayes",
        id: 0,
    };

#### OOP:

    interface User {
        name: string;
        id: number;
    }

    class UserAccount {
        name: string;
        id: number;

        constructor(name: string, id: number) {
            this.name = name;
            this.id = id;
        }
    }

    const user: User = new UserAccount("Murphy", 1);

#### You can use interfaces to annotate parameters and return values to functions:

    function getAdminUser(): User {
        //...
    }

    function deleteUser(user: User) {
        // ...
    }

#### Primitives:

- There are already a small set of primitive types available in JavaScript: boolean, bigint, null, number, string, symbol, and undefined, which you can use in an interface. 
- TypeScript extends this list with a few more, such as any (allow anything), unknown (ensure someone using this type declares what the type is), never (it’s not possible that this type could happen), and void (a function which returns undefined or has no return value).

#### Types and Interfaces:

You should prefer interface. Use type when you need specific features.  
*(more on this later)*

#### Union:

With a union, you can declare that a type could be one of many types. For example, you can describe a boolean type as being either true or false:

    type MyBool = true | false;
    let helloWorld: MyBool = true;

More examples:

    type WindowStates = "open" | "closed" | "minimized";
    type LockStates = "locked" | "unlocked";
    type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

Unions provide a way to handle different types too. For example, you may have a function that takes an array or a string:

    function getLength(obj: string | string[]) {
        return obj.length;
    }

#### Typeof:

Check type of a variable:

    string	    typeof s === "string"
    number	    typeof n === "number"
    boolean	    typeof b === "boolean"
    undefined	typeof undefined === "undefined"
    function	typeof f === "function"
    array	    Array.isArray(a)

You can make a function return different values depending on whether it is passed a string or an array:

    function wrapInArray(obj: string | string[]) {
        if (typeof obj === "string") {
            return [obj];
        } else {
            return obj;
        }
    }

#### Generics:

Generics provide variables to types. A common example is an array. An array without generics could contain anything. An array with generics can describe the values that the array contains.

    type StringArray = Array<string>;
    type NumberArray = Array<number>;
    type ObjectWithNameArray = Array<{ name: string }>;

You can declare your own types that use generics:

    interface Backpack<Type> {
        add: (obj: Type) => void;
        get: () => Type;
    }

    declare const backpack: Backpack<string>;

    const object = backpack.get();

    // Since the backpack variable is a string, you can't pass a number to the add function.
    backpack.add(23);
    
    ERR: Argument of type 'number' is not assignable to parameter of type 'string'.

#### Structural Type System:

In a structural type system, if two objects have the same shape, they are considered to be of the same type.  
In the below code, the point variable is never declared to be a *Point* type. However, TypeScript compares the shape of point to the shape of *Point* in the type-check. They have the same shape, so the code passes.

    interface Point {
        x: number;
        y: number;
    }

    function logPoint(p: Point) {
        console.log(`${p.x}, ${p.y}`);
    }

    const point = { x: 12, y: 26 };
    logPoint(point);

**NOTE:**  
It’s best not to add annotations when the type system would end up inferring the same type anyway.  

    let msg = "hello there!";
    // let msg: string

#### Function parameter type and return type annotations:

    function greet(name: string) {
        console.log("Hello, " + name.toUpperCase() + "!!");
    }

    function getFavoriteNumber(): number {
        return 26;
    }

    let sum = (a: number, b: number): number => {  
        return a + b;  
    }  

#### Object type:

    function printCoord(pt: { x: number, y: number }) {
        console.log("The coordinate's x value is " + pt.x);
        console.log("The coordinate's y value is " + pt.y);
    }
    printCoord({ x: 3, y: 7 });

Optional properties on object:

    function printName(obj: { first: string; last?: string }) {
        // ...
    }
    printName({ first: "Bob" });
    printName({ first: "Alice", last: "Alisson" });

#### Working with Union Types:

For example, if you have the union string | number, you can’t use methods that are only available on string:

    function printId(id: number | string) {
        console.log(id.toUpperCase());    
    }

    ERR: Property 'toUpperCase' does not exist on type 'string | number'.

The solution is to *narrow* the union with code:

    function printId(id: number | string) {
        if (typeof id === "string") {
            console.log(id.toUpperCase());
        } else {
            console.log(id);
        }
    }

Another example:

    function welcomePeople(x: string[] | string) {
        if (Array.isArray(x)) {
            console.log("Hello, " + x.join(" and "));
        } else {
            console.log("Welcome lone traveler " + x);
        }
    }

#### Type Alias:

    type Point = {
        x: number;
        y: number;
    };

    function printCoord(pt: Point) {
        console.log("The coordinate's x value is " + pt.x);
        console.log("The coordinate's y value is " + pt.y);
    }

    printCoord({ x: 100, y: 100 });

You can actually use a type alias to give a name to any type at all, not just an object type. For example, a type alias can name a union type:

    type ID = number | string;

### Interfaces:

    interface Point {
        x: number;
        y: number;
    }

    function printCoord(pt: Point) {
        console.log("The coordinate's x value is " + pt.x);
        console.log("The coordinate's y value is " + pt.y);
    }

    printCoord({ x: 100, y: 100 });

#### Differences Between Type Aliases and Interfaces:

Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an *interface* are available in *type*, the key distinction is that a *type* cannot be re-opened to add new properties vs an *interface* which is always extendable.

[Interface vs. Types](interface-vs-type.png)

#### Type assertions:

    const x = "hello" as string;
    const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
    const a = (expr as any) as T;

#### Literal Types:

```
let x: "hello" = "hello";
x = "hello"; // OK
x = "howdy"; // ERR
```
```
function compare(a: string, b: string): -1 | 0 | 1 {
    return a === b ? 0 : a > b ? 1 : -1;
}
```
```
function printText(s: string, alignment: "left" | "right" | "center") {
    // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre"); // ERR
```
```
interface Options {
    width: number;
}
function configure(x: Options | "auto") {
    // ...
}
configure({ width: 100 });
configure("auto");
configure("automatic"); // ERR
```

#### Literal Interface:

In the below example req.method is inferred to be string, not "GET":

    const req = { url: "https://example.com", method: "GET" };
    handleRequest(req.url, req.method);

The as const suffix acts like const but for the type system, ensuring that all properties are assigned the literal type instead of a more general version like *string* or *number*.

    const req = { url: "https://example.com", method: "GET" } as const;
    handleRequest(req.url, req.method);

#### `strictNullChecks` on:

With strictNullChecks on, when a value is null or undefined, you will need to test for those values before using methods or properties on that value.

    function doSomething(x: string | null) {
        if (x === null) {
            // do nothing
        } else {
            console.log("Hello, " + x.toUpperCase());
        }
    }

#### Non-null Assertion Operator (Postfix `!`):

Writing ! after any expression is effectively a type assertion that the value isn’t `null` or `undefined`:

    function liveDangerously(x?: number | null) {
        // No error
        console.log(x!.toFixed());
    }

Only use ! when you know that the value can’t be `null` or `undefined`.