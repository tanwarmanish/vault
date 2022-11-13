### Installing

``` bash
npm install -g typescript

# or 
yarn add global typescript
```


### Compiling

``` bash
tsc filename.ts
```


### Intro
- Static type system.
- Superset of JS
- Provides optional static typing
- Types act as documentation

### tsconfig.json

``` tsconfig.json
{
	"compilerOptions":{
		"target": "es5",
		"module": "commonjs",
		"strict": true,
		"outDir": "dist"
	}
}
```

``` bash
# enable watch mode
tsc -watch
```


### Setup webpack

```webpack.config.js
module.exports = {
	entry: './src/app.ts',
	output: {
		filename: 'app.js',
		path: __dirname + './dist'
	},
	resolve: {
		extensions: ['.ts','.js']
	},
	module:{
		rules: [
			{ test: /\.ts$/, use: 'awesome-typescript-loader' }
		]
	},
	devServer: {
		port: 3000
	}
}
```


### Number Type

```Typescript
// explicit type
let cost: number = 10;

// implicit type
let cost = 10;
```

### Function Type & Arguments

```Typescript
function calculate(cost:number,count:number):number{
	return cost * count;
}
```


### String Type
```Typescript
let coupon:string = 'this is a string';
let coupon:string = "this is a string";
let coupon:string = `this is a string`;
```

### Boolean Type
``` Typescript
let flag:boolean = true;
```

## Typescript Types

#### 1. "any"
- Can take any datatype.

#### 2. "void"
- Means there is no type at all.
- Used as return type of functions. (means function returns nothing).

#### 3. "never"
- used as function return type
- used when function never returns anthing. (i.e no return statement)

#### 4. "null" / "undefined"
```tsconfing.json
{ 
	strictNullCheck:true,
}
```
``` typescript
let name:string = 'NAME';
let user:string | null = 'NAME';

name = null; // throws error
name = undefined; // throws error
user = null; // works
```


#### 5. Union & Literal Type
- Either of the given types.
``` typescript
let size:string = 'small';
function selectSize(newSize:'small' | 'medium'):void{
	size = newSize;
}
```

#### 6. "Function"
```typescript
let sumOrder:Function;
let sumOrder : (price:number,quantity:number)=>number;

// optional argument
let sumOrder: (price:number,quantity?:number)=>number;
```

#### 7. Object Type
```typescript
let user: {name:string, id:number, getName():string};
```

#### 8. Array Type
```typescript
let sizes:string[];
```

#### 9. Generic Type
```typescript
let sizes: Array<string>
```

#### 10. Typle Types
```typescript
let user: [string,number,boolean];
user = ["Tanwar",101,true];
// order matters here
```


#### 11. Type Alias
``` typescript
type Size = 'small' | 'medium' | 'large';
let size:Size = 'small';
size = 'xlarge'; // error

// assertion
<Size> size;
// or
size as Size;
```


#### 12. Interface
```typescript
interface Name {
	firstName:string;
	lastName:string;
	fullname:()=>string;
}

// extending interfaces
interface User extends Name{
	id:number;
}

let user = {
	id: 99,
	firstName: 'Manish',
	lastName: 'Tanwar'
}

// optional property
interface Admin {
	name:string;
	id:number;
	gender?:string;
}

// index signatures
interface cart {
	[key:string] : any;
}
```


#### 13. Class


### Type Queries
#### 1. "typeof"
``` typescript
const person : {
	name: 'Manish',
	age: 23
};

type Person = typeof person; // Person will be now of type person
```