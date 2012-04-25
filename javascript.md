# JavaScript

This is based on the style that I've personally picked up. Feel free to edit it a bit. It probably isn't even self-consistent, so feel free to correct.

## Whitespace and indentation
- Always spaces, never tabs
- Always two spaces for indentation
- Never leave trailing whitespace
- One space before and after every operator
  - With the exception of unary `+` and `-`, and possibly some others

## General things
- Strings should use single quotes where possible
  - Unless the string needs to contain single quotes, in which case double quotes is cool.
- Always include semicolons if they should be there
- Never EVER EVAAAR use comma-first notation. Ever.

## Variable names
```javascript
var a = '';

var short = '';

var longerName = '';

var evenLongerNameThatShouldntBeThisLong = '';
// If you're camelCasing more than three words, you're doing it wrong.

// Simple variables, regexes, object instances and functions should
// all use lowercase-first camelCase

// Class names should be capitalised
var ClassName = function(){ /* ... */ };

// Constants shoud be uppercase + underscores.
var CONSTANT_NAME = 'blah';
// Avoid requiring underscores if possible. One is ok, two is meh, three is bad.
// Again, if your variable names are that long, then you're doing it wrong.
```

## Conditionals

```javascript
// One space at beginning and end of conditional statement
if( a === b ){
  alert('foo'); 
}

// With else: no spaces around curly braces
if( a === b ){
  alert('foo');
}else{
  alert('bar');
}

// Omitting curly braces: one space after condition
if( a === b ) alert('foo');

// If you're going to omit the curly braces, then keep it all on one line
// So this is bad and should be avoided:
if( a == b )
  alert('foo');
anotherFunction();


// Ternary statement
var b = foo ? 'bar' : 'baz';

// Nested ternaries should ideally have brackets
// This is mostly an artefact of the fact that PHP behaves differently without them
var b = foo ? (bar == 'blah' ? 'a' : 'b') : 'c';
```

## Loops
Very similar to the conditionals.
```javascript
// Where possible, prefer to use += 1 rather than ++
// More readable, makes for easier editing.
// Also where possible, make the var statements outside the loop structure
for( i = 0; i < 1000; i += 1 ){
  console.log(i);
}

while( i === 7 ){
  console.log('i is seven');
}

do{
  alert('yay');
}while( i === 7 );

for( i in blah ){
  // Always, always ALWAYS remember this check in a for/in loop
  if( !blah.hasOwnProperty(i) ) continue;
}

// Shorthand: same as conditionals. All on one line.
for( i = 0; i < 10; i += 1 ) alert(i);

// Empty loop body: single semicolon immediately after loop condition
// The one case in which I would say to use ++ or -- is when you're using the value
while( a[i++] < 0 );
```

## Functions
```javascript
function a(arg1, arg2, arg3){
  doSomeStuff();
}

// Calling
a();
a(arg);
a(blah, blah);

var a = function(){
  doSomeStuff();
}; // Always remember the semicolon if you're assigning to a variable

// Callbacks:
setTimeout(function(){
  console.log('boom');
}, 10000);

// Closures
(function(){
  var foo = 'bar';
  // More stuff
})();

// Binding scope
someAsynchronousFunction(function(err, result){
  this.log(result);
}.bind(this));
```

## Declaring variables
```javascipt
// Single variable:
var a = 0;

// Ok to leave it undefined:
var a;

// Lots of variables:
var a = 0, b = 2, c = 3;

// Above about four variables (or even fewer if the values are big), newlines
// Only have one var though
// Never never EVER comma-first.
var a = 0,
  b = 1,
  c = 2,
  d = 3,
  e, f, g, h, // Undefined variables can have their own line
  str = 'hi', len = str.length; // Tightly-related vars can share a line

// Arrays
// Again, ALWAYS comma-last
var a = [
  'foo',
  'bar',
  'baz'
];

// Short arrays can have one line:
var a = ['a', 'b', 'c', 'd'];

// Objects
// Yep, you guessed it. Commas LAST.
var obj = {
  key1: 'value1',
  key2: 'value2',
  anObjectKey: {
    a: 'a',
    b: 'b'
  },
  anArray: [0, 1, 2, 3]
};

// Similarly, short objects can have one line
var shortObj = { awesome: true };

// Local variables
// Try wherever possible to put all local variables at the top of a function,
// because that's how the JS engine interprets them anyway
function(){
  var x = 3, y; // ok to leave y undefined for now.

  // All the rest of the stuff.
}


```

### Logical Shorthands
Often conditional statements can be shorthanded using logical `&&` and `||`. They can also be used for variables, for example to provide a default value.

```javascript

// To shorten simple conditional/loop bodies
for( i in blah ) blah.hasOwnProperty(i) && someArray.push(i);

// Default variable values
function fadeOut( duration ){
  duration = duration || 500;
  // Be very careful here that you want to override ALL falsy values. So here,
  // 0 would be overridden to 500, as would undefined, null, '' etc.

  doSomeStuff(duration);
}

// More variable defaults:
a = {
  key1: 'value1',
  key2: 'value2'
}[someGivenKey] || 'defaultValue';

// or...
b = [0, 1, 2, 3][idx] || -1;
// Be very careful here that idx can't take a value like 'length'
```

