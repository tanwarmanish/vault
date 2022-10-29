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


### Promise
- A Promise is an object representing the eventual completion or failure of an Async Operation. 
- A producing code that does something and takes time. A consuming Code that wants the result of the prior. A promise is a JS object that links the both.
- Promises possible states:
	- Pending (initial state)
	- Fulfilled (completed)
	- Rejected (Failed)
```Javascript
let promise = new Promise(function(resolve,reject){
	// producing code
});
```
- Arguments resolve and reject are callbacks provided by Javascript.
	- **resolve(value)** - if job is finished successfully, with result value.
	- **reject(error)** - if an error has occurred, error is the error object.
- The promise object returned by the new Promise constructor has these properties.
	- **state** :
		- "pending" : Initially
		- "fulfilled" : Resolved
		- "rejected" : Rejected/Failed
	- **result** :
		- "undefined" : Initially
		- **value** : when resolved or rejected.

![Promise](assets/languages/javascript/promise_explained.png)

- A promise that is either resolved or rejected is called "settled".
``` Javascript
let promise = new Promise(function(resolve,reject){
	resolve("done");
	// All further calls of resolve and reject are ignored.
	reject(new Error("...")); // ignored
	resolve("..."); // ignored
});
```

- **state** and **result** are internal and cannot be directly accessed.
- Consuming functions can be registered(subscribed) using the methods .then and .catch.
- **then(f,f)**
	- It runs when promise is resolved and received the result.
	- Arguments :
		- 1st Arg : A function that runs when promise is resolved.
		- 2nd Arg: A function that runs when promise is rejected.
```Javascript
promise.then(
	function(result) { /* handle a successful result */ },
	function(error) { /* handle an error */ }
);
```
- **catch(f)**
	- It runs when promise is rejected.
- **finally(f)**
	- It set up a handler for performing cleanup/finalizing at the end.
	- It runs always in the end.

```Javascript
new Prmose((resolve,reject)=>{
 // code here
})
.then(()=>{})
.catch(()=>{})
.then(()=>{})
.then(()=>{})
.finally(()=>{}) // triggers first
.catch(()=>{}); // .catch shows the error
```
- If finally throws an error, then the execution goes to the nearest error handler.

### Promise Chaining
- To handle sequence of async tasks to perform one after another.
```Javascript
new Promise(function(resolve, reject) { 
	setTimeout(() => resolve(1), 1000); // (*) 
})
.then(function(result) { // (**) 
	alert(result); // 1 
	return result * 2; 
})
.then(function(result) { // (***) 
	alert(result); // 2 
	return result * 2; 
})
.then(function(result) { 
	alert(result); // 4 
	return result * 2; 
});
```
- The result is passed through the chain of .then handlers.

![Promise Chaining](assets/languages/javascript/promise_chaining.png)

### Promise Error Handing
- Promise chains are great at error handling.
- When a promise rejects, the control jumps to the closest rejection handler.

### Promise API
- There are 6 static methods in the Promise class:
	- Promise.all
	- Promise.allSettled
	- Promise.race
	- Promise.any
	- Promise.resolve
	- Promise.reject
	
#### Promise.all
- It allows us to execute many promises in parallel.
- It waits till all of them are ready.
- It takes an iterable (array of promises) and returns a new promise.
- The new promise resolved when all listed promises are resolve and the array of their result becomes its result.
- The order of the results is same as in its source promise.
- If any of the promise is rejected, the promise returned by Promise.all immediately rejects with that error.
- So if one promise rejects, Promise.all immediately rejects. Results of other ones are ignored.
- **NOTE :** Promise.all does nothing to cancel them, if one fails the others will still continue to execute. **AbortController** helps with the cancellation.
- It allows non-promise values in the iterable as well.
```Javascript
let promise = Promise.all(iterable)
```

#### Promise.allSettled
- Similar to Promise.all
- Promise.allSettled waits for all promises to settle, regardless of the result. (resolved or rejected)
``` Javascript
let promise = Promise.allSettled(iterable);


let result = [ 
	{status: 'fulfilled', value: ...response...}, 
	{status: 'fulfilled', value: ...response...}, 
	{status: 'rejected', reason: ...error object...} 
];
```

#### Promise.race
- Similar to Promise.all, but waits only for the first settled promise and gets its result (value or error).
- Promise that settles first becomes the result.
``` Javascript
let promise = Promise.race(iterable);
```

#### Promise.any
- Similar to Promise.race, but waits only for the first fulfilled promise and gets its result.
- If all the given promises are rejected, then the return promise is rejected (AggregateError)
``` Javascript
let promise = Promise.any(iterable);
```

#### Promise.resolve
- Rarely needed (async/await covers it)
- It creates a resolved promise with the result value.
- Method is used for compatibility, when a function is expected to return a promise.
``` Javascript
let promise = Promise.resolve(value);
```

#### Promise.reject
- Rarely needed (async/await covers it)
- It creates a rejected promise with error.
``` Javascript
let promise = Promise.reject(error);
```

### Promisification
- It is the conversion of a function that accepts a callback into a function that returns a promise.
``` Javascript
function loadFn(src,callback){
	 // code here
}

let loadPromise = function(src){
	return new Promise((resolve,reject)=>{
		loadFn(src,(err,callback)=>{
			if(err) reject(err);
			else resolve(callback);
		});
	})
}
```

``` Javascript
// promisification helper function
function promisify(f){
	return function(...args){
		return new Promise((resolve,reject)=>{
			// custom callback 
			function callback(err,result){
				if(err) reject(err);
				else resolve(result);
			}
			// append custom callback to end of arguments
			args.push(callback);
			f.call(this,...args); // call the original function
		});
	}
}

// usage
let promise = promisify(callback);
promise.then().catch();
```


### Async / Await
- This is a special syntax to work with promises.
- It is just a more elegant syntax of getting the promise result than promise.then
- **async**
	- "async" keyword is to be placed before a function.
	- it means a function always returns a promise.
	- Other values are automatically wrapped in a resolved promise automatically.
- **await**
	- works only inside async functions
	- "await" makes JS wait until the promise settles and returns its result.
	- It suspends the function execution until the promise settles.
``` Javascript
async function f(){
	let result = await promise;
	return result;
}

f();
```

- Errors are handled using **try...catch**
- Works well with Promise.all etc as well.
