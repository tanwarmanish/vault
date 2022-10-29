## Introduction
- Everything in Javascript happens inside an Execution Context.

| **Memory ( Variable Enviornment )** | **Code ( Thread of Execution )** |
| ----------------------------- | -------------------------- |
| key: value                    | --------------             |
| fn: {}                        | --------------             |

- Javascript is a Synchronous, Single threaded language.
- Two phases:
	- Memory Creation Phase
	- Code Execution Phase

- Managed via Call Stack

| **Call Stack** | Description                 |
| -------------- | --------------------------- |
| \              | <- Local Execution Context  |
| EC             | <- Local Execution Context  |
| GEC            | <- Global Execution Context |

- **Call Stack maintains the order of execution of execution contexts.
- Call Stack is also called :
	- Execution Context Stack
	- Program Stack
	- Control Stack
	- Runtime Stack
	- Machine Stack


### Hoisting
- Functions and Variables are accessible before declaration.
- Variables return undefined
- Functions are useable.

### Undefined vs. Not-defined
- **undefined** 
	- memory allocated by value assigned.
- **not-defined**
	- Variable does not exist.

### Scope Chaining

> Lexical Environment = Local Memory + Lexical Environment of Parent

- Chain of lexical environments is called Scope Chain.

### Let & Const
- **let** & **const** declarations are also hoisted.
- But are in Temporal Dead Zone initially ( Stored in a seperate location other than window object )
- They are also not attached to window object.

### Block & Scope
``` Javascript
{ 
	// example of block 
}
```

- Group of multiple statments (enclosed inside -> **{  }** )=> block
- Variables and Functions accessible inside a block is known as its Scope.
- Variables declared with let & const are declared in a seperate memory scope (Block Scope).

### Shadowing
```Javascript
var a = 100;
let b = 100;
{
	var a = 10;  // -- line 3
	let b = 10;
	console.log(a); // 10
	console.log(b); // 10
}
console.log(a); // 10; Shadowed by line 3
console.log(b); // 100
```
- **var** is function scoped.
- **let** & **const** are block scoped.

### Closures
- A function along with its Lexical Scope forms a Closure.
- Uses:
	- Module Design Pattern
	- Currying
	- Memoize
	- State in Async 
	- setTimeout
	- Iterators

### First Class Functions
``` Javascript
// Function Statement (or Function Declaration)
function fn(){
	// code here
}


// Function Expression
let f = function(){
 // code here
}


// Anonymous Function (function without a name)
function(){
 // code here
}


// Named Function Expression
let x = function abc(){
 // code here
 // function accisible with name inside the function only
}


// Parameters & Arguments
function abc(param1,param2){
	// declaration
	// code here
}
abc(arg1,arg2); // invoking


// First Class Function
 /**
  The ability of functions to be used as value and
to be passed as argument to another function and can be returned
from a function is called First Class Function
**/


// Arrow Function
```
- Function Statement and Function Expression both are ways to create a function. Difference between them is Hoisting.
- Anonymouse functions are used in a place where we want to use them as values.
- The ability of function to be used as:
	- Value
	- Passed as argument to another function
	- Returned from a function
This ability is called First Class Function or First Class Citizen.


### Event Loop
- Call Stack executes everything one by one (Sync.)
- Micro Task Queue is similar to Callback Queue, but It has Higher Priority. (eg. Promise Callbacks, Mutation Observer)
- Callback Queue is also known as Task Queue.

![Event Loop](assets/languages/javascript/js_event_loop.png)

- Scenarios such as Callbacks in Microtask Queue are a lot this leads to situation known as Starvation.

### JS Engine
- Javascript Runtime Enviornment = JS Engine + API
- JS Engines :
	- Chakra (Edge)
	- Spider Monkey (Firefox)
	- V8 (Chrome)
- Phases of Execution:
	- Parsing 
		- Code is broken down into tokens
		- Syntax Parser converts code into AST (Abstract Syntax Tree)
	- Compilation
		- JIT (Just In Time Compilation)
		- Interpretter converts AST to byte code, and takes help of Compiler to optimize it.
	- Execution
		- Execution is facilitated by Memory Heap and Call Stack.
![JS Engine](assets/languages/javascript/js_engine.png)

- Garabage Collector monitors Memory Heap to clear out memory (using Mark & Sweep Algorithm)
- **TODO**
	- Mark & Sweep Algorithm
	- Inlining
	- Copy Elision
	- Inline Caching
	
![JS Engine V8](assets/languages/javascript/js_engine_v8.png)


### Higher Order Functions
- A function that takes a function as an agrument or returns a function is known as a Higher Order Function.

### Callbacks
- A function passed as an argument to another function is called Callback Function.
```Javascript
function x(param){
	param();
}

function y(){
	// code
}

x(y); // here y is a callback function
```

- Issues with Callbacks:
	- Callback Hell (Pyramid of Doom)
	- Inversion Control (Control of code lies with Callback Function)
