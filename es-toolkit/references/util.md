# Utility Functions Reference

A comprehensive guide to es-toolkit's utility functions.

## attempt

Attempts to invoke a function, returning the result or caught error.

**Function Signature:**
```typescript
function attempt<T>(func: (...args: any[]) => T, ...args: any[]): T | Error
```

**Description:** Invokes a function and returns the result. If an error is thrown, it returns the error object instead of throwing it.

**Example:**
```javascript
import { attempt } from 'es-toolkit';

const elements = attempt(() => JSON.parse('{a:1}'));

if (elements instanceof Error) {
  console.log('Parse failed');
}

const result = attempt(() => JSON.parse('{"a":1}'));
console.log(result);    // { a: 1 }
```

---

## attemptAsync

Attempts to invoke an async function, returning the result or caught error.

**Function Signature:**
```typescript
function attemptAsync<T>(func: (...args: any[]) => Promise<T>, ...args: any[]): Promise<T | Error>
```

**Description:** Invokes an async function and returns a promise that resolves with the result or rejects with the caught error as a value.

**Example:**
```javascript
import { attemptAsync } from 'es-toolkit';

const result = await attemptAsync(async () => {
  const response = await fetch('https://example.com');
  return response.json();
});
```

---

## bind

Creates a function that is bound to a specific context.

**Function Signature:**
```typescript
function bind<F extends (...args: any[]) => any>(func: F, thisArg: any, ...partials: any[]): F
```

**Description:** Creates a new function with `this` bound to the specified value and optionally prepends arguments.

**Example:**
```javascript
import { bind } from 'es-toolkit';

const object = {
  user: 'fred',
  greet: function(greeting) {
    return `${greeting}, ${this.user}!`;
  }
};

const boundGreet = bind(object.greet, object);
boundGreet('Hello');    // 'Hello, fred!'

const boundWithArg = bind(object.greet, object, 'Hi');
boundWithArg();         // 'Hi, fred!'
```

---

## bindAll

Binds all methods of an object to the object itself.

**Function Signature:**
```typescript
function bindAll<T>(object: T, methodNames?: Array<keyof T>): T
```

**Description:** Binds all enumerable function properties of an object to the object itself, ensuring `this` always refers to the object.

**Example:**
```javascript
import { bindAll } from 'es-toolkit';

const view = {
  label: 'docs',
  click: function() {
    console.log('clicked ' + this.label);
  }
};

bindAll(view);
view.click();    // 'clicked docs' (always works, even when called as callback)
```

---

## cond

Creates a function that iterates through conditions and returns the result of the first matching one.

**Function Signature:**
```typescript
function cond(pairs: Array<[Predicate, ResultFunction]>): (...args: any[]) => any
```

**Description:** Creates a conditional function that tests each pair in order and returns the value of the first truthy predicate's corresponding function.

**Example:**
```javascript
import { cond } from 'es-toolkit';

const func = cond([
  [x => x < 5, () => 'less than 5'],
  [x => x === 5, () => 'equal to 5'],
  [x => x > 5, () => 'greater than 5']
]);

func(3);    // 'less than 5'
func(5);    // 'equal to 5'
func(10);   // 'greater than 5'
```

---

## conforms

Creates a function that invokes a predicate with the properties of an object.

**Function Signature:**
```typescript
function conforms<T>(source: Partial<T>): (obj: T) => boolean
```

**Description:** Creates a function that returns `true` if all the properties of the source object satisfy their corresponding predicates when tested on the input object.

**Example:**
```javascript
import { conforms } from 'es-toolkit';

const object = { a: 1, b: 2 };
const predicates = {
  a: n => n > 0,
  b: n => n > 1
};

const matches = conforms(predicates);
matches(object);    // true

const partialMatch = conforms({ a: n => n === 1 });
partialMatch(object);    // true
```

---

## conformsTo

Checks if an object conforms to a source object with predicate functions.

**Function Signature:**
```typescript
function conformsTo<T>(object: T, source: Partial<T>): boolean
```

**Description:** Checks if `object` conforms to the source object's predicates. Returns `true` if all predicates pass.

