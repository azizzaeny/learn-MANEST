

### Installing 
to install typescript we need node.js first installed

```
npm install -g typescript
npm install --save-dev typescript
```


### Compiling & building first typescript 

```sh
mkdir -p ../dist/typescript-essential/
cd ../dist/typescript-essential/
```

```ts
// :file=../dist/typescript-essentials/greeter.ts    

function greeter(person: string){
  return "Hello, "+ person;
}

let user = "Jane User";

document.body.textContent = greeter(user);
```

Trying wrong type of input  

```ts :file=../dist/typescript-essentials/greeter.ts   

function greeter(person :string){
  return "Hello, "+ person;
}

let user = [0,1,2];

document.body.textContent= greeter(user);
```

Input type interfaces  

```ts :file=./dist/typescript-essentials/greeter.ts  

interface Person {
  firstName: string;
  lastName: string;
}

function greeter(person: Person){
	return "Hello, " +person.firstName + "" +person.lastName;
}

let user = { firstName: "Jane", lastName: "User"};

document.body.textContent = greeter(user);
```

Try to use Input type Classes  

```typescript :file=../dist/typescript-essentials/greeter.ts  

class Student {
	fullName: string;
	constructor(
		public firstName: string,
		public middleInitial: string,
		public lastName: string
	) {
		this.fullName = firstName + " "+middleInitial+ " "+lastName;
	}
}

interface Person{
  firstName: string;
  lastName: string;
}

function greeter(person: Person){
  return "Hello, "+ person.firstName+ " "+person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.textContent = greeter(user);
```

Basic template for running in web html  

```html :file=../dist/typescript-essentials/greeter.html  

<!DOCTYPE html>
<html>
  <head>
	<title> Typescript Greeter </title>
  </head>
  <body>
	<script src="greeter.js"></script>
  </body>
</html>

```

Compiling greeter.ts  

```sh
tsc ../dist/typescript-essentials/greeter.ts
node ../dist/typescript-essentials/greeter.js
```

spin up simple http server

```sh
cd ../dist/typescript-essentials/
python -m SimpleHTTPServer
```

running in repl  

```sh
npm install -g ts-node
ts-node
```

### Basic Types  

```txt
Boolean
Number
String
Array 
Tupple
Enum
Unknown
Any
Void
Null
Undefined
Never
Object
assertions
```
quick example play around types:   

```ts 
let isDone: boolean = false;

let decimal: number = 6;
let hex: number 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;

let color: string = "blue";
color="red";

let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${fullName}. 
Ill be ${age + 1} years old next month.`;

let sentence: string =
	"Hello, my name is " +
	fullName +
	".\n\n" +
	"I'll be " +
	(age + 1) +
	" years old next month.";

let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
let x: [string, number];

x = ["hello", 10]; // OK
x = [10, "hello"]; // Error

console.log(x[0].substring(1));
console.log(x[1].substring(1));

x[3] = "world";

enum Color {
	Red,
	Green,
	Blue,
}

let c: Color = Color.Green;

let c: Color = Color.Green;

Color.Red = 0;
let colorName: string = Color[2];
colorName;

let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;

declare const maybe: unknown;
// 'maybe' could be a string, object, boolean, undefined, or other types
const aNumber: number = maybe;


if (maybe === true) {
	// TypeScript knows that maybe is a boolean now
	const aBoolean: boolean = maybe;
	// So, it cannot be a string
	const aString: string = maybe;
	Type 'boolean' is not assignable to type 'string'.
}

if (typeof maybe === "string") {
	// TypeScript knows that maybe is a string
	const aString: string = maybe;
	// So, it cannot be a boolean
	const aBoolean: boolean = maybe;
	Type 'string' is not assignable to type 'boolean'.
}


declare function getValue(key: string): any;
// OK, return value of 'getValue' is not checked
const str: string = getValue("myString");

let looselyTyped: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
// OK, toFixed exists (but the compiler doesn't check)
looselyTyped.toFixed();

let strictlyTyped: unknown = 4;
strictlyTyped.toFixed();

let looselyTyped: any = {};
let d = looselyTyped.a.b.c.d;
//  ^ = let d: any

