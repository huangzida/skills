# Array Functions Reference

A comprehensive reference for array manipulation functions in es-toolkit.

## chunk

**Signature:**
```typescript
chunk<T>(array: T[], size?: number): T[][]
```

**Description:** Splits an array into chunks of the specified size.

**Example:**
```javascript
import { chunk } from 'es-toolkit';

chunk([1, 2, 3, 4, 5], 2);
// => [[1, 2], [3, 4], [5]]

chunk([1, 2, 3, 4, 5], 3);
// => [[1, 2, 3], [4, 5]]
```

---

## compact

**Signature:**
```typescript
compact<T>(array: T[]): T[]
```

**Description:** Removes falsy values (false, null, 0, "", undefined, NaN) from an array.

**Example:**
```javascript
import { compact } from 'es-toolkit';

compact([0, 1, false, 2, '', 3, null, NaN]);
// => [1, 2, 3]
```

---

## concat

**Signature:**
```typescript
concat<T>(array: T[], ...values: (T | T[])[]): T[]
```

**Description:** Concatenates arrays and values into a single array.

**Example:**
```javascript
import { concat } from 'es-toolkit';

concat([1], [2], [3]);
// => [1, 2, 3]

concat([1], [[2, 3]], [[4, [5]]]);
// => [1, [2, 3], [4, [5]]]
```

---

## difference

**Signature:**
```typescript
difference<T>(array: T[], values: T[]): T[]
```

**Description:** Returns elements from the first array that are not in the second array.

**Example:**
```javascript
import { difference } from 'es-toolkit';

difference([1, 2, 3, 4], [2, 4]);
// => [1, 3]
```

---

## differenceBy

**Signature:**
```typescript
differenceBy<T, U>(array: T[], values: T[], iteratee: (value: T) => U): T[]
```

**Description:** Returns elements from the first array that are not in the second array, using a iteratee to compare values.

**Example:**
```javascript
import { differenceBy } from 'es-toolkit';

differenceBy([{ x: 1 }, { x: 2 }], [{ x: 1 }], (item) => item.x);
// => [{ x: 2 }]

differenceBy([2.1, 1.2], [2.3, 3.4], Math.floor);
// => [1.2]
```

---

## differenceWith

**Signature:**
```typescript
differenceWith<T>(array: T[], values: T[], comparator: (arrVal: T, othVal: T) => boolean): T[]
```

**Description:** Returns elements from the first array that are not matched by the comparator in the second array.

**Example:**
```javascript
import { differenceWith } from 'es-toolkit';

differenceWith([{ x: 1 }, { x: 2 }], [{ x: 1 }], (a, b) => a.x === b.x);
// => [{ x: 2 }]
```

---

## drop

**Signature:**
```typescript
drop<T>(array: T[], n?: number): T[]
```

**Description:** Creates a slice of an array excluding the first n elements.

**Example:**
```javascript
import { drop } from 'es-toolkit';

drop([1, 2, 3, 4], 2);
// => [3, 4]

drop([1, 2, 3], 5);
// => []

drop([1, 2, 3], 0);
// => [1, 2, 3]
```

---

## dropRight

**Signature:**
```typescript
dropRight<T>(array: T[], n?: number): T[]
```

**Description:** Creates a slice of an array excluding the last n elements.

**Example:**
```javascript
import { dropRight } from 'es-toolkit';

dropRight([1, 2, 3, 4], 2);
// => [1, 2]

dropRight([1, 2, 3], 5);
// => []

dropRight([1, 2, 3], 0);
// => [1, 2, 3]
```

---

## dropRightWhile

**Signature:**
```typescript
dropRightWhile<T>(array: T[], predicate: (value: T) => boolean): T[]
```

**Description:** Removes elements from the end of an array while the predicate returns true.

**Example:**
```javascript
import { dropRightWhile } from 'es-toolkit';

dropRightWhile([1, 2, 3, 4], (n) => n > 2);
// => [1, 2]

dropRightWhile([{ x: 1 }, { x: 2 }], (item) => item.x > 1);
// => [{ x: 1 }]
```

---

## dropWhile

**Signature:**
```typescript
dropWhile<T>(array: T[], predicate: (value: T) => boolean): T[]
```

**Description:** Removes elements from the beginning of an array while the predicate returns true.

**Example:**
```javascript
import { dropWhile } from 'es-toolkit';

dropWhile([1, 2, 3, 4], (n) => n < 3);
// => [3, 4]

dropWhile([{ x: 1 }, { x: 2 }], (item) => item.x < 2);
// => [{ x: 2 }]
```

