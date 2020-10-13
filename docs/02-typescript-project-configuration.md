
Example tsconfig.  

```json
{
	"compilerOptions":{
		"module": "commonjs",
		"noImplicitAny" : true,
		"removeComments" : true,
		"preserveConstEnums" : true,
		"sourceMap" : true
	},
	"files":[
		"core.ts",
		"sys.ts",
		"types.ts",
		"scanner.ts",
		"parser.ts",
		"utilities.ts",
		"diagnostic.generated.ts"
	]
}
```

Using include and exclude  

```json
{
	"compilerOptions":{
		"module": "system",
		"noImplicitAny": true,
		"removeComments": true,
		"preserveConstEnums": true,
		"outFile" : "../../built/local/tsc.js",
		"sourceMap" : true
	},
	"include":["src/**/*"],
	"exclude":["node_modules", "**/*.spec.ts"]
}
```

Extend tsconfig bases  

```json
{
	"extends": "@tsconfig/node12/tsconfig.json",

	"compilerOptions": {
		"preserveConstEnums": true
	},

	"include": ["src/**/*"],
	"exclude": ["node_modules", "**/*.spec.ts"]
}
```

References  
```txt
https://www.typescriptlang.org/tsconfig
```

tsc cli options  

```sh
# Run a compile based on a backwards look through the fs for a tsconfig.json
tsc

# Transpile just the index.ts with the compiler defaults
tsc index.ts

# Transpile any .ts files in the folder src, with the default settings
tsc src/*.ts

# Transpile any files referenced in with the compiler settings from tsconfig.production.json
tsc --project tsconfig.production.json
```
### example project build tools  

project structure  
```
/src/converter.ts
/src/units.ts
/test/converter-tests.ts
/test/units-tests.ts
/tsconfig.json
```

test files import and do testing  

```ts
import * as converter from "../converter";

assert.areEqual(converter.celsiusToFahrenheit(0), 32)
```

use project references   
```json
{
    "compilerOptions": {
        // The usual
    },
    "references": [
        { "path": "../src" }
    ]
}
```

using build mode  

`tsc --build` or `tsc -b`

```sh
tsc -b                            # Use the tsconfig.json in the current directory
tsc -b src                        # Use src/tsconfig.json
tsc -b foo/prd.tsconfig.json bar  # Use foo/prd.tsconfig.json and bar/tsconfig.json
```

other commandline options.  

```txt
--verbose
--dry
--clean
--force
--watch
```

using babel or other build tools.

```sh
npm install @babel/cli @babel/core @babel/preset-typescript --save-dev
```

`.babelrc`

```json
{
	"presets": ["@babel/preset-typescript"]
}
```

using command line interfaces  

```sh
./node_modules/.bin/babel --out-file bundle.js src/index.ts
```

`package.json`

```json
{
	"scripts": {
		"build": "babel --out-file bundle.js main.ts"
	},
}
```

`npm run build`  


using webpack   

```sh
npm install ts-loader --save-dev
```
webpack.config.js  

```js
module.exports = {
	entry: "./src/index.tsx",
	output: {
		path: "/",
		filename: "bundle.js",
	},
	resolve: {
		extensions: [".tsx", ".ts", ".js", ".json"],
	},
	module: {
		rules: [
			// all files with a '.ts' or '.tsx' extension will be handled by 'ts-loader'
			{ test: /\.tsx?$/, use: ["ts-loader"], exclude: /node_modules/ },
		],
	},
};
```


watching on changes  

`--watch`  

```json 
{
	// Some typical compiler options
	"compilerOptions": {
		"target": "es2020",
		"moduleResolution": "node"
		// ...
	},

	// NEW: Options for file/directory watching
	"watchOptions": {
		// Use native file system events for files and directories
		"watchFile": "useFsEvents",
		"watchDirectory": "useFsEvents",

		// Poll files for updates more frequently
		// when they're updated a lot.
		"fallbackPolling": "dynamicPriority"
	}
}
```