function warnUser(): void {
	console.log("This is my warning message");
}

let unusable: void = undefined;
// OK if `--strictNullChecks` is not given
unusable = null;

// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;

// Function returning never must not have a reachable end point
function error(message: string): never {
	throw new Error(message);
}

// Inferred return type is never
function fail() {
	return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
	while (true) {}
}

declare function create(o: object | null): void;

// OK
create({ prop: 0 });
create(null);
create(42);
// Argument of type '42' is not assignable to parameter of type 'object | null'.
create("string");
create(false);

// Argument of type 'false' is not assignable to parameter of type 'object | null'.
create(undefined);
// Argument of type 'undefined' is not assignable to parameter of type 'object | null'.

let someValue: unknown = "this is a string";

let strLength: number = (someValue as string).length;

let someValue: unknown = "this is a string";

let strLength: number = (<string>someValue).length;

function reverse(s: String): String {
	return s.split("").reverse().join("");
}

reverse("hello world");

function reverse(s: string): string {
	return s.split("").reverse().join("");
}

reverse("hello world");

```

### Interfaces and Classes

```
optional properties
read-only properties
ReadonlyArray 
```

```ts :file=../dist/interfaces.ts
interface SquareConfig {
	color?: string;
	width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
	let newSquare = { color: "white", area: 100 };
	if (config.color) {
		newSquare.color = config.color;
	}
	if (config.width) {
		newSquare.area = config.width * config.width;
	}
	return newSquare;
}

let mySquare = createSquare({ color: "black" });
console.log(mySquare);
```

Declare functions type in interfaces   

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;

mySearch = function (source: string, subString: string){
  let result = src.search(sub);
  return result > -1;
}
```

Class Type declaratioin in interfaces  

```ts
interface ClockInterface {
  currentTime: Date;
}

class Clock implements ClockInterface{
  currentTime: Date = new Date();
  constructor(h: number, m: number){}
}
```

Type a Function  


```ts
// The parameters 'x' and 'y' have the type number
let myAdd = function (x: number, y: number): number {
  return x + y;
};

// myAdd has the full function type
let myAdd2: (baseValue: number, increment: number) => number = function (x, y) {
  return x + y;
};

// optional arguments
function buildName(firstName: string, lastName?: string) {
  if (lastName) return firstName + " " + lastName;
  else return firstName;
}

// rest params
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}


let deck = {
  suits: ["hearts", "spades", "clubs", "diamonds"],
  cards: Array(52),
  createCardPicker: function () {
    return function () {
      let pickedCard = Math.floor(Math.random() * 52);
      let pickedSuit = Math.floor(pickedCard / 13);

      return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
    };
  },
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);

interface Card {
  suit: string;
  card: number;
}

interface Deck {
  suits: string[];
  cards: number[];
  createCardPicker(this: Deck): () => Card;
}

interface UIElement {
  addClickListener(onclick: (this: void, e: Event) => void): void;
}
```

Argument of types  


```ts
type Easing = "ease-in" | "ease-out" | "ease-in-out";

class UIElement {
  animate(dx: number, dy: number, easing: Easing) {
    if (easing === "ease-in") {
      // ...
    } else if (easing === "ease-out") {
    } else if (easing === "ease-in-out") {
    } else {
      // It's possible that someone could reach this
      // by ignoring your types though.
    }
  }
}

let button = new UIElement();
button.animate(0, 0, "ease-in");
button.animate(0, 0, "uneasy");

// Argument of type '"uneasy"' is not assignable to parameter of type 'Easing'.

function rollDice(): 1 | 2 | 3 | 4 | 5 | 6 {
  return (Math.floor(Math.random() * 6) + 1) as 1 | 2 | 3 | 4 | 5 | 6;
}

const result = rollDice();

interface MapConfig {
  lng: number;
  lat: number;
  tileSize: 8 | 16 | 32;
}

setupMap({ lng: -73.935242, lat: 40.73061, tileSize: 16 });

interface ValidationSuccess {
  isValid: true;
  reason: null;
};

interface ValidationFailure {
  isValid: false;
  reason: string;
};

type ValidationResult =
  | ValidationSuccess
  | ValidationFailure;

interface Bird {
  fly(): void;
  layEggs(): void;
}

interface Fish {
  swim(): void;
  layEggs(): void;
}

declare function getSmallPet(): Fish | Bird;

let pet = getSmallPet();
pet.layEggs();
```

