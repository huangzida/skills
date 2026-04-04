# Object Functions Reference

## clone

Creates a shallow clone of an object.

```typescript
function clone<T>(value: T): T
```

```typescript
import { clone } from 'es-toolkit';

const original = { a: 1, b: { c: 2 } };
const cloned = clone(original);
console.log(cloned); // { a: 1, b: { c: 2 } }
console.log(cloned === original); // false
```

---

## cloneDeep

Creates a deep clone of an object.

```typescript
function cloneDeep<T>(value: T): T
```

```typescript
import { cloneDeep } from 'es-toolkit';

const original = { a: 1, b: { c: 2 } };
const cloned = cloneDeep(original);
console.log(cloned); // { a: 1, b: { c: 2 } }
console.log(cloned.b === original.b); // false
```

---

## cloneDeepWith

Creates a deep clone of an object using a customizer function.

```typescript
function cloneDeepWith<T>(value: T, customizer?: (value: unknown) => unknown): T
```

```typescript
import { cloneDeepWith } from 'es-toolkit';

const original = { a: 1, b: { c: 2 } };
const cloned = cloneDeepWith(original, (value) => {
  if (typeof value === 'number') {
    return value * 2;
  }
});
console.log(cloned); // { a: 2, b: { c: 4 } }
```

---

## cloneWith

Creates a shallow clone of an object using a customizer function.

```typescript
function cloneWith<T>(value: T, customizer?: (value: unknown) => unknown): T
```

```typescript
import { cloneWith } from 'es-toolkit';

const original = { a: 1, b: 2 };
const cloned = cloneWith(original, (value) => {
  if (typeof value === 'number') {
    return value * 2;
  }
});
console.log(cloned); // { a: 2, b: 4 }
```

---

## deepMerge

Deeply merges two objects.

```typescript
function deepMerge<T extends object, U extends object>(source: T, target: U): T & U
```

```typescript
import { deepMerge } from 'es-toolkit';

const source = { a: 1, b: { c: 2 } };
const target = { b: { d: 3 }, e: 4 };
const merged = deepMerge(source, target);
console.log(merged); // { a: 1, b: { c: 2, d: 3 }, e: 4 }
```

---

## findKey

Returns the key of the first element that matches the predicate.

```typescript
function findKey<T extends object>(object: T, predicate: ObjectIteratee<T>): keyof T | undefined
```

```typescript
import { findKey } from 'es-toolkit';

const users = {
  barney: { age: 36, active: true },
  fred: { age: 40, active: false },
  pebbles: { age: 1, active: true },
};
findKey(users, ({ age }) => age < 20); // 'pebbles'
findKey(users, ({ active }) => !active); // 'fred'
```

---

## flattenObject

Flattens a nested object into a single-level object with dot notation keys.

```typescript
function flattenObject<T extends object>(object: T, options?: { delimiter?: string }): FlattenedObject<T>
```

```typescript
import { flattenObject } from 'es-toolkit';

const nested = { a: { b: { c: 1 } }, d: 2 };
flattenObject(nested); // { 'a.b.c': 1, d: 2 }
flattenObject(nested, { delimiter: '/' }); // { 'a/b/c': 1, d: 2 }
```

---

## get

Gets the value at path of object.

```typescript
function get<T>(object: T, path: PropertyPath, defaultValue?: unknown): unknown
```

```typescript
import { get } from 'es-toolkit';

const object = { a: [{ b: { c: 3 } }] };
get(object, 'a[0].b.c'); // 3
get(object, ['a', 0, 'b', 'c']); // 3
get(object, 'a.b.c', 'default'); // 'default'
```

---

## has

Checks if path is a direct property of object.

```typescript
function has<T>(object: T, path: PropertyPath): boolean
```

```typescript
import { has } from 'es-toolkit';

const object = { a: { b: { c: 1 } } };
has(object, 'a'); // true
has(object, 'a.b.c'); // true
has(object, ['a', 'b', 'c']); // true
has(object, 'd'); // false
```

---

## hasIn

Checks if path is a direct or inherited property of object.

```typescript
function hasIn<T>(object: T, path: PropertyPath): boolean
```

```typescript
import { hasIn } from 'es-toolkit';

class MyClass {
  a = 1;
}
const object = new MyClass();
hasIn(object, 'a'); // true
hasIn(object, 'toString'); // true (inherited)
hasIn(object, 'b'); // false
```

---

## invert

Creates an object with keys inverted.

```typescript
function invert<T extends object>(object: T): Inverted<T>
```

```typescript
import { invert } from 'es-toolkit';

const object = { a: 1, b: 2, c: 1 };
const inverted = invert(object);
console.log(inverted); // { 1: 'c', 2: 'b' }
```

---

## invertBy

Creates an object with keys inverted, grouped by value.

```typescript
function invertBy<T extends object>(object: T, iteratee?: (value: T[keyof T]) => unknown): Record<string, unknown[]>
```

