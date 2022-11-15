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

#### 2. "keyof"
```typescript
const person = {
	name: "Manish",
	age: 23
}

type Person = typeof person;
type PersonKeys = keyof Person; // "name" | "age"

type PersonTypes = Person[PersonKeys]; // string | number

// 2. generic types
function getProperty <T, K extends keyof T>(obj : T, key: K){
	// T & K are placeholder types defined here (sortof variables)
	return obj[key];
}

const personName = getProperty(person,'name');
const personAge = getProperty(person,'age2'); //error
```


### Mapped Type
#### 1. "Readonly" Mapped Type
``` typescript

// problem
interface Person {
	name: string;
	age: number;
}

interface ReadonlyPerson {
	readonly name: string;
	readonly age: number;
}

const person: Person = ...;

function freezePerson(person:Person):ReadonlyPerson{
	return Object.freeze(person);
}

// solution
// using generics & mapped Types
function freeze<T>(person: T): Readonly<T>{
	return Object.freeze(obj);
}


// custom readonly
type MyReadOnly<T> = {
	readonly [P in keyof T]: T[P]
}
```


#### 2. "Partial" Mapped Type
``` typescript

// problem
interface Person {
	name: string;
	age: number;
}

interface PartialPerson{
	name?: string;
	age?: string;
}

function updatePerson(person:Person,prop PartialPerson){
	return {...person, ...prop};
}

let person:Person = ...;
updatePerson(person, {name:'ABC'});

// solution
function updatePerson(person:Person,prop:Partial<Person>){
	return {...person, ...prop};
}

// custom
type MyPartial<T> = {
	[P in keyof T]? : T[P]
}
function updatePerson(person:Person, prop:MyPartial<Person>){
	return {...person, ...prop};
}
```

#### 3. "Required" Mapped Type, +/- Modifiers
``` typescript
// opposite of partial

// problem
interface Person {
	name: string;
	age?: number;
}

function printAge(person:Person){
	retunr `${person.name} is ${person.age}`
}

let person:Person = ...;
printAge(person);

// solution
const person: Required<Person> = ...;

// custom
type MyRequired<t> = {
	[P in keyof T]?: T[P] // add prop as optional
	[P in keyof T]+?: T[P] // same as above
	[P in keyof T]-?: T[P] // make everything required
	+readonly [P in keyof T]-?: T[P]
}
```

#### 4. "Pick" Mapped Type
```typescript
// similar to pluck in lodash
interace Person {
	name: string;
	age: number;
	address: {}
}

const person = {
	name: 'Manish',
	age: 23
	// error since address missing
}

//solution
const person: Pick<Person, 'name'|'age'> = {
	name: 'Manish',
	age: 23
} // works now

// custom
type MyPick<T,K extends keyof T> = {
	[P in K]: T[P]
}
```


#### 5.  "Record" Mapped Type
```Typescript
let dict: { [key:string]:any } = {};

interface TrackStates {
	current: string;
	next: string;
}

const item: TrackStates = {
	current: 'json2012',
	next: 'a8s8f9s'
}

// numbers are converted to string (keys)
dict[0] = item;


// another way
let dict:Record<string,TrackStates> = {};
let item: Record<keyof TrackStates, string> = {
	current: 'asdasd',
	next: 'asdasdasd'
}
```


### Type Guards
#### 1. "typeof" and Type Guard
```typescript
function foo(bar: string | number){
	if(typeof bar === 'string'){
		// string
		bar.toUpperCase();
		return;
	}
	//number
	bar
}

```

#### 2. "instanceof" and Type Guard
``` typescript
item instanceOf Song;
```

#### 3. User Defined Type Guards
```typescript
function isSong(item:any): item is Song{
	return item instanceof Song;
	// means when true the item is Song
}
if(isSong){
	item.title; // works
}
```

#### 4. "in" Type Guards 
``` typescript
function isSong(item:any):item is Song{
	return 'title' in item
}
if(isSong(item)){
	item.title; //works
}
```


