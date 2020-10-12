

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


