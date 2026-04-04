# Predicate (Type Checking) Functions

Reference documentation for type checking and predicate functions in es-toolkit.

## isArguments

Checks if the given value is an arguments object.

```typescript
function isArguments(value: unknown): boolean
```

```typescript
import { isArguments } from 'es-toolkit/predicate';

function foo() {
  console.log(isArguments(arguments)); // true
}
foo();

console.log(isArguments([1, 2, 3])); // false
console.log(isArguments('abc')); // false
```

## isArray

Checks if the given value is an array.

```typescript
function isArray(value: unknown): value is unknown[]
```

```typescript
import { isArray } from 'es-toolkit/predicate';

console.log(isArray([1, 2, 3])); // true
console.log(isArray({ 0: 'a', length: 1 })); // false
console.log(isArray('abc')); // false
```

## isArrayBuffer

Checks if the given value is an ArrayBuffer.

```typescript
function isArrayBuffer(value: unknown): value is ArrayBuffer
```

```typescript
import { isArrayBuffer } from 'es-toolkit/predicate';

console.log(isArrayBuffer(new ArrayBuffer(10))); // true
console.log(isArrayBuffer(new Int8Array(10))); // false
console.log(isArrayBuffer([])); // false
```

## isArrayLike

Checks if the given value is array-like (has a length property and can be iterated).

```typescript
function isArrayLike(value: unknown): value is ArrayLike<unknown>
```

```typescript
import { isArrayLike } from 'es-toolkit/predicate';

console.log(isArrayLike([1, 2, 3])); // true
console.log(isArrayLike(document.querySelectorAll('div'))); // true
console.log(isArrayLike('abc')); // true
console.log(isArrayLike({ length: 0 })); // true
console.log(isArrayLike({})); // false
console.log(isArrayLike(null)); // false
```

## isArrayLikeObject

Checks if the given value is an array-like object (not a function, has numeric length property).

```typescript
function isArrayLikeObject(value: unknown): value is ArrayLikeObject
```

```typescript
import { isArrayLikeObject } from 'es-toolkit/predicate';

console.log(isArrayLikeObject({ 0: 'a', length: 1 })); // true
console.log(isArrayLikeObject([1, 2, 3])); // true
console.log(isArrayLikeObject('abc')); // false
console.log(isArrayLikeObject(() => {})); // false
```

## isBlob

Checks if the given value is a Blob.

```typescript
function isBlob(value: unknown): value is Blob
```

```typescript
import { isBlob } from 'es-toolkit/predicate';

const blob = new Blob(['hello'], { type: 'text/plain' });
console.log(isBlob(blob)); // true
console.log(isBlob('hello')); // false
console.log(isBlob(null)); // false
```

## isBoolean

Checks if the given value is a boolean primitive.

```typescript
function isBoolean(value: unknown): value is boolean
```

```typescript
import { isBoolean } from 'es-toolkit/predicate';

console.log(isBoolean(true)); // true
console.log(isBoolean(false)); // true
console.log(isBoolean(new Boolean(true))); // false
console.log(isBoolean(1)); // false
console.log(isBoolean('true')); // false
```

## isBrowser

Checks if code is running in a browser environment.

```typescript
function isBrowser(): boolean
```

```typescript
import { isBrowser } from 'es-toolkit/predicate';

if (isBrowser()) {
  console.log('Running in browser');
  // Browser-specific code
} else {
  console.log('Running in Node.js or another environment');
}
```

## isBuffer

Checks if the given value is a Buffer.

```typescript
function isBuffer(value: unknown): value is Buffer
```

```typescript
import { isBuffer } from 'es-toolkit/predicate';
import { Buffer } from 'node:buffer';

console.log(isBuffer(Buffer.from('hello'))); // true
console.log(isBuffer(new Uint8Array([1, 2, 3]))); // false
console.log(isBuffer([1, 2, 3])); // false
```

## isDate

Checks if the given value is a Date object.

```typescript
function isDate(value: unknown): value is Date
```

```typescript
import { isDate } from 'es-toolkit/predicate';

console.log(isDate(new Date())); // true
console.log(isDate('Mon April 23 2012')); // false
console.log(isDate(Date.now())); // false
console.log(isDate(new Error('msg'))); // false
```

## isDefined

Checks if the given value is not undefined.