**Example:**
```javascript
import { conformsTo } from 'es-toolkit';

const object = { a: 1, b: 2 };
const source = {
  a: n => n > 0,
  b: n => n > 1
};

conformsTo(object, source);    // true
conformsTo({ a: 1 }, { a: n => n === 2 });    // false
```

---

## constant

Creates a function that returns the same value each time it's called.

**Function Signature:**
```typescript
function constant<T>(value: T): () => T
```

**Description:** Creates a function that always returns the provided value, useful for creating placeholder functions.

**Example:**
```javascript
import { constant } from 'es-toolkit';

const object = { name: 'fred' };
const constantObject = constant(object);

console.log(constantObject());    // { name: 'fred' }
console.log(constantObject() === constantObject());    // true (same reference)
```

---

## defaultTo

Returns the first argument if it's not `null`, `undefined`, or `NaN`, otherwise returns the default value.

**Function Signature:**
```typescript
function defaultTo<T, U>(value: T, defaultValue: U): T | U
```

**Description:** Provides a safe way to handle optional values by providing a fallback default.

**Example:**
```javascript
import { defaultTo } from 'es-toolkit';

defaultTo(1, 10);           // 1
defaultTo(null, 10);         // 10
defaultTo(undefined, 10);    // 10
defaultTo(NaN, 10);          // 10
defaultTo('hello', 10);      // 'hello'
```

---

## eq

Performs a deep comparison between two values for equality.

**Function Signature:**
```typescript
function eq<T>(value: T, other: any): boolean
```

**Description:** Performs a deep equality check between two values. Handles objects, arrays, and primitives.

**Example:**
```javascript
import { eq } from 'es-toolkit';

eq('a', 'a');                // true
eq('a', 'b');                // false
eq([1, 2], [1, 2]);          // true
eq({ a: 1 }, { a: 1 });      // true
eq({ a: 1 }, { a: 2 });      // false
eq(NaN, NaN);                // true (unlike ===)
```

---

## identity

Returns the first argument it receives.

**Function Signature:**
```typescript
function identity<T>(value: T): T
```

**Description:** A simple identity function that returns its input unchanged. Useful as a default callback or placeholder.

**Example:**
```javascript
import { identity } from 'es-toolkit';

identity('a');      // 'a'
identity({ a: 1 }); // { a: 1 }
identity(5);        // 5
```

---

## iteratee

Creates a function that invokes a method on objects or returns a property value.

**Function Signature:**
```typescript
function iteratee(func?: Function | string | object): Function
```

**Description:** Creates a function that transforms values based on the input. Can be a function, property name, or predicate object.

**Example:**
```javascript
import { iteratee } from 'es-toolkit';

const func = iteratee('a');
func({ a: 1 });    // 1

const isEven = iteratee(n => n % 2 === 0);
isEven(4);    // true
isEven(3);    // false

const matches = iteratee({ a: 1 });
matches({ a: 1, b: 2 });    // true
matches({ a: 2, b: 2 });    // false
```

---

## invariant

Throws an error with a message if a condition is false.

**Function Signature:**
```typescript
function invariant(condition: any, message?: string): void
```

**Description:** Throws an `Error` with the provided message if the condition is falsy. Useful for runtime assertions.

**Example:**
```javascript
import { invariant } from 'es-toolkit';

invariant(true, 'Expected true');    // passes
invariant(1 === 1, 'Math is broken');    // passes
invariant(false, 'Expected truthy');     // throws Error: 'Expected truthy'
invariant(condition, 'Condition failed');    // throws if condition is falsy
```

---

## invoke

Invokes a method on an object at a specified path.

**Function Signature:**
```typescript
function invoke<T>(object: T, path: PropertyPath, args?: any[]): any
```

**Description:** Invokes the method at `path` of `object` with `args` as arguments. Works with nested paths.

**Example:**
```javascript
import { invoke } from 'es-toolkit';

invoke({ a: [{ b: { c: function(x, y) { return x + y; } } }] }, 'a[0].b.c', 1, 2);    // 3
invoke({ a: [{ b: function() { return 'hello'; } }] }, 'a.0.b');    // 'hello'
```

---

## invokeAsync

Invokes an async method on an object at a specified path.

**Function Signature:**
```typescript
function invokeAsync<T>(object: T, path: PropertyPath, args?: any[]): Promise<any>
```

