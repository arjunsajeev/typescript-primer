# Typescript Primer

Condensed Typescript concepts with code examples

## Primitives - string, number and boolean

```typescript
let x: number

let name: string

let isComplete: boolean
```

## Arrays

```typescript
let numbers: number[]
// or
let numbers: Array<number>
```

## Any

```typescript
let obj = {} // any. Use `noImplicitAny` to prevent
obj()
obj.foo()
```

## Functions

```typescript
// Parameter type annotation
function greet(name: string) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}

// Return type annotation
function getFavoriteNumber(): number {
  return 26;
}
```

## Object types

```typescript
// The parameter's type annotation is an object type
function printCoord(pt: { x: number; y: number }) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 3, y: 7 });

// Optional properties
function printName(obj: { first: string; last?: string }) {
  // ...
}
// Both OK
printName({ first: "Bob" });
printName({ first: "Alice", last: "Alisson" });

// Note: In Javascript `?.` is the optional chaining operator
```

## Union types

```typescript
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
// OK
printId(101);
// OK
printId("202");
// Error
printId({ myID: 22342 });


// When working with union types, you might have to narrow the type before using it if the operation is not common to all types in the union

function printId(id: number | string) {
  if (typeof id === "string") {
    // In this branch, id is of type 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, id is of type 'number'
    console.log(id);
  }
```

## Type aliases

```typescript
// A type alias is just a type with a name
type Point = {
  x: number;
  y: number;
};
 
// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });
```

## Interfaces

```typescript
// An interface declaration is another way to name an object type:

interface Point {
  x: number;
  y: number;
}
 
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });
```

## Interface vs Type alias

Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an `interface` are available in `type`, the key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.

```typescript
// Extending an interface

interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear() 
bear.name
bear.honey

// Extending a type with intersections

type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: boolean 
}

const bear = getBear();
bear.name;
bear.honey;
```

```typescript
// Adding new fields to an existing interface

interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {}); 

// A type cannot be changed after being created

type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```

## Type assertions

When you know more about the type of a value than Typescript, you can use a type assertion to specify a more specific type

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
// or
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

## Literal types

In addition to the general types `string` and `number`, we can refer to *specific* strings and numbers in type positions.

```typescript
let changingString = "Hello World";
changingString = "Olá Mundo";
// Because `changingString` can represent any possible string, that
// is how TypeScript describes it in the type system
changingString;
      
let changingString: string
 
const constantString = "Hello World";
// Because `constantString` can only represent 1 possible string, it
// has a literal type representation
constantString;
```

### Literal inference

When you initialize a variable with an object, TypeScript assumes that the properties of that object might change values later. For example, if you wrote code like this:

```typescript
const obj = { counter: 0 }; // type of counter is inferred as number and not 0
if (someCondition) {
  obj.counter = 1;
}
```

You can use `as const` to convert the entire object to be type literals:

```typescript
const req = { url: "[https://example.com](https://example.com)", method: "GET" } as const;

handleRequest(req.url, req.method);
```

## Null and undefined

Typescript also has corresponding `null` and `undefined` values like Javascript. With [`strictNullChecks`](https://www.typescriptlang.org/tsconfig#strictNullChecks) *on*, when a value is `null` or `undefined`, you will need to test for those values before using methods or properties on that value. Just like checking for `undefined` before using an optional property, we can use *narrowing* to check for values that might be `null`:

```typescript
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

### Non null assertion operator

TypeScript also has a special syntax for removing `null` and `undefined` from a type without doing any explicit checking. Writing `!` after any expression is effectively a type assertion that the value isn’t `null` or `undefined`:

```typescript
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

## Less common primitives

### bigint

```typescript
// Creating a bigint via the BigInt function
const oneHundred: bigint = BigInt(100);
 
// Creating a BigInt via the literal syntax
const anotherHundred: bigint = 100n;
```

### symbol

```typescript
const firstName = Symbol("name");
const secondName = Symbol("name");
 
if (firstName === secondName) {
This condition will always return 'false' since the types 'typeof firstName' and 'typeof secondName' have no overlap.
  // Can't ever happen
}
```