```typescript
function isDefined<T>(value: T | undefined): value is T
```

```typescript
import { isDefined } from 'es-toolkit/predicate';

console.log(isDefined(1)); // true
console.log(isDefined(null)); // true
console.log(isDefined(undefined)); // false
console.log(isDefined(NaN)); // true

const maybeUndefined: string | undefined = getValue();
if (isDefined(maybeUndefined)) {
  console.log(maybeUndefined.toUpperCase()); // TypeScript knows it's a string
}
```

## isEmpty

Checks if the given value is empty (length 0 or no keys).

```typescript
function isEmpty(value: unknown): boolean
```

```typescript
import { isEmpty } from 'es-toolkit/predicate';

console.log(isEmpty(null)); // true
console.log(isEmpty('')); // true
console.log(isEmpty([])); // true
console.log(isEmpty({})); // true
console.log(isEmpty(new Set())); // true
console.log(isEmpty(new Map())); // true
console.log(isEmpty([1, 2, 3])); // false
console.log(isEmpty('abc')); // false
console.log(isEmpty({ a: 1 })); // false
```

## isEmptyObject

Checks if the given value is an empty object (has no own enumerable properties).

```typescript
function isEmptyObject(value: unknown): value is Record<string, unknown>
```

```typescript
import { isEmptyObject } from 'es-toolkit/predicate';

console.log(isEmptyObject({})); // true
console.log(isEmptyObject({ a: 1 })); // false
console.log(isEmptyObject([])); // false
console.log(isEmptyObject(null)); // false
console.log(isEmptyObject('abc')); // false
```

## isEqual

Checks if two values are deeply equal.

```typescript
function isEqual(value: unknown, other: unknown): boolean
```

```typescript
import { isEqual } from 'es-toolkit/predicate';

console.log(isEqual(1, 1)); // true
console.log(isEqual('a', 'a')); // true
console.log(isEqual([1, 2, 3], [1, 2, 3])); // true
console.log(isEqual({ a: 1 }, { a: 1 })); // true
console.log(isEqual(1, '1')); // false
console.log(isEqual([1, 2], [1, 2, 3])); // false
console.log(isEqual({ a: 1 }, { a: 2 })); // false
```

## isEqualWith

Checks if two values are deeply equal using a custom comparator function.

```typescript
function isEqualWith<T>(value: T, other: T, customizer?: (value: unknown, other: unknown, key?: number | string) => boolean): boolean
```

```typescript
import { isEqualWith } from 'es-toolkit/predicate';

const object = { a: 1, b: { c: 2 } };
const other = { a: 1, b: { c: 3 } };

const isResult = isEqualWith(object, other, (value, other) => {
  if (typeof value === 'object' && typeof other === 'object') {
    return true;
  }
  return value === other;
});

console.log(isResult); // true
```

## isError

Checks if the given value is an Error object.

```typescript
function isError(value: unknown): value is Error
```

```typescript
import { isError } from 'es-toolkit/predicate';

console.log(isError(new Error())); // true
console.log(isError(new TypeError())); // true
console.log(isError(new RangeError())); // true
console.log(isError('error')); // false
console.log(isError({ message: 'error' })); // false
```

## isEven

Checks if the given number is even.

```typescript
function isEven(value: number): boolean
```

```typescript
import { isEven } from 'es-toolkit/predicate';

console.log(isEven(2)); // true
console.log(isEven(4)); // true
console.log(isEven(0)); // true
console.log(isEven(1)); // false
console.log(isEven(3)); // false
console.log(isEven(-2)); // true
```

## isFile

Checks if the given value is a File.

```typescript
function isFile(value: unknown): value is File
```

```typescript
import { isFile } from 'es-toolkit/predicate';

const file = new File(['hello'], 'hello.txt', { type: 'text/plain' });
console.log(isFile(file)); // true
console.log(isFile(new Blob(['hello']))); // false
console.log(isFile('hello')); // false
```

## isFinite

Checks if the given value is a finite number.

```typescript
function isFinite(value: unknown): value is number
```

```typescript
import { isFinite } from 'es-toolkit/predicate';

console.log(isFinite(1)); // true
console.log(isFinite(0)); // true
console.log(isFinite(-1)); // true
console.log(isFinite(Infinity)); // false
console.log(isFinite(NaN)); // false
console.log(isFinite('1')); // false
```