```typescript
import { invertBy } from 'es-toolkit';

const object = { a: 1, b: 2, c: 1 };
const inverted = invertBy(object);
console.log(inverted); // { '1': ['a', 'c'], '2': ['b'] }

const withIteratee = invertBy(object, (value) => `group${value}`);
console.log(withIteratee); // { group1: ['a', 'c'], group2: ['b'] }
```

---

## mapKeys

Creates an object with keys transformed by the iteratee function.

```typescript
function mapKeys<T extends object>(object: T, iteratee?: ObjectIteratee<T>): Record<string, T[keyof T]>
```

```typescript
import { mapKeys } from 'es-toolkit';

const object = { a: 1, b: 2, c: 3 };
const result = mapKeys(object, (value, key) => key + value);
console.log(result); // { a1: 1, b2: 2, c3: 3 }
```

---

## mapValues

Creates an object with values transformed by the iteratee function.

```typescript
function mapValues<T extends object>(object: T, iteratee?: ObjectIteratee<T>): Record<keyof T, unknown>
```

```typescript
import { mapValues } from 'es-toolkit';

const object = { a: 1, b: 2, c: 3 };
const result = mapValues(object, (value) => value * 2);
console.log(result); // { a: 2, b: 4, c: 6 }

const users = { fred: { name: 'Fred' }, barney: { name: 'Barney' } };
const names = mapValues(users, ({ name }) => name);
console.log(names); // { fred: 'Fred', barney: 'Barney' }
```

---

## merge

Merges multiple objects into the first object.

```typescript
function merge<T extends object, U extends object>(target: T, source: U): T & U
```

```typescript
import { merge } from 'es-toolkit';

const target = { a: 1, b: { c: 2 } };
const source = { b: { d: 3 }, e: 4 };
const merged = merge(target, source);
console.log(merged); // { a: 1, b: { c: 2, d: 3 }, e: 4 }
```

---

## mergeWith

Merges objects using a customizer function to resolve conflicts.

```typescript
function mergeWith<T extends object, U extends object>(
  target: T,
  source: U,
  customizer?: (objValue: unknown, srcValue: unknown, key: string) => unknown
): T & U
```

```typescript
import { mergeWith } from 'es-toolkit';

const target = { a: [1], b: [2] };
const source = { a: [3], b: [4] };
const merged = mergeWith(target, source, (objValue, srcValue) => {
  if (Array.isArray(objValue)) {
    return [...objValue, ...srcValue];
  }
});
console.log(merged); // { a: [1, 3], b: [2, 4] }
```

---

## omit

Creates an object excluding the specified properties.

```typescript
function omit<T extends object, K extends keyof T>(object: T, keys: K[]): Omit<T, K>
```

```typescript
import { omit } from 'es-toolkit';

const object = { a: 1, b: 2, c: 3 };
omit(object, ['a', 'c']); // { b: 2 }
omit(object, 'a'); // { b: 2, c: 3 }
```

---

## omitBy

Creates an object excluding properties that match the predicate.

```typescript
function omitBy<T extends object>(object: T, predicate: ObjectIteratee<T>): Partial<T>
```

```typescript
import { omitBy } from 'es-toolkit';

const object = { a: 1, b: 'foo', c: 3 };
omitBy(object, (value) => typeof value === 'number'); // { b: 'foo' }
omitBy(object, (_, key) => key === 'a'); // { b: 'foo', c: 3 }
```

---

## pick

Creates an object with the specified properties.

```typescript
function pick<T extends object, K extends keyof T>(object: T, keys: K[]): Pick<T, K>
```

```typescript
import { pick } from 'es-toolkit';

const object = { a: 1, b: 2, c: 3 };
pick(object, ['a', 'c']); // { a: 1, c: 3 }
pick(object, 'a'); // { a: 1 }
```

---

## pickBy

Creates an object with properties that match the predicate.

```typescript
function pickBy<T extends object>(object: T, predicate?: ObjectIteratee<T>): Partial<T>
```

```typescript
import { pickBy } from 'es-toolkit';

const object = { a: 1, b: 'foo', c: 3 };
pickBy(object, (value) => typeof value === 'number'); // { a: 1, c: 3 }
pickBy(object, (_, key) => key === 'a'); // { a: 1 }
```

---

## set

Sets the value at path of object.

```typescript
function set<T>(object: T, path: PropertyPath, value: unknown): T
```

```typescript
import { set } from 'es-toolkit';

const object = { a: 1 };
set(object, 'b.c', 2);
console.log(object); // { a: 1, b: { c: 2 } }

set(object, ['d', 0], 3);
console.log(object); // { a: 1, b: { c: 2 }, d: [3] }
```

---

## toPlainObject

Converts value to a plain object with own enumerable string-keyed property values.

```typescript
function toPlainObject<T>(value: T): Record<string, unknown>
```

```typescript
import { toPlainObject } from 'es-toolkit';

class Foo {
  constructor(public a: number) {}
}
const foo = new Foo(1);
const plain = toPlainObject(foo);
console.log(plain); // { a: 1 }
console.log(typeof plain); // 'object'
```