---

## fill

**Signature:**
```typescript
fill<T>(array: T[], value: T, start?: number, end?: number): T[]
```

**Description:** Fills elements of an array with a value from start to end positions.

**Example:**
```javascript
import { fill } from 'es-toolkit';

fill([1, 2, 3], 'a');
// => ['a', 'a', 'a']

fill([1, 2, 3, 4], 'a', 1, 3);
// => [1, 'a', 'a', 4]
```

---

## filter

**Signature:**
```typescript
filter<T>(array: T[], predicate: (value: T, index: number) => boolean): T[]
```

**Description:** Creates a new array with all elements that pass the predicate test.

**Example:**
```javascript
import { filter } from 'es-toolkit';

filter([1, 2, 3, 4], (n) => n % 2 === 0);
// => [2, 4]

filter([{ active: true }, { active: false }], (item) => item.active);
// => [{ active: true }]
```

---

## filterAsync

**Signature:**
```typescript
filterAsync<T>(array: T[], predicate: (value: T, index: number) => Promise<boolean>): Promise<T[]>
```

**Description:** Asynchronously filters an array using an async predicate function.

**Example:**
```javascript
import { filterAsync } from 'es-toolkit';

const isActive = async (item) => {
  const response = await fetch(`/api/${item.id}`);
  return response.ok;
};

await filterAsync([{ id: 1 }, { id: 2 }], isActive);
// => filtered results
```

---

## find

**Signature:**
```typescript
find<T>(array: T[], predicate: (value: T, index: number) => boolean, fromIndex?: number): T | undefined
```

**Description:** Returns the first element that passes the predicate test.

**Example:**
```javascript
import { find } from 'es-toolkit';

find([1, 2, 3, 4], (n) => n > 2);
// => 3

find([{ id: 1 }, { id: 2 }], (item) => item.id === 2);
// => { id: 2 }
```

---

## flatMap

**Signature:**
```typescript
flatMap<T, R>(array: T[], iteratee: (value: T, index: number) => R | R[]): R[]
```

**Description:** Maps each element using a function and flattens the result into a single array (one level deep).

**Example:**
```javascript
import { flatMap } from 'es-toolkit';

flatMap([1, 2], (n) => [n, n * 2]);
// => [1, 2, 2, 4]

flatMap(['hello', 'world'], (str) => str.split(''));
// => ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```

---

## flatMapAsync

**Signature:**
```typescript
flatMapAsync<T, R>(array: T[], iteratee: (value: T, index: number) => Promise<R | R[]>): Promise<R[]>
```

**Description:** Asynchronously maps and flattens an array using an async iteratee function.

**Example:**
```javascript
import { flatMapAsync } from 'es-toolkit';

const fetchItems = async (id) => {
  const res = await fetch(`/api/${id}`);
  return res.json();
};

await flatMapAsync([1, 2], fetchItems);
// => flattened results
```

---

## flatMapDeep

**Signature:**
```typescript
flatMapDeep<T, R>(array: T[], iteratee: (value: T, index: number) => R | R[] | R[][]): R[]
```

**Description:** Maps and deeply flattens the result into a single array.

**Example:**
```javascript
import { flatMapDeep } from 'es-toolkit';

flatMapDeep([1, 2, 3], (n) => [[n, n + 1], [n + 2]]);
// => [1, 2, 3, 2, 3, 4, 3, 4, 5]
```

---

## flatten

**Signature:**
```typescript
flatten<T>(array: T[][]): T[]
```

**Description:** Flattens a nested array one level deep.

**Example:**
```javascript
import { flatten } from 'es-toolkit';

flatten([[1, 2], [3, 4], [5]]);
// => [1, 2, 3, 4, 5]

flatten([[1], [[2]], [[[3]]]]);
// => [1, [2], [[3]]]
```

---

## flattenDeep

**Signature:**
```typescript
flattenDeep<T>(array: any[]): T[]
```

**Description:** Recursively flattens a nested array to any depth.

**Example:**
```javascript
import { flattenDeep } from 'es-toolkit';

flattenDeep([1, [2, [3, [4]], 5]]);
// => [1, 2, 3, 4, 5]

flattenDeep([[1], [[2]], [[[3]]]]);
// => [1, 2, 3]
```

---

## flattenObject

**Signature:**
```typescript
flattenObject<T>(obj: Record<string, T>): Record<string, T>
```

**Description:** Flattens a nested object into a single-level object with dot-notation keys.

