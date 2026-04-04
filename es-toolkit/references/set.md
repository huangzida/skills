# Set Operations Reference

Collection functions for working with Sets and performing set-like operations.

## countBy

Groups Set elements by a key and counts elements in each group.

```typescript
function countBy<T>(set: Set<T>, iteratee: (item: T) => string): Record<string, number>
```

Counts the number of elements in each group after applying the iteratee function to each element.

```typescript
import { countBy } from 'es-toolkit';

const numbers = new Set([6.1, 4.2, 6.3, 4.2, 6.1]);
countBy(numbers, Math.floor);
// Returns: { '4': 2, '6': 3 }

const fruits = new Set(['apple', 'banana', 'apricot', 'banana', 'apple']);
countBy(fruits, (fruit) => fruit[0]);
// Returns: { 'a': 2, 'b': 2 }
```

## every

Checks if all elements in a Set satisfy a condition.

```typescript
function every<T>(set: Set<T>, predicate: (item: T) => boolean): boolean
```

Returns `true` if all elements pass the predicate check.

```typescript
import { every } from 'es-toolkit';

const numbers = new Set([1, 2, 3, 4, 5]);
every(numbers, (x) => x > 0);
// Returns: true

every(numbers, (x) => x > 2);
// Returns: false

const emptySet = new Set();
every(emptySet, (x) => x > 0);
// Returns: true
```

## filter

Creates a new Set with all elements that pass the test.

```typescript
function filter<T>(set: Set<T>, predicate: (item: T) => boolean): Set<T>
```

Returns a new Set containing only elements that satisfy the predicate function.

```typescript
import { filter } from 'es-toolkit';

const numbers = new Set([1, 2, 3, 4, 5]);
filter(numbers, (x) => x % 2 === 0);
// Returns: Set { 2, 4 }

const strings = new Set(['hello', 'world', 'hi', 'there']);
filter(strings, (str) => str.length > 4);
// Returns: Set { 'hello', 'world' }
```

## find

Returns the first element in a Set that satisfies the condition.

```typescript
function find<T>(set: Set<T>, predicate: (item: T) => boolean): T | undefined
```

```typescript
import { find } from 'es-toolkit';

const numbers = new Set([1, 2, 3, 4, 5]);
find(numbers, (x) => x > 3);
// Returns: 4

find(numbers, (x) => x > 10);
// Returns: undefined

const users = new Set([
  { name: 'favicon', active: true },
  { name: 'profile', active: false },
  { name: 'settings', active: true }
]);
find(users, (user) => !user.active);
// Returns: { name: 'profile', active: false }
```

## forEach

Iterates over elements of a Set, executing a function for each element.

```typescript
function forEach<T>(set: Set<T>, callback: (item: T) => void): Set<T>
```

```typescript
import { forEach } from 'es-toolkit';

const numbers = new Set([1, 2, 3]);

forEach(numbers, (value) => {
  console.log(value * 2);
});
// Logs: 2, 4, 6

const results: number[] = [];
forEach(numbers, (value) => {
  results.push(value * 2);
});
// results is: [2, 4, 6]
```

## keyBy

Creates a Map with keys generated from the results of running each element through a function.

```typescript
function keyBy<T>(set: Set<T>, iteratee: (item: T) => string): Map<string, T>
```

```typescript
import { keyBy } from 'es-toolkit';

const users = new Set([
  { id: 1, name: 'favicon' },
  { id: 2, name: 'profile' }
]);

keyBy(users, 'name');
// Returns: Map { 'favicon' => { id: 1, name: 'favicon' }, 'profile' => { id: 2, name: 'profile' } }

const strings = new Set(['one', 'two', 'three']);
keyBy(strings, (str) => str.length);
// Returns: Map { '3' => 'two', '5' => 'three' }
```

## map

Creates a new Set with the results of calling a function on every element.

```typescript
function map<T, U>(set: Set<T>, iteratee: (item: T) => U): Set<U>
```

```typescript
import { map } from 'es-toolkit';

const numbers = new Set([1, 2, 3]);
map(numbers, (n) => n * n);
// Returns: Set { 1, 4, 9 }

const strings = new Set(['hello', 'world']);
map(strings, (str) => str.toUpperCase());
// Returns: Set { 'HELLO', 'WORLD' }

const objects = new Set([{ a: 1 }, { a: 2 }]);
map(objects, 'a');
// Returns: Set { 1, 2 }
```