## Type coercion
JavaScript can be a right bastard at times with type coercion.
```javascript

// If in doubt, use === unless you have a reason not to.
if( a === b ) blah();

// One exception: typeof checking, because it will always be a string
if( typeof a == 'undefined' ) blah();
```

### Converting types
```javascript

// Converting to number: use the unary +
b = +a;

// Remark: try to avoid using parseInt here
parseInt('010'); // 8, because it's octal.
// Unless you're doing it deliberately, that is.

// Converting to string: append/prepend empty string
// This is an instance where it's excusable to omit the whitespace around the +
a = ''+b;

// Coercing to boolean: bang bang
a = !!b
```

## More Shorthand
```javascript
// You'll see me do this all the time: substitutes for Math.floor
// Use these very carefully. Ideally only for small positive numbers.
b = ~~a;
b = 0|a;
// Use these in different ways depending on precedence.

// Generally speaking, default to ~~
b = ~~functionName(blah);

// Use | to avoid brackets in simple cases:
b = 0|a*pi
// This is equivalent to Math.floor(a*pi) or ~~(a*pi)

b = ~~a * pi
// This is equivalent to Math.floor(a) * pi;

// If you do use both of the above, try to use the whitespace to make it clear what's going on.

// Avoid using these shorthands on long expressions.
// Single variables or function calls are good.
// Any expression with more than two variables in it is bad.
```

## Classes
Well, they're not classes. JavaScript doesn't have classes. Instantiable objects then.

```javascript
// Capitalise the name
var MyClass = function(){

  // JavaScript doesn't have private variables, but use underscores to indicate
  // intent. If you're accessing an underscored variable from outside one of these
  // class functions, you're doing it wrong
  this._internalVariable = 'blah';
  this.otherVariable = 0;

  // Don't declare functions in here. It's very inefficient
  // UNLESS there's something very specific that depends on the input argument
  // But try to avoid it
};

// Prototype properties. Only use primitives. Never put objects on the prototype
MyClass.prototype.defaultVariableValue = 0;
// Try to avoid these though. Put them in the constructor.

MyClass.prototype.instanceMethod = function(){
  this._internalVariable = 'foo';
};

// Binding scope (this is as good a place as any)
// Prefer .bind(this) rather than using var self = this;
MyClass.prototype.asyncMethod = function(){

  someAsyncCall(this.instanceMethod.bind(this));

  someOtherAsyncCall(function(){
    this.otherVariable = 22;
  }.bind(this));
}
```

### Inheritance
Inheritance is messy in JavaScript, however you do it. This is my take on it.

```javascript
var MyInheritingClass = funciton(){
  // possibly some pre-initialization here...

  MyClass.apply(this, arguments);

  // Ideally all initialization should come afterwards
};

MyInheritingClass.prototype = Object.create(MyClass.prototype);

// Overriding functions
MyInheritingClass.instanceMethod = function(){
  // To call the base function, do this:
  MyClass.prototype.instanceMethod.apply(this, arguments);
  // Could alternatively .call() it to specify arguments. Depends on use case

  // More stuff here...
};
```

## Chaining functions
jQuery is wonderful; it does chaining. Keep it neat though
```javascript
$('.some-selector')
  .css({ blah: 'blah' })
  .appendTo('body')
  .parent();

// Unless the variable name is really short (it shouldn't really be),
// in which case the first one can go on the first line, thus:
a.css({ display: 'foo' })
  .animate({ height: 100 }, 500);
```

## Comments
- Single-line (`// blah`) or multiline (`/* blah */`)
- Try to use multiline for anything more than two lines
- Prefer single-lines though. If your comment needs to be that long, then your code should be better written to be more self-explanatory.
- Always have a space inside the multiline
  - i.e. never do `/*comment*/`, always `/* comment */`
- Avoid end-of-line comments if possible. On short lines it's ok, but definitely avoid on long lines
- Never, ever, EVAR use a `/* blah */` in the middle of a line
  - Only time where this is excusable: commenting something out (eg `if( a === 1 /* && b === 2 */)`) when testing. But then delete it before it reaches production.

## More general things to bear in mind
- Unless you have a very, VERY good reason otherwise, avoid `eval` like the plague.
  - Likewise with passing strings as arguments to `setTimeout` or `setInterval`.
- If you ever use a `with` clause, I will personally come round to your house and punch you in the face.
  - Only one exception: templating functions. Unfortunately `with` is the only way to keep the scope nice. But don't use it ANYWHERE else. EVAR.
- Never avoid using semicolons. Don't go randomly throwing them in unnecessarily, but always put them in if necessary. Especially after functions assigned to variables, or self-executing functions.