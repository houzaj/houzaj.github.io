---
layout: post
title: 'TypeScript Notebook - Essential'
date: 2019-09-10
author: hpp2334
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190909%20Problems/main-01.png'
tags: TypeScript
---

> Notebook about learning TypeScript

### BB

Lacking of a chinese input method, I write this article in English. Sorry for my poor English in advance. QAQ  
The following note is not containing all the content in the official guide.  

&nbsp;

### Reference

[https://www.typescriptlang.org/docs/home.html](https://www.typescriptlang.org/docs/home.html)  
[https://www.differencebetween.com/difference-between-javascript-and-vs-typescript/](https://www.differencebetween.com/difference-between-javascript-and-vs-typescript/)  

&nbsp;

### Enviroment

TypeScript: v3.6.2  

&nbsp;

### What is TypeScript

Typescirpt is a superset of JavaScript, which is needed to compiled into javascript file via compiler. It is an OOP language and has static type checking, modules, etc.  

&nbsp;

### Basic Type

```typescript
// boolean
let isOk: boolean = false;

// number
let x: number = 66;
let inf: number = 0x3f3f3f3f;

// string
let str: string = "Hello World";
let template_str: string = `The no. ${ x } string is: ${ str }`; // Template strings
let equiv_str: string = "The no. " + x + " string is: " + str; // equivalant type

// Array
let list: number[] = [1, 2, 3];
let list2: Array<number> = [1, 2, 3];

// Tuple
let tupleX: [string, number] = ["abc", 123];

// Enum
enum Col {Red, Green, Blue};
let c: Col = Col.Green;

// Any
// "Any" can call arbitary methods, but "Object" not
let listAny: any[] = [isOk, x, inf, str, template_str, equiv_str, list, list2, tupleX, c];

// Void
// "void" can be assigned only "null" or "undefined"
function printStr(x : string) : void {
    console.log(x);
}

// Null & Undefined
// "null" can be assigend only "null", and "undefined" can be assigned only "undefined"
let n1 : null = null;
let u1 : undefined = undefined;

// Never
// "never" represents the types of values that never occur
function forever(): never {
    while(true) {

    }
}

// Object
// "object" presents the non-primitive type
function callStudent(x: object) {
    console.log("The id of ${ x.name } is ${ x.id }.");
}
let student: object = {
    id: 1000,
    name: "John"
};

// Type Assertions
// type assertions: like type cast
let aVal: any = "this is a string";
let l1: number = (<string>aVal).length;
let l2: number = (aVal as string).length;
```

&nbsp;

### Interface

#### Basic

**Interace** can describe the requirement of the object.  

#### Point 1: Optional Property

```typescript
interface SquareConfig {
    color?: string,  // optional property (use '?')
    width: number,
    height: number   // needed property
}

function createSquare(conf: SquareConfig): { color: string, area: number } {
    let nSqure = {
        color: "black",
        area: conf.height * conf.width
    }
    if(conf.color) {
        nSqure.color = conf.color;
    }
    return nSqure;
}

// Okay, must satisfy the needed property
let withdout_color_square = createSquare( { width: 100, height: 123 });
// Okay, can have the optional property
let with_color_squre = createSquare( { width: 34, height: 45, color: "green"} );
// Err, property "name" doesn't in the interface
//let errSquare = createSquare({ width: 12, height: 45, name: "Err!" });
// Err, property "height" dosen't exist
//let errSqre = createSquare( { width: 2 } );
```

#### Point 2: Readonly Property

Readonly Property can make variables unchanged after initialization  
Variables use `const`, whereas property use `readonly`  

```typescript
interface Point {
    readonly x: number,
    readonly y: number
};

let p1: Point = {
    x: 50,
    y: 50
};
// p1.x = 50;  // Err!

let arr = [1, 2, 3, 4];
let roArr: ReadonlyArray<number> = arr;
// roArr[0] = 12;  // Err!
// roArr.length = 12; // Err!
// roArr.push(12); // Err!
// arr = roArr // Err!
arr = roArr as number[]  // type assertion, okay
```

#### Point 3: String Index Signature

Add a string index signature if the object can have some extra properties  

```typescript
interface SquareConfig {
    color?: string,
    width: number,
    height: number,
    [prop: string]: any   // Receive any number of props
}

function createSquare(conf: SquareConfig): void { }

// okay
createSquare( { width: 100, height: 123, aProp: [1, 2], bProp: [3, 4] });
```

#### Point 4: Function Types

```typescript
interface cmpFunc {
    (a: number, b: number): boolean   // Function
};

let myCmpFunc: cmpFunc = function(x: number, y: number): boolean {
    return x > y;
}
```

#### Point 5: Indexable Types

```typescript
interface StringArray {
    [index: number]: string
}
interface StringOrNumberArray {
    [index: number]: string | number    // Union
}
let arr: StringArray = ["AA", "BB"];
// let arr2: StringArray = [1, 2]; // Err!
let str: string = arr[1];

let arr3: StringOrNumberArray = ["AA", 12];
// let num: number = arr3[1];   // Err!
```

#### Point 6: Class Types

```typescript
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;  // Constructor
}
interface ClockInterface {
    tick(): void
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log("beep beep");
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log("tick tick")
    }
}

function createClock(obj: ClockConstructor, h: number, m: number): ClockInterface {
    return new obj(h, m);
}

let obj1 = createClock(DigitalClock, 1, 2);
let obj2 = createClock(AnalogClock, 3, 4);
```

#### Point 7: Hybrid Types

```typescript
interface Counter {
    (start: number): string,
    interval: number,
    reset(): void
};

function getCounter(): Counter {
    let obj = (function (start: number) {}) as Counter;
    obj.interval = 123;
    obj.reset = function() {};
    return obj;
}

let a = getCounter();
let counterName: string = a(10);
a.interval = 156;
a.reset();
```

&nbsp;

### Function

#### Basic Function Type

```typescript
function add1(x: number, y:number): number {
    return x + y;
}

let add2 = function(x: number, y: number): number {
    return x + y;
};

let add3: (x: number, y:number) => number =
    function(x: number, y: number): number {
        return x + y;
    };

let out : string = `Res: ${add1(1, 2)} ${add2(1, 2)} ${add3(1, 2)}`;
console.log(out);
```

#### Inferring the types

For example, the typescript compiler can figure out the type of `function(x, y)` in function `add2`  

```typescript
let add1: (x: number, y:number) => number =
    function(x: number, y: number): number {
        return x + y;
    };

let add2: (x: number, y:number) => number =
    function(x, y) {
        return x + y;
    };

let x : number = add1(1, 2); // okay
let y : number = add2(1, 2); // okay
// let z : number = add2("1", 2);  //ERR!
```

#### Optional and Default Parameters

Optional parameters are metioned above, and default parameters are similar to those in C++. In addition, when the user does not pass the value to the optional parameter, the value of the parameter is `undefined`  

```typescript
function buildName(firstName: string, lastName?: string) {
    return lastName ? `${firstName} ${lastName}` : `${firstName}`;
}

function buildName2(firstName: string, lastName: string = "HHH") {
    return `${firstName} ${lastName}`;
}

function buildName3(firstName: string = "HHH", lastName: string) {
    return `${firstName} ${lastName}`;
}

let s1 = buildName("aaa", "bbb");   //"aaa bbb"
let s2 = buildName("aaa");  //"aaa"
let s3 = buildName2("aaa", "bbb");  //"aaa bbb"
let s4 = buildName2("aaa"); //"aaa HHH"
let s5 = buildName2("aaa", undefined);  //"aaa HHH"
//let s6 = buildName3("aaa"); // ERR! Too few arguments
let s7 = buildName3("aaa", "bbb");  //"aaa bbb"
let s8 = buildName3(undefined, "bbb");  //"HHH bbb"
```

#### Rest Parameters

The rest parameters can deal with the function with unknown number of the parameters  

```typescript
function buildName(firstName: string, ...restName: string[]) {
    return firstName + " " + restName.join(" ");
}

let s1 = buildName("aaa", "bbb", "ccc", "ddd"); //"aaa bbb ccc ddd"
```

#### `this`

##### `this` and arrow functions

The example is simplified from official guide. In ES6, arrow functions capture the `this` where the function is created rather than where it is invoked.  

```typescript
/*
// ERR!

let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    createCardPicker: function() {
        return function() {
            return {suit: this.suits[0]};
        }
    }
}
*/

let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    createCardPicker: function() {
        return () => {
            return {suit: this.suits[0]};
        }
    }
}
```

##### `this` parameters

```typescript
interface Card {
    suit: string
};

interface Deck {
    suits: string[],
    createCardPicker(this: Deck): () => Card;
}

let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    createCardPicker: function(this: Deck) {
        return (): Card => {
            return {suit: this.suits[0]};
        }
    }
}
```

#### Overload

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: any, b: any): any {
    if(typeof a == "number" && typeof b == "number") {
        return a + b;
    } else if(typeof a == "string" && typeof b == "string") {
        return a + b;
    } else {
        return null;
    }
}

add(1, 2);
add("1", "2");
// add(1, "2");    // ERR!
```

&nbsp;

### Generics

#### Basic of Generics

The concept of **generics** is similar with the most popular language.  

```typescript
function fun<T>(arg: T): T {
    return arg;
}
let x = fun<string>("str");
let y = fun<number>(1);
// let z = fun<string>(1); // ERR!
```