**Example:**
```javascript
import { flattenObject } from 'es-toolkit';

flattenObject({ a: { b: { c: 1 } }, d: 2 });
// => { 'a.b.c': 1, d: 2 }
```

---

## forEach

**Signature:**
```typescript
forEach<T>(array: T[], iteratee: (value: T, index: number) => void): T[]
```

**Description:** Iterates over elements of an array and invokes iteratee for each element.

**Example:**
```javascript
import { forEach } from 'es-toolkit';

forEach([1, 2, 3], (value, index) => {
  console.log(`${index}: ${value}`);
});
// => '0: 1' '1: 2' '2: 3'
```

---

## forEachAsync

**Signature:**
```typescript
forEachAsync<T>(array: T[], iteratee: (value: T, index: number) => Promise<void>): Promise<T[]>
```

**Description:** Asynchronously iterates over elements of an array.

**Example:**
```javascript
import { forEachAsync } from 'es-toolkit';

await forEachAsync([1, 2, 3], async (value) => {
  await fetch(`/api/${value}`);
  console.log(value);
});
```

---

## forEachRight

**Signature:**
```typescript
forEachRight<T>(array: T[], iteratee: (value: T, index: number) => void): T[]
```

**Description:** Iterates over elements of an array from right to left.

**Example:**
```javascript
import { forEachRight } from 'es-toolkit';

forEachRight([1, 2, 3], (value, index) => {
  console.log(`${index}: ${value}`);
});
// => '2: 3' '1: 2' '0: 1'
```

---

## groupBy

**Signature:**
```typescript
groupBy<T, K extends string>(array: T[], iteratee: (value: T) => K): Record<K, T[]>
```

**Description:** Groups elements of an array by the return value of the iteratee.

**Example:**
```javascript
import { groupBy } from 'es-toolkit';

groupBy([6.1, 4.2, 6.3], Math.floor);
// => { '4': [4.2], '6': [6.1, 6.3] }

groupBy(['one', 'two', 'three'], 'length');
// => { '3': ['one', 'two'], '5': ['three'] }
```

---

## intersection

**Signature:**
```typescript
intersection<T>(array1: T[], array2: T[]): T[]
```

**Description:** Returns elements that appear in both arrays.

**Example:**
```javascript
import { intersection } from 'es-toolkit';

intersection([1, 2, 3], [2, 3, 4]);
// => [2, 3]

intersection([1, 2, 3], [4, 5, 6]);
// => []
```

---

## intersectionBy

**Signature:**
```typescript
intersectionBy<T, U>(array1: T[], array2: T[], iteratee: (value: T) => U): T[]
```

**Description:** Returns elements that appear in both arrays using an iteratee for comparison.

**Example:**
```javascript
import { intersectionBy } from 'es-toolkit';

intersectionBy([2.1, 1.2], [2.3, 3.4], Math.floor);
// => [2.1]

intersectionBy([{ x: 1 }, { x: 2 }], [{ x: 1 }], (item) => item.x);
// => [{ x: 1 }]
```

---

## intersectionWith

**Signature:**
```typescript
intersectionWith<T>(array1: T[], array2: T[], comparator: (a: T, b: T) => boolean): T[]
```

**Description:** Returns elements that appear in both arrays using a custom comparator.

**Example:**
```javascript
import { intersectionWith } from 'es-toolkit';

intersectionWith(
  [{ x: 1, y: 2 }, { x: 2, y: 1 }],
  [{ x: 1, y: 2 }],
  (a, b) => a.x === b.x && a.y === b.y
);
// => [{ x: 1, y: 2 }]
```

---

## isSubset

**Signature:**
```typescript
isSubset<T>(array: T[], subset: T[]): boolean
```

**Description:** Checks if the first array is a subset of the second array.

**Example:**
```javascript
import { isSubset } from 'es-toolkit';

isSubset([1, 2, 3], [1, 2, 3, 4, 5]);
// => true

isSubset([1, 5, 6], [1, 2, 3, 4, 5]);
// => false
```

---

## isSubsetWith

**Signature:**
```typescript
isSubsetWith<T>(array: T[], subset: T[], comparator: (a: T, b: T) => boolean): boolean
```

**Description:** Checks if the first array is a subset of the second using a custom comparator.

**Example:**
```javascript
import { isSubsetWith } from 'es-toolkit';

isSubsetWith(
  [{ x: 1 }, { x: 2 }],
  [{ x: 1 }, { x: 2 }, { x: 3 }],
  (a, b) => a.x === b.x
);
// => true
```

---

## keyBy