### Type Classes, Enums and Generics  
Classes  

```ts
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

class Animal {
	move(distanceInMeters: number = 0) {
		console.log(`Animal moved ${distanceInMeters}m.`);
	}
}

class Dog extends Animal {
	bark() {
		console.log("Woof! Woof!");
	}
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

Enums Declaration   

```ts
enum Direction {
	Up = 1,
	Down,
	Left,
	Right
}

enum Direction {
	Up,
	Down,
	Left,
	Right
}

enum UserResponse {
	No = 0,
	Yes = 1
}

function respond(recipient: string, message: UserResponse): void {
	// ...
}

respond("Princess Caroline", UserResponse.Yes);

enum Direction {
	Up = "UP",
	Down = "DOWN",
	Left = "LEFT",
	Right = "RIGHT"
}

// enums value string


enum E1 {
	X,
	Y,
	Z
}

enum E2 {
	A = 1,
	B,
	C
}
// constant value
```

Generic

```ts
function identity(arg: number): number {
	return arg;
}

function identity(arg: any): any {
	return arg;
}

function identity<T>(arg: T): T {
	return arg;
}

let output = identity<string>("myString");
//       ^ = let output: string

let output = identity<string>("myString");
//       ^ = let output: string

function loggingIdentity<T>(arg: Array<T>): Array<T> {
	console.log(arg.length); // Array has a .length, so no more error
	return arg;
}
```

### Advanced Types   

type predicate   

```ts
function isFish(pet: Fish | Bird): pet is Fish {
	return (pet as Fish).swim !== undefined;
}
// Both calls to 'swim' and 'fly' are now okay.
let pet = getSmallPet();

if (isFish(pet)) {
	pet.swim();
} else {
	pet.fly();
}

// quicker using in operator
function move(pet: Fish | Bird) {
	if ("swim" in pet) {
		return pet.swim();
	}
	return pet.fly();
}

// using type guards
function isNumber(x: any): x is number {
	return typeof x === "number";
}

function isString(x: any): x is string {
	return typeof x === "string";
}

function padLeft(value: string, padding: string | number) {
	if (isNumber(padding)) {
		return Array(padding + 1).join(" ") + value;
	}
	if (isString(padding)) {
		return padding + value;
	}
	throw new Error(`Expected string or number, got '${padding}'.`);
}

// interfaceof

interface Padder {
	getPaddingString(): string;
}

class SpaceRepeatingPadder implements Padder {
	constructor(private numSpaces: number) {}
	getPaddingString() {
		return Array(this.numSpaces + 1).join(" ");
	}
}

class StringPadder implements Padder {
	constructor(private value: string) {}
	getPaddingString() {
		return this.value;
	}
}

function getRandomPadder() {
	return Math.random() < 0.5
		? new SpaceRepeatingPadder(4)
		: new StringPadder("  ");
}

let padder: Padder = getRandomPadder();
//       ^ = let padder: Padder

if (padder instanceof SpaceRepeatingPadder) {
	padder;
	//       ^ = Could not get LSP result: er;>
	//	<  /
}
if (padder instanceof StringPadder) {
	padder;
	//       ^ = Could not get LSP result: er;>
	//	<  /
}
```

assign null types or undefined types    

```ts
let exampleString = "foo";
exampleString = null;
//Type 'null' is not assignable to type 'string'.

let stringOrNull: string | null = "bar";

stringOrNull = null;

stringOrNull = undefined;
//Type 'undefined' is not assignable to type 'string | null'.

// should with optional parameters --strictNullChecks
function f(x: number, y?: number) {
	return x + (y ?? 0);
}

f(1, 2);
f(1);
f(1, undefined);
f(1, null);

class C {
	a: number;
	b?: number;
}

let c = new C();

