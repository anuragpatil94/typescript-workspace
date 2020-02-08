# Typescript

> Typescript = Javascript + **_A Type System_**

## Table of Contents

- [Typescript](#typescript)
  - [Table of Contents](#table-of-contents)
  - [Goals](#goals)
    - [Syntax + Features](#syntax--features)
    - [Design Patterns with TS](#design-patterns-with-ts)
  - [Advantages](#advantages)
  - [Commands](#commands)
    - [Run Typescript](#run-typescript)
  - [Steps](#steps)
    - [FetchJSON](#fetchjson)
    - [Type System](#type-system)
      - [Plain Definition + Overview](#plain-definition--overview)
      - [Why do we care](#why-do-we-care)
      - [When to use this](#when-to-use-this)
    - [Type Annotations + Type Inference](#type-annotations--type-inference)
      - [Variables](#variables)
      - [Functions](#functions)
      - [Objects](#objects)
      - [When to use Type Annotations](#when-to-use-type-annotations)
    - [Typed Arrays](#typed-arrays)
    - [Tuples](#tuples)
    - [Interface](#interface)
    - [Class](#class)
  - [(Project) Maps](#project-maps)
  - [(Project) Sort](#project-sort)
    - [Typescript Compiler Configuration](#typescript-compiler-configuration)
      - [Automate Compiling and Execution](#automate-compiling-and-execution)
    - [Type Guard](#type-guard)
  - [(Project) Football Stats](#project-football-stats)
    - [ENUM](#enum)
      - [When to use ENUM](#when-to-use-enum)
    - [Type Assertion](#type-assertion)
      - [Converting to TUPLE](#converting-to-tuple)
    - [Reusability](#reusability)
      - [Refactor #1](#refactor-1)
      - [Refactor #3](#refactor-3)
    - [Retrospective (Interface or Abstract Class)](#retrospective-interface-or-abstract-class)
    - [Generics](#generics)
      - [Class Generics](#class-generics)
      - [Function Generics](#function-generics)
      - [Generic Constraints](#generic-constraints)
  - [Decorators](#decorators)
    - [Property Descriptor](#property-descriptor)
  - [Packages](#packages)

## Goals

### Syntax + Features

- Understand basic types in TS
- Function typing + annotations
- Type definition files
- Arrays in TS
- Modules Systems
- Classes + Refresher on OOP
- What is an interface?
- What is the syntax for defining an interface?

### Design Patterns with TS

- Projects
- How do we use interfaces to write reusable code?

## Advantages

- Helps us catch errors during development
- 👤 Uses 'type annotations' to analyze our code
- Only active during development
- ❌Doesn't provide any performance optimization

## Commands

- `tsc -v` -> Typescirpt Compiler
- `tsc-node <filename>.ts` -> build and run at same time in cmd
- `parcel <filename>.ts` -> This will build the html index file connecting whole project will run on a server.
- `tsc --init` -> To create TS Config file.
- `tsc` -> Once TS Config file is set up we can run `tsc` to build whole src files.
- `tsc -w` -> If an changes made we can run this command to rebuild but it will only look for changes instead of build whole project again

### Run Typescript

- Compile the file `tsc file.ts`
- Compile file is `file.js`. Run the file as `node file.js`

> ts-node will combile both the commands at the same time. So, we don't have to write command 2 times.

- Command - `ts-node file.ts`

## Steps

### FetchJSON

1. Make a network request to fetch some JSON and print the result
   How Typescript handles errors

### Type System

#### Plain Definition + Overview

Type - Easy way to refer to the different properties + functions that a value has.

#### Why do we care

- To avoid errors
- increases redability of the code

#### When to use this

- Everywhere, Any value which is declared has a type associated with it.

### Type Annotations + Type Inference

- These are used differently for `variables`, `functions`, `objects`

> **_Type Annotations_** - Code we add to tell ts what type of value a variable will refer to \
> **_Type inference_** - Typescript tries to figure out what type of value a variable refers to based on how the variable was initialized.

#### Variables

```typescript
let apples: number = 5;

// Error if something else than the annotated type
// apples = "hello";

let speed: string = "fast";
let nothing: undefined = undefined;

// Built in objects
let now: Date = new Date();

// Array
let colors: string[] = ["red", "green"];
```

#### Functions

> Only inferences return value \
> NO TYPE INFERENCE FOR ARGUMENTS hence Always add annotations for arguments

```typescript
/**
 * NO TYPE INFERENCE FOR ARGUMENTS hence Always add annotations for arguments
 * @param a :number
 * @param b :number
 *
 * TYPE INFERENCE ONLY ON RETURN
 */
const add = (a: number, b: number): number => {
  return a + b;
};

function divide(a: number, b: number): number {
  return a / b;
}

const multiphy = function(a: number, b: number): number {
  return a * b;
};

/**
 * All 3 statements return void
 * @param message string
 */
const logger = (message: string): void => {
  console.log(message);
  //   return null;
  //   return undefined;
};

/**
 * Only when it only throws error. if the function returns errors or return string then we annotate string
 * @param message string
 */
const throwError = (message: string): never => {
  throw new Error(message);
};

const todaysWeather = {
  date: new Date(),
  weather: "sunny"
};
const logWeather = (forecast: { date: Date; weather: string }): void => {
  console.log(forecast.date);
  console.log(forecast.weather);
};
logWeather(todaysWeather);

// ES2015
const logWeather2015 = ({ date, weather }) => {
  console.log(date);
  console.log(weather);
};

// Destructuring
const logWeather2016 = ({
  date,
  weather
}: {
  date: Date;
  weather: string;
}): void => {
  console.log(date);
  console.log(weather);
};
```

```typescript
// In the annotation - what input we send to the function and what output is returned
const logNumber: (i: number) => void = (i: number) => {
  console.log(i);
};
```

#### Objects

```typescript
const profile = {
  name: "alex",
  age: 20,
  coords: {
    lat: 0,
    lng: 15
  },
  setAge(age: number): void {
    this.age = age;
  }
};

const { age }: { age: number } = profile;
const {
  coords: { lat, lng }
}: { coords: { lat: number; lng: number } } = profile;

// Classes
class Car {}
let car: Car = new Car();

// Object Literal
let point: { x: number; y: number } = {
  x: 10,
  y: 20
};
```

#### When to use Type Annotations

- When we declare a variable on one line then initialize it later
- When we want a variable to have atype that can't be infered
- When a function returns the `any` type and we need to clarify the value
  - Avoid `any` at all cost

```typescript
// When to use annotations
// 1) Function that returns the 'any' type
const json = '{"x":10, "y":20}';
// const coordinates = JSON.parse(json); // any type, returns any type
const coordinates: { x: number; y: number } = JSON.parse(json); // any type, returns any type
// 'false' -> JSON.parse -> any

// 2) Declare first, Initialize later
let words = ["red", "green"];
// let foundWord; //Type Any
let foundWord: boolean; //Type Any
words.forEach(word => {
  if (word == "green") {
    foundWord = true;
  }
});

// 3) When Variable cannot be inferred correctly
let numberss = [-10, -1, 12];
// Bad code - 2 different types  <0 -> false,    >0 -> print num
// let numberssAboveZero = false;   // Always boolean
let numberssAboveZero: boolean | number = false; //boolean or number

numberss.forEach(element => {
  if (element > 0) {
    numberssAboveZero = element;
  }
});
```

### Typed Arrays

```typescript
//We don't need to write annotations since if the array has data it inferences the type
const carMakers: string[] = ["ford", "toyota", "chevy"];
const dates = [new Date(), new Date()];
const carsByMake: string[][] = [["f150"], ["corolla"], ["camaro"]];

// We need only when the array is empty
const numbersList: number[] = [];
```

- TS can do type inference when extracting values from an array
- TS can prevent us from adding incompatible values to the array
- We can ge help with `map`, `forEach`, `reduce` functions
- Flexible - arrays can still contain multiple different types

```typescript
// Help with Inference when extracting values
// Hover over car to check inference
const car = carMakers[0];
const date = dates[0];

// prevent adding incompatible values
// carMakers.push(100);

// Help with 'map'
carMakers.map((car: string): string => {
  return car.toUpperCase();
});

// Flexible Types in array
const importantDates = [new Date(), "2020-28-01"];
const importantDatess: (Date | string)[] = [new Date()];
importantDatess.push("2020");
// importantDatess.push(2020);
```

### Tuples

- Array-like structure where each element represents some property of a record

```typescript
const drink = {
  color: "brown",
  carbonated: true,
  sugar: 40
};

// Still Array
// const pepsi = ["black", true, 40];

// This annotation turns array to tuple
const pepsi: [string, boolean, number] = ["black", true, 40];

// Type Alias
type Drink = [string, boolean, number];
const sprite: Drink = ["clear", true, 40];
```

### Interface

> Creates a new type, describing the property names and value types of an object

`Interface` + `Classes` -> Strong reusable code

```Typescript

interface Vehicle {
  name: string;
  year: number;
  broken: boolean;
}

const printVehiclee = (vehicle: Vehicle): void => {
  console.log("Name: ", vehicle.name);
  console.log("Year: ", vehicle.year);
  console.log("Broken: ", vehicle.broken);
};

printVehiclee(oldCivic);

// Another Example
interface Vehicle {
  name: string;
  year: number;
  broken: boolean;
  summary(): string; //has a Function
}

```

### Class

> Blueprint to create an object with some fields and methods to represent a 'thing'

```typescript
class Vehicle {
  drive(): void {
    console.log("Vroom Vroom");
  }

  honk(): void {
    console.log("Peep!");
  }
}

class Car extends Vehicle {
  drive(): void {
    console.log("Boom Boom");
  }
}

const vehicle = new Vehicle();
vehicle.drive();
vehicle.honk();

const car = new Car();
car.drive();
car.honk();
```

## (Project) Maps

- `Type Definition File` to connect typescript and javascript

- But some modules doesn't come with type declaration files.
  - You will know this based on an error which is shown when the package is imported.
  
  ```text
    WARNING TEXT
    - 'faker' is declared but its value is never read.ts(6133)
    - Could not find a declaration file for module 'faker'. '.../node_modules/faker/index.js' implicitly has an 'any' type.
    - Try `npm install @types/faker` if it exists or add a new declaration (.d.ts) file containing `declare module 'faker';`ts(7016)
  ```

- For example faker doesn't have type declaration file in its package
- In such cases we use @types package for the package. example `@types/{library name}`
- `@types/faker`

## (Project) Sort

- In this application we are making a sorting algorithm such that we can take number, string or linked list to sort.
- We will be focusing on interface and class such  that we reuse the code

### Typescript Compiler Configuration

- To make sure that the .js file which is created on compiling .ts file goes to the correct folder like `build`, we have to create a file called tsconfig.json we run `tsc --config` command
- To do this,
  - we find 2 properties in the `tsconfig.json` file. `outDir` for build files and `rootDir` for Source files
    - `tsc --init` -> To create TS Config file.
    - `tsc` -> Once TS Config file is set up we can run `tsc` to build whole src files.
    - `tsc -w` -> If an changes made we can run this command to rebuild but it will only look for changes instead of build whole project again.
    - run using `node build/index.js`

#### Automate Compiling and Execution

- Use `Concurrently` to run multiple scripts 1) compile ts using `tsc -w` and 2) `nodemon` to run node everytime.

```json
//package.json file

  "scripts": {
    "start:build": "tsc -w",
    "start:run": "nodemon build/index.js",
    "start": "concurrently npm:start:*"
  }
```

### Type Guard

- `this.collection instanceof Array`

## (Project) Football Stats

- CSV -> Load -> Parse -> ANalyze -> Report
- Topics Covered
  - `enum`
  - `generics`
  - `abstract class`
  - `composition`

### ENUM

- This will also create a new type

```ts
// Just a way to show that the properties of an enum are very closely related values
enum MatchResult {
  HomeWin = "H",
  AwayWin = "A",
  Draw = "D"
}
```

#### When to use ENUM

- Follow near-identical syntax rules as normal objects
- Creates an object with the same keys and values when converted from TS to JS
- Primary goal is to signal to other engineers that these are all closely related values
- Use whenever we have a small fixed set of values that are all closely related and known at compile time

### Type Assertion

```ts
return [
  dateStringToDate(row[0]),
  row[1],
  row[2],
  parseInt(row[3]),
  parseInt(row[4]),
  row[5] as MatchResult                  // default behavior was that row[5] was a string. This is Type Assertion where we are trying to override Typescript's default behavior
];

// Instead of array since the index of each property won't change we will use TUPLE
```

#### Converting to TUPLE

```ts
// New Tuple
type MatchData = [Date, string, string, number, number, MatchResult, string];

export class CSVFileReader {
  data: MatchData[] = [];                               // Changes Here
  constructor(public filename: string) {}
  read(): void {
    this.data = fs
      .readFileSync(this.filename, {
        encoding: "utf-8"
      })
      .trim()
      .split("\n")
      .map((row: string): string[] => row.split(","))
      .map(
        (row: string[]): MatchData => {                              // Changes Here
          return [
            dateStringToDate(row[0]),
            row[1],
            row[2],
            parseInt(row[3]),
            parseInt(row[4]),
            row[5] as MatchResult,
            row[6]
          ];
        }
      );
  }
}
```

### Reusability

#### Refactor #1

- Make CSVFileReader class a Abstract Class so that if in future we have any other kind of data, it can use CSVFileReader and create its own class that extends it.
- Since we don't want any reference to Football match in CSVFileReader, we have to somehow remove MatchData Type from the file. But that would create a issue because data property needs to have a type.
- We can change is to `any` but that's a bad refactor. Hence we use `Generics`

#### Refactor #3

- Using Interface
- So here what we are doing is creating a `interface` called `DataReader` which has same methods as `CSVFileReader`. This makes sure that infuture if we have any other file readers it would have this method and then we can use it for our own `<ANY>DataReader`.
- The way we do is we pass which reader we want to use as a parameter to out custom MatchReader Class, so that it can use the reusable code

### Retrospective (Interface or Abstract Class)

- INHERITANCE VS COMPOSITION
- IS A vs HAS A
- Delegating other classes in the Class if what Composition is so that class can use delegated classes.

### Generics

- Like functio narguments, but for types in class/function definitions
- Allows us to define the type of a propery/argument/return value at a future point
- Used for reusability

#### Class Generics

```ts
// Noting to do with generics

const addOne = (a: number): number => {
  return a + 1;
};
const addTwo = (a: number): number => {
  return a + 2;
};

// Instead of above where we are just creating a new function everytime we do below
// Kindof a Generic but not really. but generic is same concept
const add = (a: number, b: number): number => {
  return a + b;
};
add(10, 1);
add(10, 2);

// Another Example
class HoldNumber {
  data: number;
}
class HoldString {
  data: string;
}

const holdNumebr = new HoldNumber();
holdNumebr.data = 123;

const holdString = new HoldString();
holdString.data = "asdlfkh";

// GENERICS
// Customize the definition of the class
// What we are trying to do is add type on the fly so that we don't have to create a duplicate class everytime.
// This is like an argument just the same way as we added b in the add function.
// If you understand the add analogy what we are doing here is same. instead of b -> <TypeOfData> for a class.
class HoldAnything<TypeOfData> {
  data: TypeOfData;
}

const holdAnythingNumber = new HoldAnything<number>();
// gives error
// holdAnythingNumber.data = "akjsdfh";
holdAnythingNumber.data = 124;

const holdAnythingDate = new HoldAnything<Date>();
// Error
// holdAnythingDate.data = "2018/12/03";
holdAnythingDate.data = new Date();
```

- By Covention Generic Type has a very small name so. instead of `<TypeOfData>` it will be `<T>`

```ts
// Initial Example
class ArrayOfAnything<T> {
  constructor(public collection: T[]) {}
  get(index: number): T {
    return this.collection[index];
  }
}

new ArrayOfAnything<string>(["a", "b"]);

// Uses type inference to make generic type = string because it checks array of strings
const arr = new ArrayOfAnything(["a", "b"]);
```

#### Function Generics

```ts
function printAnything<T>(arr: T[]): void {
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
  }
}

printAnything<string>(["a", "b"]);
// Type Inference
printAnything(["a", "b"]);
// Error
// printAnything<number>(['a','b'])
```

#### Generic Constraints

```ts
class Car {
  print() {
    console.log("CAR");
  }
}

class House {
  print() {
    console.log("HOUSE");
  }
}

interface Printable {
  print(): void;
}

//Constraint will make sure that we will have a print method for type T.
// This is called adding a constraint
function printHousesAndCar<T extends Printable>(arr: T[]): void {
  for (let i = 0; i < arr.length; i++) {
    // It says that type t has no gaurantee that there will be print method
    arr[i].print();
  }
}

// Gives error because numbers don't have print method defined in Printable constraint
// printHousesAndCar([1, 2, 3]);
printHousesAndCar([new House(), new Car()]);
```

## Decorators

- Function that can be used to modify/change/anything different properties/methods in the class
- Not the same as Javascript decorators
- Used on classes only
- Understanding the order in which decorators are ran are the key to understanding them.

 > Enable the `"experimentalDecorators"` and `"emitDecoratorMetadata"` flags in tsconfig

```typescript
    class Boat {
      color: string = "red";                    //Property

      get formattedColor(): string {            //accessor
        return `This boat's color is ${this.color}`;
      }

      @testDecorator
      pilot(): void {                           //method
        console.log("Jush");
      }
    }

    // 1st Decorator
    function testDecorator(target: any, key: string): void {
      console.log("Target: ", target);
      console.log("Key:", key);
    }

    Output:
      Target:  Boat { formattedColor: [Getter], pilot: [Function] }
      Key: pilot

    testDecorator(Boat.prototype, "pilot");   ----- same as -----    @testDecorator
```

- Decorators are applicable for Property, Accessor or Method
  - First argument `target` is prototype of the object `Boat`
  - Second argument is the key of the property/method/accessor on the object
  - Third argument is the property descriptor
  - Decorator are applied *ONE SINGLE TIME* when the code for this class is ran ie.when Javascript 1st parses whole code. (**NOT WHEN THE INSTANCE IS CREATED**).

### Property Descriptor

- For `Methods`
  - `writable` : Whether or not this property can be changed
  - `enumerable` : Whether or not this propety get looped over by a `for...in`
  - `value` : current value
  - `configurable` : property definition can be changed and propery can be deleted

```typescript
    class Boat {
      color: string = "red";
      get formattedColor(): string {
        return `This boat's color is ${this.color}`;
      }
      /**
       * We are trying to excute a piece of code such that whenever pilot is called and
       * we get an error, we run this decorator code
       */
      @logError
      pilot(): void {
        throw new Error();
        console.log("Jush");
      }
    }
    /**
     *
     * @param target Object.Prototype
     * @param key key on which decorator is to be applied
     * @param desc Object that has some configuration options around a property defied in the class object
     */
    function logError(target: any, key: string, desc: PropertyDecorator): void {
      console.log("Target: ", target);
      console.log("Key:", key);
    }
```


## Packages

- `typescript`
- `ts-node` - To build and run with single command
- `parcel-bundler` - This package will allow to run typescript code easily on browser
- `faker` - To generate fake data
- `nodemon` - Rerun node anytime changes are detected
- `concurrently` - Run multiple script at the same time
- `@types/node` - type definition file for all internal node modules
  