**Signature:**
```typescript
keyBy<T, K extends string>(array: T[], iteratee: (value: T) => K): Record<K, T>
```

**Description:** Creates an object keyed by the result of the iteratee function.

**Example:**
```javascript
import { keyBy } from 'es-toolkit';

keyBy([{ id: 'a', name: 'Alice' }, { id: 'b', name: 'Bob' }], 'id');
// => { a: { id: 'a', name: 'Alice' }, b: { id: 'b', name: 'Bob' } }

keyBy(['one', 'two', 'three'], (s) => s.length);
// => { '3': 'two', '5': 'three' }
```

---

## limitAsync

**Signature:**
```typescript
limitAsync<T, R>(limit: number, array: T[], iteratee: (value: T) => Promise<R>): Promise<R[]>
```

**Description:** Processes array elements asynchronously with a concurrency limit.

**Example:**
```javascript
import { limitAsync } from 'es-toolkit';

const fetchUser = async (id) => {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
};

await limitAsync(2, [1, 2, 3, 4, 5], fetchUser);
// => processes up to 2 concurrent requests
```

---

## map

**Signature:**
```typescript
map<T, R>(array: T[], iteratee: (value: T, index: number) => R): R[]
```

**Description:** Creates a new array with the results of calling iteratee on every element.

**Example:**
```javascript
import { map } from 'es-toolkit';

map([1, 2, 3], (n) => n * 2);
// => [2, 4, 6]

map([{ x: 1 }, { x: 2 }], (item) => item.x);
// => [1, 2]
```

---

## mapAsync

**Signature:**
```typescript
mapAsync<T, R>(array: T[], iteratee: (value: T, index: number) => Promise<R>): Promise<R[]>
```

**Description:** Creates a new array with the results of calling async iteratee on every element.

**Example:**
```javascript
import { mapAsync } from 'es-toolkit';

const fetchUser = async (id) => {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
};

await mapAsync([1, 2, 3], fetchUser);
// => [user1, user2, user3]
```

---

## orderBy

**Signature:**
```typescript
orderBy<T>(array: T[], orders: ('asc' | 'desc')[]): T[]
orderBy<T>(array: T[], iteratees: ((value: T) => any)[], orders: ('asc' | 'desc')[]): T[]
```

**Description:** Sorts elements of an array by one or more iteratees in ascending or descending order.

**Example:**
```javascript
import { orderBy } from 'es-toolkit';

orderBy([{ a: 1, b: 2 }, { a: 3, b: 1 }], ['a', 'b'], ['asc', 'desc']);
// => [{ a: 1, b: 2 }, { a: 3, b: 1 }]

orderBy([1, 2, 3, 4, 5], [(n) => n % 2], ['desc']);
// => [2, 4, 1, 3, 5]
```

---

## partition

**Signature:**
```typescript
partition<T>(array: T[], predicate: (value: T) => boolean): [T[], T[]]
```

**Description:** Creates an array of elements split into two groups based on the predicate.

**Example:**
```javascript
import { partition } from 'es-toolkit';

partition([1, 2, 3, 4], (n) => n % 2 === 0);
// => [[2, 4], [1, 3]]

partition(['a', 'b', 'aa', 'bb'], (s) => s.length === 1);
// => [['a', 'b'], ['aa', 'bb']]
```

---

## pull

**Signature:**
```typescript
pull<T>(array: T[], ...values: T[]): T[]
```

**Description:** Removes all given values from an array in place.

**Example:**
```javascript
import { pull } from 'es-toolkit';

const array = [1, 2, 3, 2, 4, 2];
pull(array, 2);
// => [1, 3, 4]

pull(['a', 'b', 'c', 'a', 'b'], 'a', 'b');
// => ['c']
```

---

## sample

**Signature:**
```typescript
sample<T>(array: T[]): T
```

**Description:** Gets a random element from an array.

**Example:**
```javascript
import { sample } from 'es-toolkit';

sample([1, 2, 3, 4, 5]);
// => random element from the array
```

---

## shuffle

**Signature:**
```typescript
shuffle<T>(array: T[]): T[]
```

**Description:** Creates a shuffled copy of an array using Fisher-Yates algorithm.

**Example:**
```javascript
import { shuffle } from 'es-toolkit';

shuffle([1, 2, 3, 4, 5]);
// => [3, 1, 5, 2, 4] (random order)
```

---

## size

**Signature:**
```typescript
size<T>(array: T[]): number
```

**Description:** Gets the size of an array, string, or object.