c.a = 12;
c.a = undefined;
// Type 'undefined' is not assignable to type 'number'.
c.b = 13;
c.b = undefined;
c.b = null;
// Type 'null' is not assignable to type 'number | undefined'.
```


type aliases  

```ts
type Second = number;
let timeInSecond: number = 10;
let time: Second = 10;

type Container<T> = { value: T };

type Tree<T> = {
	value: T;
	left?: Tree<T>;
	right?: Tree<T>;
};

type LinkedList<Type> = Type & { next: LinkedList<Type> };

interface Person {
	name: string;
}

let people = getDriversLicenseQueue();
people.name;
people.next.name;
people.next.next.name;
people.next.next.next.name;
//                  ^ = (property) next: LinkedList
```


Difference between interface and types is that we cannot extend types, when we redeclare interfaces it would extend previous one.  

```ts
interface Window {
	title: string
}

interface Window {
	ts: import("typescript")
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});

type Window = {
	title: string
}

type Window = {
	ts: import("typescript")
}

// Error: Duplicate identifier 'Window'.

```
### Advanced types  
Index types, keyof   
```ts
function pluck<T, K extends keyof T>(o: T, propertyNames: K[]): T[K][] {
	return propertyNames.map((n) => o[n]);
}

interface Car {
	manufacturer: string;
	model: string;
	year: number;
}

let taxi: Car = {
	manufacturer: "Toyota",
	model: "Camry",
	year: 2014,
};

// Manufacturer and model are both of type string,
// so we can pluck them both into a typed string array
let makeAndModel: string[] = pluck(taxi, ["manufacturer", "model"]);

// If we try to pluck model and year, we get an
// array of a union type: (string | number)[]
let modelYear = pluck(taxi, ["model", "year"]);

function getProperty<T, K extends keyof T>(o: T, propertyName: K): T[K] {
	return o[propertyName]; // o[propertyName] is of type T[K]
}
```

mapped types   
```ts
type Partial<T> = {
	[P in keyof T]?: T[P];
};

type Readonly<T> = {
	readonly [P in keyof T]: T[P];
};

// to use it
type PersonPartial = Partial<Person>;
//   ^ = type PersonPartial = {
//  name?: string | undefined;
//  age?: number | undefined;
// }
type ReadonlyPerson = Readonly<Person>;
//   ^ = type ReadonlyPerson = {
// readonly name: string;
// readonly age: number;
//}

// wrapped types
type Proxy<T> = {
	get(): T;
	set(value: T): void;
};

type Proxify<T> = {
	[P in keyof T]: Proxy<T[P]>;
};

function proxify<T>(o: T): Proxify<T> {
	// ... wrap proxies ...
}

let props = { rooms: 4 };
let proxyProps = proxify(props);
//  ^ = let proxyProps: Proxify<{
//          rooms: number;
// }>
	
```

Conditional Types   

```ts 
//T extends U ? X : Y

declare function f<T extends boolean>(x: T): T extends true ? string : number;

// Type is 'string | number'
let x = f(Math.random() < 0.5);
//  ^ = let x: string | number
```

### Utility types
```ts
// partial
interface Todo {
	title: string;
	description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
	return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
	title: "organize desk",
	description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
	description: "throw out trash",
});

// readonly
interface Todo {
	title: string;
}

const todo: Readonly<Todo> = {
	title: "Delete inactive users",
};

todo.title = "Hello";
// Cannot assign to 'title' because it is a read-only property.


// Object.freeze is also readonly
function freeze<Type>(obj: Type): Readonly<Type>;

// records
interface PageInfo {
	title: string;
}

type Page = "home" | "about" | "contact";

const nav: Record<Page, PageInfo> = {
	about: { title: "about" },
	contact: { title: "contact" },
	home: { title: "home" },
};

nav.about;
// ^ = const nav: Record

// pick

interface Todo {
	title: string;
	description: string;
	completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
	title: "Clean room",
	completed: false,
};

todo;
// ^ = const todo: Pick


// omit

interface Todo {
	title: string;
	description: string;
	completed: boolean;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
	title: "Clean room",
	completed: false,
};

todo;
// ^ = const todo: Pick

