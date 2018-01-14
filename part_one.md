## Additions to `Number`, `String`, `Array`, `Object`, and `RegExp`

### Number

- Number.isNaN() determines whether the passed value is NaN and its type is Number. Replaces global `isNaN()`
- Number.isFinite() replaces global `isFinite()`
- Number.isInteger()
- Number.parseInt() replaces global `parseInt()`
- Number.parseFloat() replaces global `parseFloat()`

Yahoo, less globals!! But old habits can die hard :(

```js

// Number.isNaN()
isNaN('hello')              // true
isNaN(0/0)                  // true
Number.isNaN('hello')       // false
Number.isNaN(0/0)           // true

// Number.isFinite()
Number.isFinite(Infinity)  // false
Number.isFinite(NaN)       // false
Number.isFinite(-Infinity) // false

Number.isFinite(0)         // true
Number.isFinite(2e64)      // true

Number.isFinite('0')        // false, would've been true with
                            // global isFinite('0')
Number.isFinite(null)       // false, would've been true with
                            // global isFinite(null)
// Number.isInteger()
Number.isInteger(0)          // true
Number.isInteger(1)          // true
Number.isInteger(1/2)        // false
Number.isInteger('hello')    // false
Number.isInteger(false)      // false
Number.isInteger([1])        // false

// Number.parseInt()
Number.parseInt('1', 10)   // 1
Number.parseInt('1/2', 10) // 1
Number.parseInt('15 sheep', 10) // 15
Number.parseInt('sheep 15', 10) // NaN
Number.parseInt('10:15', 10) // 10
Number.parseInt('-10', 10) // -10
Number.parseInt('A', 16) // 10
Number.parseInt('F', 16) // 15
Number.parseInt('G', 16) // NaN
Number.parseInt('G', 17) // 16

// Number.parseFloat()
// The following examples all return 3.14
parseFloat(3.14);
parseFloat('3.14');
parseFloat('314e-2');
parseFloat('0.0314E+2');
parseFloat('3.14more non-digit characters');


```

  - Number.EPSILON
  - Number.MAX_SAFE_INTEGER, Number.MIN_SAFE_INTEGER
  - Number.isSafeInteger()


### String

  - String.fromCodePoint()
  - String.prototype.codePointAt()
  - String.prototype.startsWith()
  - String.prototype.endsWith()
  - String.prototype.includes()
  - String.prototype.repeat()
  - String.raw()

```js

// String.fromCodePoint()

String.fromCodePoint(42);       // "*"
String.fromCodePoint(65, 90);   // "AZ"
String.fromCodePoint(97, 98, 122);   // "abz"

// String.prototype.codePointAt()

'abz'.codePointAt(0) // 97
'abz'.codePointAt(1) // 98
'abz'.codePointAt(2) // 122
'abz'.codePointAt(3) // undefined

// String.prototype.startsWith()
var str = 'To be, or not to be, that is the question.';

str.startsWith('To be')         // true
str.startsWith('not to be')     // false
str.startsWith('not to be', 10) // true

// String.prototype.endsWith()
var str = 'To be, or not to be, that is the question.';

str.endsWith('question.') // true
str.endsWith('to be')     // false
str.endsWith('to be', 19) // true

// String.prototype.includes()
var str = 'To be, or not to be, that is the question.';

str.includes('To be')       // true
str.includes('question')    // true
str.includes('nonexistent') // false
str.includes('To be', 1)    // false
str.includes('TO BE')       // false

// String.prototype.repeat()
'abc'.repeat(-1);   // RangeError
'abc'.repeat(0);    // ''
'abc'.repeat(1);    // 'abc'
'abc'.repeat(2);    // 'abcabc'
'abc'.repeat(3.5);  // 'abcabcabc' (count will be converted to integer)
'abc'.repeat(1/0);  // RangeError

// String.raw()
console.log('\u0041\n\/')
/*
'A
/'
*/
String.raw`\u0041\n\/` // \u0041\n\/


```


  - String.prototype.normalize()
  - \u{XXXXXX} Unicode code point escapes

### Array

- Array iteration with for...of - the generic iterator
- Array.from() - convert array-like to array for access to its prototype methods
- Array.of() - Array.of(element0[, element1[, ...[, elementN]]])
- Array.prototype.fill() - arr.fill(value[, start[, end]])
- Array.prototype.find(), Array.prototype.findIndex()
- Array.prototype.entries(),
- Array.prototype.keys()
- Array.prototype.copyWithin()

#### Challenge
- create array

### Object

- Object.prototype.__proto__ has been standardized
- Object.is()
- Object.setPrototypeOf()
- Object.assign()
  - Object.getOwnPropertySymbols()

### RegExp

- RegExp sticky (y) flag
- generic RegExp.prototype.toString
- RegExp.prototype[@@match]()
- RegExp.prototype[@@replace]()
- RegExp.prototype[@@search]()
- RegExp.prototype[@@split]()

  - get RegExp[@@species]
  - RegExp unicode (u) flag


## arrow functions

### example

```js

const simplestArrow = () => true
console.log(simplestArrow()) // true

const simpleParam = p => p
console.log(simpleParam('foo')) // foo

const multiparamSingleline = (p1, p2) => p1 + p2
console.log(multiparamSingleline('foo', 'bar')) // foobar

const multilineScope = p => {
  const f = p[0]
  return f + 'ee'
}
console.log(multilineScope('foo')) // fee

// brackets!
const multiparamMultiline = (p1, p2) => {
  const f = p[0]
  return f + 'ee' + p2
}
console.log(multiparamMultiline('foo', 'bar')) // feebar
// brackets!

const fnWrapping = fn1_p => fn2_p => fn1_p + fn2_p
console.log(fnWrapping('foo')('bar')) // foobar
// ahh, the currying construct...
// ==
function fnWrappingClassical(fn1_p) {
  return function (fn2_p) {
    return fn1_p + fn2_p
  }
}
console.log(fnWrappingClassical('foo')('bar')) // foobar

```

### use cases
- reduces code
- facilitates Fn programming
  - curry
    - configuration for modules, `const configed_mod = require(a_module)(its_configurations)`
    - deferred executions - a lecture in itself
### tradeoffs
- reading and writing arrow functions take practice

## classes

### example


```js

class myArrayType extends Array {
  constructor(...elements) {
    super(...elements)
  }
  count() {
    return this.length
  }
  get firstElement() {
    return this[0]
  }
  set firstElement(val) {
    this[0] = val
  }
  static collectionKeyVal(keys, vals) {
    return keys.map( (ea, index) => { return {[ea]: vals[index]} } )
  }
}

const my_arr = new myArrayType('d','b', 'c')

console.log( my_arr)
my_arr myArrayType [ 'd', 'b', 'c' ]
console.log( my_arr.count()) // 3
console.log( my_arr.firstElement) // d
my_arr.firstElement = 'a'
console.log( my_arr.firstElement) // a
console.log( my_arr) // [ 'a', 'b', 'c' ]
console.log( myArrayType.collectionKeyVal(my_arr, ['e', 'f', 'g'])) // [ { a: 'e' }, { b: 'f' }, { c: 'g' } ]

```

### use cases
- realigns with classical OOP
- safer alternative than `prototype` for extending built-in native JS objects and libraries

### tradeoffs
- `get`, `set` are anti-patterns for Fn programming
- though safer to `prototype`, it's really sugar

## enhanced object literals

### example

```js
var handler = function(param) {
  return param + 'toc'
}
var obj = {
    // __proto__
    __proto__: {

    },
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return 'd ' + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};

console.log(obj)
// { handler: [Function: handler],
//   toString: [Function: toString],
//   prop_42: 42 }

console.log(obj.toString()) // d [object Object]
console.log(obj.handler('tic')) // tictoc

```

### use cases

### tradeoffs

Overriding can break presumptions

```js

const obj_native = new Object()

console.log(obj.__proto__)
console.log(obj_native.__proto__)

```
### challenge

- Object literal that extends native Object prototype?

## template strings

```js

const verb_participle = 'interpolated'
const base_template_string = `This sentences is ${verb_participle}`

console.log(base_template_string) // This sentences is interpolated


const multiline = `Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.

Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.`


const multiline_interpolation = `Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.

This sentences is ${verb_participle}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
`


const expression_interpolation = `Five plus two is ${5+2}`

console.log(expression_interpolation) // Five plus two is 7


const logic_interpolation = `Three is ${ 3 > 2 ? 'larger' : 'smaller'} than two.`

console.log(logic_interpolation) // Three is larger than two.


// allows functions to interpolate strings

function sentenceComposer(strings, s, v) {
  console.log(strings) // strings [ 'The ', ' happily ', ' in the woods' ]
  console.log(s) // bear
  console.log(v) // eats
  return `${strings[0]}${s}${strings[1]}${v}${strings[2]}`
}

const subject = 'bear'
const verb = 'eats'
const tagged_template_literal = sentenceComposer`The ${subject} happily ${verb} in the woods`
console.log(tagged_template_literal) // The bear happily eats in the woods

const tagged_template_literal_2 = sentenceComposer`A ${"monkey"} sadly ${"cries"} in the jungle`
console.log(tagged_template_literal_2) // A monkey sadly cries in the jungle


// There are a few other features. See MDN 'Template literals'. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals


```

### use cases

- single and multiline string formation and manipulation



## destructuring

### example

### use cases

### tradeoffs



## default
## rest
## spread

### example

### use cases



## let
## const

### example

### use cases



## iterators + for..of

### example

### use cases


## shorthand property names (Firefox 33) https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Property_definitions
## computed property names (Firefox 34) https://developer.mozilla.org/en-US/Firefox/Releases/34
## shorthand method names (Firefox 34) https://developer.mozilla.org/en-US/Firefox/Releases/34