## isFunction

Checks if the given value is a function.

```typescript
function isFunction(value: unknown): value is (...args: unknown[]) => unknown
```

```typescript
import { isFunction } from 'es-toolkit/predicate';

console.log(isFunction(() => {})); // true
console.log(isFunction(async () => {})); // true
console.log(isFunction(function() {})); // true
console.log(isFunction({})); // false
console.log(isFunction([])); // false
console.log(isFunction('function')); // false
```

## isInteger

Checks if the given value is an integer.

```typescript
function isInteger(value: unknown): value is number
```

```typescript
import { isInteger } from 'es-toolkit/predicate';

console.log(isInteger(1)); // true
console.log(isInteger(0)); // true
console.log(isInteger(-1)); // true
console.log(isInteger(1.5)); // false
console.log(isInteger(1.0)); // true (1.0 is integer)
console.log(isInteger(NaN)); // false
console.log(isInteger(Infinity)); // false
console.log(isInteger('1')); // false
```

## isJSON

Checks if the given value is valid JSON (parseable).

```typescript
function isJSON(value: unknown): boolean
```

```typescript
import { isJSON } from 'es-toolkit/predicate';

console.log(isJSON('{"a":1}')); // true
console.log(isJSON('[1,2,3]')); // true
console.log(isJSON('"hello"')); // true
console.log(isJSON('null')); // true
console.log(isJSON('{a:1}')); // false (missing quotes)
console.log(isJSON(undefined)); // false
console.log(isJSON(123)); // false
```

## isJSONArray

Checks if the given value is a JSON array string.

```typescript
function isJSONArray(value: unknown): value is string
```

```typescript
import { isJSONArray } from 'es-toolkit/predicate';

console.log(isJSONArray('[1,2,3]')); // true
console.log(isJSONArray('[]')); // true
console.log(isJSONArray('[{"a":1}]')); // true
console.log(isJSONArray('{"a":1}')); // false
console.log(isJSONArray('[1,2,3')); // false (malformed)
console.log(isJSONArray(123)); // false
```

## isJSONObject

Checks if the given value is a JSON object string.

```typescript
function isJSONObject(value: unknown): value is string
```

```typescript
import { isJSONObject } from 'es-toolkit/predicate';

console.log(isJSONObject('{"a":1}')); // true
console.log(isJSONObject('{}')); // true
console.log(isJSONObject('{"a":[1,2,3]}')); // true
console.log(isJSONObject('[1,2,3]')); // false
console.log(isJSONObject('{a:1}')); // false
console.log(isJSONObject(123)); // false
```

## isJSONValue

Checks if the given value is a JSON primitive value (string, number, boolean, or null).

```typescript
function isJSONValue(value: unknown): boolean
```

```typescript
import { isJSONValue } from 'es-toolkit/predicate';

console.log(isJSONValue('"hello"')); // true
console.log(isJSONValue('123')); // true
console.log(isJSONValue('true')); // true
console.log(isJSONValue('false')); // true
console.log(isJSONValue('null')); // true
console.log(isJSONValue('{"a":1}')); // false (object)
console.log(isJSONValue('[1,2]')); // false (array)
console.log(isJSONValue(undefined)); // false
```

## isLength

Checks if the given value is a valid array-like length.

```typescript
function isLength(value: unknown): value is number
```

```typescript
import { isLength } from 'es-toolkit/predicate';

console.log(isLength(0)); // true
console.log(isLength(1)); // true
console.log(isLength(10)); // true
console.log(isLength(2 ** 53 - 1)); // true
console.log(isLength(-1)); // false
console.log(isLength(1.5)); // false
console.log(isLength(NaN)); // false
console.log(isLength(Infinity)); // false
console.log(isLength(2 ** 53)); // false
```

## isMap

Checks if the given value is a Map.

```typescript
function isMap(value: unknown): value is Map<unknown, unknown>
```

```typescript
import { isMap } from 'es-toolkit/predicate';

console.log(isMap(new Map())); // true
console.log(isMap(new Map([['a', 1]]))); // true
console.log(isMap(new WeakMap())); // false
console.log(isMap({})); // false
console.log(isMap([])); // false
```

## isMatch

Checks if an object matches the specified source properties.

```typescript
function isMatch<T extends Record<string, unknown>>(object: T, source: Partial<T>): boolean
```