**Description:** Invokes the async method at `path` of `object` with `args` as arguments and returns a promise.

**Example:**
```javascript
import { invokeAsync } from 'es-toolkit';

const object = {
  fetch: async function(url) {
    return await fetch(url);
  }
};

const result = await invokeAsync(object, 'fetch', ['https://example.com']);
```

---

## lt

Checks if a value is less than another value.

**Function Signature:**
```typescript
function lt(value: any, other: any): boolean
```

**Description:** Returns `true` if `value` is strictly less than `other` according to JavaScript's comparison algorithm.

**Example:**
```javascript
import { lt } from 'es-toolkit';

lt(1, 3);      // true
lt(3, 3);      // false
lt(3, 1);      // false
lt('a', 'b');  // true
lt('b', 'a');  // false
```

---

## lte

Checks if a value is less than or equal to another value.

**Function Signature:**
```typescript
function lte(value: any, other: any): boolean
```

**Description:** Returns `true` if `value` is less than or equal to `other` according to JavaScript's comparison algorithm.

**Example:**
```javascript
import { lte } from 'es-toolkit';

lte(1, 3);     // true
lte(3, 3);     // true
lte(3, 1);     // false
lte('a', 'b'); // true
lte('a', 'a'); // true
```

---

## method

Creates a function that invokes a method at a specified path on an object.

**Function Signature:**
```typescript
function method(path: PropertyPath, args?: any[]): Function
```

**Description:** Returns a function that calls the method at `path` on any provided object with the specified arguments.

**Example:**
```javascript
import { method } from 'es-toolkit';

const getName = method('name');
const person = { name: 'fred', age: 30 };
getName(person);    // 'fred'

const getValue = method('getValue', 1, 2);
const obj = { getValue: function(a, b) { return a + b; } };
getValue(obj);    // 3
```

---

## methodOf

Creates a function that invokes a method on an object at a specified path.

**Function Signature:**
```typescript
function methodOf(object: object, args?: any[]): Function
```

**Description:** Returns a function that calls the method at a specified path on the provided object. Inverse of `method`.

**Example:**
```javascript
import { methodOf } from 'es-toolkit';

const object = {
  speak: function(greeting) {
    return `${greeting}!`;
  }
};

const speakHello = methodOf(object)('speak');
speakHello();    // '!'
```

---

## noop

A no-operation function that returns `undefined`.

**Function Signature:**
```typescript
function noop(): undefined
```

**Description:** A function that does nothing and returns `undefined`. Useful as a default callback or placeholder.

**Example:**
```javascript
import { noop } from 'es-toolkit';

noop();    // undefined

const callback = noop;
[1, 2, 3].forEach(noop);    // does nothing
```

---

## now

Returns the timestamp of the current time in milliseconds.

**Function Signature:**
```typescript
function now(): number
```

**Description:** Returns `Date.now()` or an equivalent high-resolution timestamp for performance measurement.

**Example:**
```javascript
import { now } from 'es-toolkit';

const start = now();
someOperation();
const end = now();
console.log(`Operation took ${end - start}ms`);
```

---

## nthArg

Creates a function that returns the nth argument passed to it.

**Function Signature:**
```typescript
function nthArg(n?: number): (...args: any[]) => any
```

**Description:** Returns a function that returns the argument at index `n` (negative indices count from the end).

**Example:**
```javascript
import { nthArg } from 'es-toolkit';

const secondArg = nthArg(1);
secondArg(1, 2, 3);    // 2

const lastArg = nthArg(-1);
lastArg(1, 2, 3);      // 3

const firstArg = nthArg();
firstArg(1, 2, 3);     // 1
```

---

## property

Creates a function that returns the property value at a specified path.

**Function Signature:**
```typescript
function property(path?: PropertyPath): (object: object) => any
```

**Description:** Returns a function that gets the value at `path` from an object. Supports nested paths.

**Example:**
```javascript
import { property } from 'es-toolkit';

const getName = property('name');
const person = { name: 'fred', age: 30 };
getName(person);    // 'fred'

const getStreet = property('address.street');
const user = { address: { street: '123 Main St' } };
getStreet(user);    // '123 Main St'
```

---

## propertyOf