// exclude 
type T0 = Exclude<"a" | "b" | "c", "a">;
//    ^ = type T0 = "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;
//    ^ = type T1 = "c"
type T2 = Exclude<string | number | (() => void), Function>;
//    ^ = type T2 = string | number

// extract
type T0 = Extract<"a" | "b" | "c", "a" | "f">;
//    ^ = type T0 = "a"
type T1 = Extract<string | number | (() => void), Function>;
//    ^ = type T1 = () => void

// nonnullable
type T0 = NonNullable<string | number | undefined>;
//    ^ = type T0 = string | number
type T1 = NonNullable<string[] | null | undefined>;
//    ^ = type T1 = string[]

// parameters 
declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>;
//    ^ = type T0 = []
type T1 = Parameters<(s: string) => void>;
//    ^ = type T1 = [s: string]
type T2 = Parameters<<T>(arg: T) => T>;
//    ^ = type T2 = [arg: unknown]
type T3 = Parameters<typeof f1>;
//    ^ = type T3 = [arg: {
//       a: number;
//       b: string;
//      }]
type T4 = Parameters<any>;
//    ^ = type T4 = unknown[]
type T5 = Parameters<never>;
//    ^ = type T5 = never


// instanceType
class C {
	x = 0;
	y = 0;
}

type T0 = InstanceType<typeof C>;
//    ^ = type T0 = C
type T1 = InstanceType<any>;
//    ^ = type T1 = any
type T2 = InstanceType<never>;
//    ^ = type T2 = never
type T3 = InstanceType<string>

// required
interface Props {
	a?: number;
	b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5 };
//Property 'b' is missing in type '{ a: number; }' but required in type 'Required<Props>'.

// this parameter type
function toHex(this: Number) {
	return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
	return toHex.apply(n);
}
```

### Decorator, Module and Mixins

decorator  

enable experimental support for decorator.  
`tsc --target ES5 --experimentalDecorators`

or in tsconfig.json  

```json
{
	"compilerOptions": {
		"target": "ES5",
		"experimentalDecorators": true
	}
}
```

decorator factories  

```ts
function color(value: string) {
	// this is the decorator factory
	return function (target) {
		// this is the decorator
		// do something with 'target' and 'value'...
	};
}

// use @functionName
function sealed(target) {
	// do something with 'target' ...
}
// to use as decorator use @sealed

// multiple decorator declaration
// @f @g x
// resulting composite (f ∘ g)(x) is equivalent to f(g(x)).

function f() {
	console.log("f(): evaluated");
	return function (
		target,
		propertyKey: string,
		descriptor: PropertyDescriptor
	) {
		console.log("f(): called");
	};
}

function g() {
	console.log("g(): evaluated");
	return function (
		target,
		propertyKey: string,
		descriptor: PropertyDescriptor
	) {
		console.log("g(): called");
	};
}

class C {
	@f()
	@g()
	method() {}
}
// evaluation order
// f(): evaluated
// g(): evaluated
// g(): called
// f(): called


// decorator in classes
@sealed
class Greeter {
	greeting: string;
	constructor(message: string) {
		this.greeting = message;
	}
	greet() {
		return "Hello, " + this.greeting;
	}
}

// definition @sealed
function sealed(constructor: Function) {
	Object.seal(constructor);
	Object.seal(constructor.prototype);
}

function classDecorator<T extends { new (...args: any[]): {} }>(
	constructor: T
) {
	return class extends constructor {
		newProperty = "new property";
		hello = "override";
	};
}

@classDecorator
class Greeter {
	property = "property";
	hello: string;
	constructor(m: string) {
		this.hello = m;
	}
}

console.log(new Greeter("world"));

// method decorator
class Greeter {
	greeting: string;
	constructor(message: string) {
		this.greeting = message;
	}

	@enumerable(false)
	greet() {
		return "Hello, " + this.greeting;
	}
}

function enumerable(value: boolean) {
	return function (
		target: any,
		propertyKey: string,
		descriptor: PropertyDescriptor
	) {
		descriptor.enumerable = value;
	};
}

// parameter decorators
class Greeter {
	greeting: string;