```typescript
import { isMatch } from 'es-toolkit/predicate';

const object = { a: 1, b: 2, c: 3 };

console.log(isMatch(object, { a: 1 })); // true
console.log(isMatch(object, { a: 2 })); // false
console.log(isMatch(object, { a: 1, b: 2 })); // true
console.log(isMatch(object, { a: 1, d: 4 })); // false
console.log(isMatch(object, {})); // true
```

## isNaN

Checks if the given value is NaN.

```typescript
function isNaN(value: unknown): value is number
```

```typescript
import { isNaN } from 'es-toolkit/predicate';

console.log(isNaN(NaN)); // true
console.log(isNaN(0 / 0)); // true
console.log(isNaN('abc' - 1)); // true
console.log(isNaN(1)); // false
console.log(isNaN(0)); // false
console.log(isNaN('1')); // false
console.log(isNaN(Infinity)); // false
```

## isNative

Checks if the given value is a native function.

```typescript
function isNative(value: unknown): value is Function
```

```typescript
import { isNative } from 'es-toolkit/predicate';

console.log(isNative(Array.prototype.push)); // true
console.log(isNative(Object.prototype.toString)); // true
console.log(isNative(function() {})); // false
console.log(isNative(() => {})); // false
console.log(isNative(class {})); // false
```

## isNil

Checks if the given value is null or undefined.

```typescript
function isNil(value: unknown): value is null | undefined
```

```typescript
import { isNil } from 'es-toolkit/predicate';

console.log(isNil(null)); // true
console.log(isNil(undefined)); // true
console.log(isNil(NaN)); // false
console.log(isNil(0)); // false
console.log(isNil('')); // false
console.log(isNil(false)); // false
```

## isNode

Checks if the given value is a Node.js environment (not a browser).

```typescript
function isNode(): boolean
```

```typescript
import { isNode } from 'es-toolkit/predicate';

if (isNode()) {
  console.log('Running in Node.js');
  // Node.js-specific code
} else {
  console.log('Running in browser');
}
```

## isNotNil

Checks if the given value is not null or undefined.

```typescript
function isNotNil<T>(value: T): value is NonNullable<T>
```

```typescript
import { isNotNil } from 'es-toolkit/predicate';

console.log(isNotNil(1)); // true
console.log(isNotNil('hello')); // true
console.log(isNotNil(null)); // false
console.log(isNotNil(undefined)); // false

const maybeNull: string | null = getValue();
if (isNotNil(maybeNull)) {
  console.log(maybeNull.toUpperCase()); // TypeScript knows it's a string
}
```

## isNull

Checks if the given value is null.

```typescript
function isNull(value: unknown): value is null
```

```typescript
import { isNull } from 'es-toolkit/predicate';

console.log(isNull(null)); // true
console.log(isNull(undefined)); // false
console.log(isNull(0)); // false
console.log(isNull('')); // false
console.log(isNull(NaN)); // false
console.log(isNull({})); // false
```

## isNumber

Checks if the given value is a number primitive (not NaN or Infinity).

```typescript
function isNumber(value: unknown): value is number
```

```typescript
import { isNumber } from 'es-toolkit/predicate';

console.log(isNumber(1)); // true
console.log(isNumber(0)); // true
console.log(isNumber(-1.5)); // true
console.log(isNumber(NaN)); // false
console.log(isNumber(Infinity)); // false
console.log(isNumber('1')); // false
console.log(isNumber(new Number(1))); // false
```

## isObject

Checks if the given value is an object (not null, not primitive).

```typescript
function isObject(value: unknown): value is object
```

```typescript
import { isObject } from 'es-toolkit/predicate';

console.log(isObject({})); // true
console.log(isObject([])); // true
console.log(isObject(new Date())); // true
console.log(isObject(new Error())); // true
console.log(isObject(null)); // false
console.log(isObject('abc')); // false
console.log(isObject(123)); // false
console.log(isObject(true)); // false
console.log(isObject(() => {})); // false
```

## isObjectLike

Checks if the given value is object-like (has typeof 'object').

```typescript
function isObjectLike(value: unknown): value is object
```

```typescript
import { isObjectLike } from 'es-toolkit/predicate';

console.log(isObjectLike({})); // true
console.log(isObjectLike([])); // true
console.log(isObjectLike(new Date())); // true
console.log(isObjectLike(null)); // false
console.log(isObjectLike('abc')); // false
console.log(isObjectLike(123)); // false
console.log(isObjectLike(() => {})); // false
```