Creates a function that returns the property value at a specified path on an object.

**Function Signature:**
```typescript
function propertyOf(object: object): (path: PropertyPath) => any
```

**Description:** Returns a function that gets the value at `path` from the provided object. Inverse of `property`.

**Example:**
```javascript
import { propertyOf } from 'es-toolkit';

const object = {
  person: {
    name: 'fred',
    address: { street: '123 Main St' }
  }
};

const getPersonName = propertyOf(object)('person.name');
getPersonName;    // 'fred'

const getStreet = propertyOf(object)('person.address.street');
getStreet;    // '123 Main St'
```

---

## range

Creates an array of numbers from start to end (exclusive).

**Function Signature:**
```typescript
function range(start: number, end: number, step?: number): number[]
```

**Description:** Generates an array of numbers starting from `start` and incrementing by `step` (default: 1) until reaching `end`.

**Example:**
```javascript
import { range } from 'es-toolkit';

range(0, 4);      // [0, 1, 2, 3]
range(1, 5);      // [1, 2, 3, 4]
range(0, 10, 2);  // [0, 2, 4, 6, 8]
range(0, -5, -1); // [0, -1, -2, -3, -4]
range(5);         // [0, 1, 2, 3, 4]
```

---

## rangeRight

Creates an array of numbers from start to end (exclusive), in reverse order.

**Function Signature:**
```typescript
function rangeRight(start: number, end: number, step?: number): number[]
```

**Description:** Same as `range` but fills the array from right to left, which can be more efficient for large ranges.

**Example:**
```javascript
import { rangeRight } from 'es-toolkit';

rangeRight(0, 4);      // [3, 2, 1, 0]
rangeRight(0, 10, 2);  // [8, 6, 4, 2, 0]
rangeRight(5);         // [4, 3, 2, 1, 0]
```

---

## result

Invokes a method on an object, or returns a property value if it's a function.

**Function Signature:**
```typescript
function result<T>(object: T, path?: PropertyPath, args?: any[]): any
```

**Description:** Gets the value at `path` of `object`. If the resolved value is a function, it's invoked with `args`.

**Example:**
```javascript
import { result } from 'es-toolkit';

const object = {
  greet: function() {
    return 'Hello!';
  },
  person: {
    name: 'fred'
  }
};

result(object, 'greet');                      // 'Hello!'
result(object, 'person.name');                // 'fred'
result({ foo: 'bar' }, 'foo');                // 'bar'
result({ foo: null }, 'foo', 'default');      // 'default'
```

---

## times

Invokes a callback function n times and returns an array of results.

**Function Signature:**
```typescript
function times<T>(n: number, iteratee?: (index: number) => T): T[]
```

**Description:** Calls `iteratee` `n` times, returning an array of results. The callback receives the index as an argument.

**Example:**
```javascript
import { times } from 'es-toolkit';

times(3, String);              // ['0', '1', '2']
times(4, n => n * n);         // [0, 1, 4, 9]
times(2, () => Math.random()); // [0.234, 0.567] (example values)
times(0);                      // []
```

---

## toPath

Converts a string or array path to an array of property keys.

**Function Signature:**
```typescript
function toPath(path: PropertyPath): Array<string | number>
```

**Description:** Converts a dot-notation string, bracket notation string, or array into an array of property keys that can be used to access nested properties.

**Example:**
```javascript
import { toPath } from 'es-toolkit';

toPath('a.b.c');            // ['a', 'b', 'c']
toPath('a[0].b');           // ['a', '0', 'b']
toPath('a.b[0][1]');        // ['a', 'b', '0', '1']
toPath(['a', 'b', 'c']);    // ['a', 'b', 'c']
toPath('a.0.b');            // ['a', '0', 'b']
```

---

## uniqueId

Generates a unique ID with an optional prefix.

**Function Signature:**
```typescript
function uniqueId(prefix?: string): string
```

**Description:** Generates a unique ID string, incrementing a counter each time. Optionally prepends a prefix.

**Example:**
```javascript
import { uniqueId } from 'es-toolkit';

uniqueId();        // '1'
uniqueId();        // '2'
uniqueId('item_'); // 'item_3'
uniqueId('user_'); // 'user_4'
```