	constructor(message: string) {
		this.greeting = message;
	}

	@validate
	greet(@required name: string) {
		return "Hello " + name + ", " + this.greeting;
	}
}
// use 'reflect-metadata' module
// tsc --target ES5 --experimentalDecorators --emitDecoratorMetadata
// import "reflect-metadata";

```

Namespaces 
namespaces are not merged unlike interfaces

```ts  

namespace Animal {
	let haveMuscles = true;

	export function animalsHaveMuscles() {
		return haveMuscles;
	}
} 

namespace Animal {
	export function doAnimalsHaveMuscles() {
		return haveMuscles; // Error, because haveMuscles is not accessible here
	}
}
```

Iterators and Generators  

```ts  
// for of
let someArray = [1, "string", false];

for (let entry of someArray) {
	console.log(entry); // 1, "string", false
}

let list = [4, 5, 6];

for (let i in list) {
	console.log(i); // "0", "1", "2",
}

for (let i of list) {
	console.log(i); // "4", "5", "6"
}
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";

for (let pet in pets) {
	console.log(pet); // "species"
}

for (let pet of pets) {
	console.log(pet); // "Cat", "Dog", "Hamster"
}
```

Mixins   

```ts
class Sprite {
	name = "";
	x = 0;
	y = 0;

	constructor(name: string) {
		this.name = name;
	}
}

type Constructor = new (...args: any[]) => {};

function Scale<TBase extends Constructor>(Base: TBase) {
	return class Scaling extends Base {
		// Mixins may not declare private/protected properties
		// however, you can use ES2020 private fields
		_scale = 1;

		setScale(scale: number) {
			this._scale = scale;
		}

		get scale(): number {
			return this._scale;
		}
	};
}

// Compose a new class from the Sprite class,
// with the Mixin Scale applier:
const EightBitSprite = Scale(Sprite);

const flappySprite = new EightBitSprite("Bird");
flappySprite.setScale(0.8);
console.log(flappySprite.scale);

// This was our previous constructor:
type Constructor = new (...args: any[]) => {};
// Now we use a generic version which can apply a constraint on
// the class which this mixin is applied to
type GConstructor<T = {}> = new (...args: any[]) => T;
type Positionable = GConstructor<{ setPos: (x: number, y: number) => void }>;
type Spritable = GConstructor<typeof Sprite>;
type Loggable = GConstructor<{ print: () => void }>;
function Jumpable<TBase extends Positionable>(Base: TBase) {
	return class Jumpable extends Base {
		jump() {
			// This mixin will only work if it is passed a base
			// class which has setPos defined because of the
			// Positionable constraint.
			this.setPos(0, 20);
		}
	};
}

// applying mixins
// Each mixin is a traditional ES class
class Jumpable {
	jump() {}
}

class Duckable {
	duck() {}
}

// Including the base
class Sprite {
	x = 0;
	y = 0;
}

// Then you create an interface which merges
// the expected mixins with the same name as your base
interface Sprite extends Jumpable, Duckable {}
// Apply the mixins into the base class via
// the JS at runtime
applyMixins(Sprite, [Jumpable, Duckable]);

let player = new Sprite();
player.jump();
console.log(player.x, player.y);

// This can live anywhere in your codebase:
function applyMixins(derivedCtor: any, constructors: any[]) {
	constructors.forEach((baseCtor) => {
		Object.getOwnPropertyNames(baseCtor.prototype).forEach((name) => {
			Object.defineProperty(
				derivedCtor.prototype,
				name,
				Object.getOwnPropertyDescriptor(baseCtor.prototype, name)
			);
		});
	});
}
```

Modules  

```ts  

// Exporting
// stringValidator.ts
export interface StringValidator {
	isAcceptable(s: string): boolean;
}
// zipCodeValidator.ts
import { StringValidator } from "./StringValidator";

export const numberRegexp = /^[0-9]+$/;

export class ZipCodeValidator implements StringValidator {
	isAcceptable(s: string) {
		return s.length === 5 && numberRegexp.test(s);
	}
}