## isOdd

Checks if the given number is odd.

```typescript
function isOdd(value: number): boolean
```

```typescript
import { isOdd } from 'es-toolkit/predicate';

console.log(isOdd(1)); // true
console.log(isOdd(3)); // true
console.log(isOdd(-1)); // true
console.log(isOdd(2)); // false
console.log(isOdd(4)); // false
console.log(isOdd(0)); // false
```

## isPlainObject

Checks if the given value is a plain object (created by Object constructor or Object.create(null)).

```typescript
function isPlainObject(value: unknown): value is Record<string, unknown>
```

```typescript
import { isPlainObject } from 'es-toolkit/predicate';

console.log(isPlainObject({})); // true
console.log(isPlainObject({ a: 1 })); // true
console.log(isPlainObject(new Object())); // true
console.log(isPlainObject(Object.create(null))); // true
console.log(isPlainObject(new Date())); // false
console.log(isPlainObject([])); // false
console.log(isPlainObject(null)); // false
console.log(isPlainObject('abc')); // false
console.log(isPlainObject(class {})); // false
```

## isPrimitive

Checks if the given value is a primitive (null, undefined, string, number, boolean, symbol, bigint).

```typescript
function isPrimitive(value: unknown): value is Primitive
```

```typescript
import { isPrimitive } from 'es-toolkit/predicate';

console.log(isPrimitive(null)); // true
console.log(isPrimitive(undefined)); // true
console.log(isPrimitive('abc')); // true
console.log(isPrimitive(123)); // true
console.log(isPrimitive(true)); // true
console.log(isPrimitive(Symbol('sym'))); // true
console.log(isPrimitive(123n)); // true
console.log(isPrimitive({})); // false
console.log(isPrimitive([])); // false
console.log(isPrimitive(() => {})); // false
```

## isPromise

Checks if the given value is a Promise.

```typescript
function isPromise<T = unknown>(value: unknown): value is Promise<T>
```

```typescript
import { isPromise } from 'es-toolkit/predicate';

console.log(isPromise(Promise.resolve(1))); // true
console.log(isPromise(new Promise((resolve) => resolve(1)))); // true
console.log(isPromise(async () => {})); // true
console.log(isPromise({ then: () => {} })); // false (not a real Promise)
console.log(isPromise(123)); // false
console.log(isPromise('promise')); // false
```

## isRegExp

Checks if the given value is a RegExp.

```typescript
function isRegExp(value: unknown): value is RegExp
```

```typescript
import { isRegExp } from 'es-toolkit/predicate';

console.log(isRegExp(/abc/)); // true
console.log(isRegExp(new RegExp('abc'))); // true
console.log(isRegExp('abc')); // false
console.log(isRegExp({})); // false
console.log(isRegExp(['/abc/'])); // false
```

## isSafeInteger

Checks if the given value is a safe integer (between Number.MIN_SAFE_INTEGER and Number.MAX_SAFE_INTEGER).

```typescript
function isSafeInteger(value: unknown): value is number
```

```typescript
import { isSafeInteger } from 'es-toolkit/predicate';

console.log(isSafeInteger(1)); // true
console.log(isSafeInteger(-1)); // true
console.log(isSafeInteger(0)); // true
console.log(isSafeInteger(Number.MAX_SAFE_INTEGER)); // true
console.log(isSafeInteger(Number.MIN_SAFE_INTEGER)); // true
console.log(isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false
console.log(isSafeInteger(1.5)); // false
console.log(isSafeInteger(NaN)); // false
console.log(isSafeInteger(Infinity)); // false
```

## isSet

Checks if the given value is a Set.

```typescript
function isSet(value: unknown): value is Set<unknown>
```

```typescript
import { isSet } from 'es-toolkit/predicate';

console.log(isSet(new Set())); // true
console.log(isSet(new Set([1, 2, 3]))); // true
console.log(isSet(new WeakSet())); // false
console.log(isSet({})); // false
console.log(isSet([])); // false
```

## isString

Checks if the given value is a string primitive.

```typescript
function isString(value: unknown): value is string
```

