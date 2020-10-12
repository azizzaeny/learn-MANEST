

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

### Basic Types