class ZipCodeValidator implements StringValidator {
	isAcceptable(s: string) {
		return s.length === 5 && numberRegexp.test(s);
	}
}
export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };

// importing
import { ZipCodeValidator as ZCV } from "./ZipCodeValidator";
let myValidator = new ZCV();

// import entire module aliases
import * as validator from "./ZipCodeValidator";
let myValidator = new validator.ZipCodeValidator();

// sideEffect only
import "./my-module.js";

// importing type

// Re-using the same import
import { APIResponseType } from "./api";

// Explicitly use import type
import type { APIResponseType } from "./api";

// default exporting
declare let $: JQuery;
export default $;

import $ from "jquery";

$("button.continue").html("Next Step...");

// 
export default class ZipCodeValidator {
	static numberRegexp = /^[0-9]+$/;
	isAcceptable(s: string) {
		return s.length === 5 && ZipCodeValidator.numberRegexp.test(s);
	}
}

// export all as
export * as utilities from "./utilities";
// importing all
import { utilities } from "./index";

// import, require
import zip = require("./ZipCodeValidator");

```

### Namespaces, symbols and variable declaration

Concept of namespaces

```ts
// declaring validator in single file
interface StringValidator {
	isAcceptable(s: string): boolean;
}

let lettersRegexp = /^[A-Za-z]+$/;
let numberRegexp = /^[0-9]+$/;

class LettersOnlyValidator implements StringValidator {
	isAcceptable(s: string) {
		return lettersRegexp.test(s);
	}
}

class ZipCodeValidator implements StringValidator {
	isAcceptable(s: string) {
		return s.length === 5 && numberRegexp.test(s);
	}
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
	for (let name in validators) {
		let isMatch = validators[name].isAcceptable(s);
		console.log(`'${s}' ${isMatch ? "matches" : "does not match"} '${name}'.`);
	}
}

// instead writing all into one global object, we can group them into a namespace

namespace Validation {
	export interface StringValidator {
		isAcceptable(s: string): boolean;
	}

	const lettersRegexp = /^[A-Za-z]+$/;
	const numberRegexp = /^[0-9]+$/;

	export class LettersOnlyValidator implements StringValidator {
		isAcceptable(s: string) {
			return lettersRegexp.test(s);
		}
	}

	export class ZipCodeValidator implements StringValidator {
		isAcceptable(s: string) {
			return s.length === 5 && numberRegexp.test(s);
		}
	}
}

// multi file namespaces

// validation.ts

namespace Validation {
	export interface StringValidator {
		isAcceptable(s: string): boolean;
	}
}

// lettersOnlyValidator.ts
/// <reference path="Validation.ts" />
namespace Validation {
	const lettersRegexp = /^[A-Za-z]+$/;
	export class LettersOnlyValidator implements StringValidator {
		isAcceptable(s: string) {
			return lettersRegexp.test(s);
		}
	}
}
// ZipCodeValidator
/// <reference path="Validation.ts" />
namespace Validation {
	const numberRegexp = /^[0-9]+$/;
	export class ZipCodeValidator implements StringValidator {
		isAcceptable(s: string) {
			return s.length === 5 && numberRegexp.test(s);
		}
	}
}

// test.ts
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />
/// <reference path="ZipCodeValidator.ts" />

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
	for (let name in validators) {
		console.log(
			`"${s}" - ${
validators[name].isAcceptable(s) ? "matches" : "does not match"
} ${name}`
		);
	}
}

// concat all into one
// tsc --outFile sample.js Test.ts

// or declare in ordered top-down
// <script src="Validation.js" type="text/javascript" />
// <script src="LettersOnlyValidator.js" type="text/javascript" />
// <script src="ZipCodeValidator.js" type="text/javascript" />
// <script src="Test.js" type="text/javascript" />

```

Module

```ts
// myModules.d.ts

// In a .d.ts file or .ts file that is not a module:
declare module "SomeModule" {
	export function fn(): string;
}

/// <reference path="myModules.d.ts" />
import * as m from "SomeModule";

```

Symbols  

symbols are unique   

```ts
let sym2 = Symbol("key");
let sym3 = Symbol("key");
sym2 === sym3; // false, symbols are unique
```