```typescript
import { isString } from 'es-toolkit/predicate';

console.log(isString('abc')); // true
console.log(isString('')); // true
console.log(isString(`template`)); // true
console.log(isString(new String('abc'))); // false
console.log(isString(123)); // false
console.log(isString(true)); // false
console.log(isString(['abc'])); // false
```

## isSymbol

Checks if the given value is a Symbol.

```typescript
function isSymbol(value: unknown): value is symbol
```

```typescript
import { isSymbol } from 'es-toolkit/predicate';

console.log(isSymbol(Symbol('abc'))); // true
console.log(isSymbol(Symbol())); // true
console.log(isSymbol(Symbol.iterator)); // true
console.log(isSymbol('abc')); // false
console.log(isSymbol(123)); // false
console.log(isSymbol({})); // false
```

## isTypedArray

Checks if the given value is a TypedArray.

```typescript
function isTypedArray(value: unknown): value is TypedArray
```

```typescript
import { isTypedArray } from 'es-toolkit/predicate';

console.log(isTypedArray(new Int8Array())); // true
console.log(isTypedArray(new Uint8Array())); // true
console.log(isTypedArray(new Uint8ClampedArray())); // true
console.log(isTypedArray(new Int16Array())); // true
console.log(isTypedArray(new Uint16Array())); // true
console.log(isTypedArray(new Int32Array())); // true
console.log(isTypedArray(new Uint32Array())); // true
console.log(isTypedArray(new Float32Array())); // true
console.log(isTypedArray(new Float64Array())); // true
console.log(isTypedArray(new BigInt64Array())); // true
console.log(isTypedArray(new BigUint64Array())); // true
console.log(isTypedArray([])); // false
console.log(isTypedArray(new ArrayBuffer(10))); // false
```

## isUndefined

Checks if the given value is undefined.

```typescript
function isUndefined(value: unknown): value is undefined
```

```typescript
import { isUndefined } from 'es-toolkit/predicate';

console.log(isUndefined(undefined)); // true
console.log(isUndefined(null)); // false
console.log(isUndefined(NaN)); // false
console.log(isUndefined(0)); // false
console.log(isUndefined('')); // false
console.log(isUndefined({})); // false
```

## isUrl

Checks if the given value is a valid URL string.

```typescript
function isUrl(value: unknown): value is string
```

```typescript
import { isUrl } from 'es-toolkit/predicate';

console.log(isUrl('https://example.com')); // true
console.log(isUrl('http://example.com/path?query=1')); // true
console.log(isUrl('ftp://files.example.com')); // true
console.log(isUrl('example.com')); // false (missing protocol)
console.log(isUrl('not a url')); // false
console.log(isUrl('')); // false
console.log(isUrl({ url: 'https://example.com' })); // false
```

## isValidDate

Checks if the given value is a valid Date object (not Invalid Date).

```typescript
function isValidDate(value: unknown): value is Date
```

```typescript
import { isValidDate } from 'es-toolkit/predicate';

console.log(isValidDate(new Date())); // true
console.log(isValidDate(new Date('2023-01-01'))); // true
console.log(isValidDate(new Date(2023, 0, 1))); // true
console.log(isValidDate(new Date('invalid'))); // false
console.log(isValidDate(new Date(NaN))); // false
console.log(isValidDate('Mon April 23 2012')); // false
console.log(isValidDate(Date.now())); // false
```

## isWeakMap

Checks if the given value is a WeakMap.

```typescript
function isWeakMap(value: unknown): value is WeakMap<object, unknown>
```

```typescript
import { isWeakMap } from 'es-toolkit/predicate';

console.log(isWeakMap(new WeakMap())); // true
const weakMap = new WeakMap();
weakMap.set({}, 'value');
console.log(isWeakMap(weakMap)); // true
console.log(isWeakMap(new Map())); // false
console.log(isWeakMap({})); // false
```

## isWeakSet

Checks if the given value is a WeakSet.

```typescript
function isWeakSet(value: unknown): value is WeakSet<object>
```

```typescript
import { isWeakSet } from 'es-toolkit/predicate';

console.log(isWeakSet(new WeakSet())); // true
const weakSet = new WeakSet();
weakSet.add({});
console.log(isWeakSet(weakSet)); // true
console.log(isWeakSet(new Set())); // false
console.log(isWeakSet({})); // false
console.log(isWeakSet([])); // false
```