**Example:**
```javascript
import { size } from 'es-toolkit';

size([1, 2, 3]);
// => 3

size('hello');
// => 5

size({ a: 1, b: 2 });
// => 2
```

---

## slice

**Signature:**
```typescript
slice<T>(array: T[], start?: number, end?: number): T[]
```

**Description:** Creates a slice of an array from start up to (but not including) end.

**Example:**
```javascript
import { slice } from 'es-toolkit';

slice([1, 2, 3, 4, 5], 1, 3);
// => [2, 3]

slice([1, 2, 3], 0, 5);
// => [1, 2, 3]
```

---

## sortBy

**Signature:**
```typescript
sortBy<T>(array: T[], iteratees: ((value: T) => any)[]): T[]
```

**Description:** Sorts elements of an array by one or more iteratees in ascending order.

**Example:**
```javascript
import { sortBy } from 'es-toolkit';

sortBy([{ a: 1, b: 2 }, { a: 3, b: 1 }], [(item) => item.a]);
// => [{ a: 1, b: 2 }, { a: 3, b: 1 }]

sortBy(['b', 'a', 'cc'], [(s) => s.length]);
// => ['b', 'a', 'cc']
```

---

## take

**Signature:**
```typescript
take<T>(array: T[], n?: number): T[]
```

**Description:** Creates a slice of an array from the beginning with n elements.

**Example:**
```javascript
import { take } from 'es-toolkit';

take([1, 2, 3, 4, 5], 2);
// => [1, 2]

take([1, 2, 3], 5);
// => [1, 2, 3]

take([1, 2, 3], 0);
// => []
```

---

## takeRight

**Signature:**
```typescript
takeRight<T>(array: T[], n?: number): T[]
```

**Description:** Creates a slice of an array from the end with n elements.

**Example:**
```javascript
import { takeRight } from 'es-toolkit';

takeRight([1, 2, 3, 4, 5], 2);
// => [4, 5]

takeRight([1, 2, 3], 5);
// => [1, 2, 3]

takeRight([1, 2, 3], 0);
// => []
```

---

## uniq

**Signature:**
```typescript
uniq<T>(array: T[]): T[]
```

**Description:** Creates a duplicate-free version of an array.

**Example:**
```javascript
import { uniq } from 'es-toolkit';

uniq([1, 2, 2, 3, 3, 3, 4]);
// => [1, 2, 3, 4]

uniq([1.1, 2.2, 2.1, 3.3], Math.floor);
// => [1.1, 2.2, 3.3]
```

---

## uniqBy

**Signature:**
```typescript
uniqBy<T, U>(array: T[], iteratee: (value: T) => U): T[]
```

**Description:** Creates a duplicate-free version of an array using an iteratee for comparison.

**Example:**
```javascript
import { uniqBy } from 'es-toolkit';

uniqBy([{ x: 1 }, { x: 2 }, { x: 1 }], (item) => item.x);
// => [{ x: 1 }, { x: 2 }]

uniqBy([1.2, 1.1, 2.2, 2.3], Math.floor);
// => [1.2, 2.2]
```

---

## unzip

**Signature:**
```typescript
unzip<T>(array: T[][]): T[][]
```

**Description:** Opposite of zip - groups elements by their indices into separate arrays.

**Example:**
```javascript
import { unzip } from 'es-toolkit';

unzip([['a', 1], ['b', 2], ['c', 3]]);
// => [['a', 'b', 'c'], [1, 2, 3]]

unzip([['a', 1, true], ['b', 2, false]]);
// => [['a', 'b'], [1, 2], [true, false]]
```

---

## zip

**Signature:**
```typescript
zip<T>(...arrays: T[][]): T[][]
```

**Description:** Creates an array of grouped elements from multiple arrays.

**Example:**
```javascript
import { zip } from 'es-toolkit';

zip(['a', 'b'], [1, 2], [true, false]);
// => [['a', 1, true], ['b', 2, false]]

zip(['a', 'b'], [1, 2, 3]);
// => [['a', 1], ['b', 2], [undefined, 3]]
```

---

## zipObject

**Signature:**
```typescript
zipObject<T>(keys: (string | number)[], values: T[]): Record<string | number, T>
```

**Description:** Creates an object from two arrays - one of keys and one of values.

**Example:**
```javascript
import { zipObject } from 'es-toolkit';

zipObject(['a', 'b', 'c'], [1, 2, 3]);
// => { a: 1, b: 2, c: 3 }

zipObject(['x', 'y'], [10, 20, 30]);
// => { x: 10, y: 20 }
```
