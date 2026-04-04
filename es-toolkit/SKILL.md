---
name: es-toolkit
description: High-performance JavaScript utility library. Use when you need fast, tree-shakeable utility functions for arrays, objects, strings, promises, and more. Use for code refactoring, performance optimization, and modern JS/TS projects.
---

# es-toolkit

es-toolkit is a modern JavaScript utility library with first-class TypeScript support and small bundle size. It provides optimized, tree-shakeable utility functions that are 2-3x faster than lodash while being 97% smaller.

## Table of Contents

1. [Array](#array)
2. [Function](#function)
3. [Math](#math)
4. [Object](#object)
5. [Predicate](#predicate)
6. [Promise](#promise)
7. [String](#string)
8. [Util](#util)
9. [Error](#error)
10. [Map](#map)
11. [Set](#set)
12. [Performance Advantages](#performance-advantages)
13. [Usage Examples](#usage-examples)

## Array

| Function | Description | Type |
|----------|-------------|------|
| `chunk` | Splits array into chunks of specified size | native |
| `compact` | Removes falsy values from array | native |
| `concat` | Concatenates arrays and values | native |
| `difference` | Returns array elements not in other array | native |
| `differenceBy` | Returns difference based on iteratee | compat |
| `drop` | Drops elements from start of array | native |
| `dropRight` | Drops elements from end of array | native |
| `fill` | Fills array with value | native |
| `filter` | Filters array by predicate | native |
| `find` | Finds element in array | native |
| `flatMap` | Maps and flattens result | native |
| `flatten` | Flattens array one level | native |
| `forEach` | Iterates over array elements | native |
| `groupBy` | Groups array by key | native |
| `intersection` | Returns common elements | native |
| `map` | Maps array elements | native |
| `uniq` | Returns unique elements | native |
| `uniqBy` | Returns unique elements by iteratee | native |
| `zip` | Combines arrays into tuples | native |

## Function

| Function | Description | Type |
|----------|-------------|------|
| `after` | Creates function that runs after n calls | native |
| `ary` | Creates function with limited arity | compat |
| `before` | Creates function that runs before n calls | native |
| `curry` | Curries function | native |
| `debounce` | Debounces function calls | native |
| `memoize` | Memoizes function results | native |
| `negate` | Negates predicate function | native |
| `noop` | No-operation function | native |
| `once` | Creates single-use function | native |
| `throttle` | Throttles function calls | native |

## Math

| Function | Description | Type |
|----------|-------------|------|
| `add` | Adds two numbers | native |
| `ceil` | Rounds up to precision | native |
| `clamp` | Clamps number within range | native |
| `floor` | Rounds down to precision | native |
| `inRange` | Checks if number is in range | native |
| `mean` | Calculates average | native |
| `meanBy` | Calculates average by iteratee | native |
| `median` | Calculates median | native |
| `random` | Generates random number | native |
| `round` | Rounds to precision | native |
| `subtract` | Subtracts two numbers | native |
| `sum` | Sums array elements | native |
| `sumBy` | Sums by iteratee | native |

## Object

| Function | Description | Type |
|----------|-------------|------|
| `clone` | Shallow clones object | native |
| `cloneDeep` | Deep clones object | native |
| `deepMerge` | Deep merges objects | native |
| `get` | Gets nested property safely | native |
| `has` | Checks if property exists | native |
| `invert` | Inverts keys and values | native |
| `mapKeys` | Maps object keys | native |
| `mapValues` | Maps object values | native |
| `merge` | Shallow merges objects | native |
| `omit` | Omits specified keys | native |
| `pick` | Picks specified keys | native |
| `set` | Sets nested property | native |

## Predicate

| Function | Description | Type |
|----------|-------------|------|
| `isArray` | Checks if value is array | native |
| `isEmpty` | Checks if value is empty | native |
| `isEqual` | Deep equality comparison | native |
| `isNil` | Checks if null or undefined | native |
| `isNull` | Checks if null | native |
| `isNumber` | Checks if number | native |
| `isObject` | Checks if object | native |
| `isString` | Checks if string | native |
| `isUndefined` | Checks if undefined | native |

## Promise

| Function | Description | Type |
|----------|-------------|------|
| `delay` | Delays execution | native |
| `memoizeAsync` | Memoizes async function | compat |
| `Mutex` | Mutex for async operations | native |
| `poll` | Polls until condition | native |
| `retry` | Retries failed operations | native |
| `sleep` | Sleeps for specified duration | native |

## String

| Function | Description | Type |
|----------|-------------|------|
| `camelCase` | Converts to camelCase | native |
| `capitalize` | Capitalizes first letter | native |
| `kebabCase` | Converts to kebab-case | native |
| `lowerCase` | Converts to lowercase | native |
| `lowerFirst` | Lowercases first character | compat |
| `snakeCase` | Converts to snake_case | native |
| `template` | Template string interpolation | compat |
| `truncate` | Truncates string | native |
| `upperCase` | Converts to uppercase | native |
| `upperFirst` | Uppercases first character | compat |

## Util

| Function | Description | Type |
|----------|-------------|------|
| `attempt` | Wraps function with error handling | compat |
| `bind` | Binds function context | compat |
| `defaultTo` | Returns default if value is invalid | native |
| `identity` | Returns input as-is | native |
| `noop` | No-operation function | native |
| `property` | Creates property accessor | compat |
| `times` | Calls function n times | compat |
| `uniqueId` | Generates unique ID | compat |

## Error

| Function | Description | Type |
|----------|-------------|------|
| `AbortError` | Error class for abort operations | native |

## Map

| Function | Description | Type |
|----------|-------------|------|
| `countBy` | Counts by iteratee | native |
| `every` | Checks if all match predicate | native |
| `filter` | Filters map entries | native |
| `findKey` | Finds key by predicate | native |
| `forEach` | Iterates over entries | native |
| `keyBy` | Keys by iteratee | native |
| `map` | Maps values | native |
| `mapValues` | Maps values with iteratee | native |

## Set

| Function | Description | Type |
|----------|-------------|------|
| `countBy` | Counts by iteratee | native |
| `every` | Checks if all match predicate | native |
| `filter` | Filters set entries | native |
| `find` | Finds element by predicate | native |
| `forEach` | Iterates over entries | native |
| `keyBy` | Keys by iteratee | native |
| `map` | Maps values | native |

## Performance Advantages

- **97% smaller** than lodash (~14KB vs ~531KB minified)
- **2-3x faster** than lodash for most operations
- **Tree-shakeable** - only bundle functions you use
- **Native implementations** - optimized for modern JavaScript engines
- **100% test coverage** - battle-tested reliability
- **TypeScript support** - full type definitions included
- **ESM-first** - modern module system support

## Usage Examples

### Array Operations

```typescript
import { chunk, uniq, difference, groupBy } from 'es-toolkit';

const numbers = [1, 2, 2, 3, 3, 3, 4];
uniq(numbers); // [1, 2, 3, 4]

chunk([1, 2, 3, 4, 5], 2); // [[1, 2], [3, 4], [5]]

difference([1, 2, 3], [2, 4]); // [1, 3]

groupBy([{ type: 'a', val: 1 }, { type: 'b', val: 2 }, { type: 'a', val: 3 }], 'type');
// { a: [{ type: 'a', val: 1 }, { type: 'a', val: 3 }], b: [{ type: 'b', val: 2 }] }
```

### Object Operations

```typescript
import { get, set, cloneDeep, omit, pick } from 'es-toolkit';

const obj = { a: { b: { c: 1 } } };
get(obj, 'a.b.c'); // 1
get(obj, 'a.b.d', 'default'); // 'default'

set(obj, 'a.b.e', 2);
omit({ a: 1, b: 2, c: 3 }, ['b']); // { a: 1, c: 3 }
pick({ a: 1, b: 2, c: 3 }, ['a', 'c']); // { a: 1, c: 3 }
```

### Function Utilities

```typescript
import { debounce, throttle, memoize, curry } from 'es-toolkit';

const debouncedFn = debounce(() => console.log('debounced'), 300);
const throttledFn = throttle(() => console.log('throttled'), 300);

const memoized = memoize((n: number) => expensiveCalc(n));

const curried = curry((a: number, b: number, c: number) => a + b + c);
curried(1)(2)(3); // 6
curried(1, 2)(3); // 6
```

### Math Operations

```typescript
import { clamp, random, sum, mean } from 'es-toolkit';

clamp(10, 0, 5); // 5
clamp(-10, 0, 5); // 0
clamp(3, 0, 5); // 3

random(1, 10); // Random integer between 1 and 10
sum([1, 2, 3, 4, 5]); // 15
mean([1, 2, 3, 4, 5]); // 3
```

### Promise Utilities

```typescript
import { delay, retry, poll, sleep } from 'es-toolkit';

await sleep(1000); // Wait 1 second

const result = await retry(() => fetchData(), { count: 3 });

const data = await poll(() => checkStatus(), { interval: 1000, timeout: 10000 });
```

### String Utilities

```typescript
import { camelCase, kebabCase, snakeCase, truncate } from 'es-toolkit';

camelCase('hello_world'); // 'helloWorld'
kebabCase('helloWorld'); // 'hello-world'
snakeCase('helloWorld'); // 'hello_world'

truncate('Hello World', { length: 8 }); // 'Hello...'
truncate('Hello World', { length: 8, omission: '～' }); // 'Hello W～'
```

### Predicate Utilities

```typescript
import { isEmpty, isEqual, isNil } from 'es-toolkit';

isEmpty([]); // true
isEmpty({}); // true
isEmpty('hello'); // false

isEqual({ a: 1 }, { a: 1 }); // true
isEqual([1, 2, 3], [1, 2, 3]); // true

isNil(null); // true
isNil(undefined); // true
isNil(0); // false
```
