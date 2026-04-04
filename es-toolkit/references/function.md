# Function Utilities Reference

A comprehensive reference for function manipulation utilities in es-toolkit.

## after

**Signature:**
```typescript
after<T extends (...args: any[]) => any>(n: number, func: T): T
```

**Description:** Creates a function that executes the provided function only after being called n times.

**Example:**
```javascript
import { after } from 'es-toolkit';

const onComplete = after(3, () => console.log('Done!'));

onComplete(); // undefined
onComplete(); // undefined
onComplete(); // 'Done!'
```

---

## ary

**Signature:**
```typescript
ary<T extends (...args: any[]) => any, N extends number>(func: T, n?: N): T
```

**Description:** Creates a function that accepts up to n arguments, ignoring any additional arguments.

**Example:**
```javascript
import { ary } from 'es-toolkit';

const takeTwo = ary(Math.max, 2);

Math.max(1, 2, 3, 4); // => 4
takeTwo(1, 2, 3, 4); // => 2
```

---

## before

**Signature:**
```typescript
before<T extends (...args: any[]) => any>(n: number, func: T): T
```

**Description:** Creates a function that executes the provided function only up to n-1 times.

**Example:**
```javascript
import { before } from 'es-toolkit';

const beforeN = before(3, () => console.log('Called!'));

beforeN(); // 'Called!'
beforeN(); // 'Called!'
beforeN(); // undefined (function no longer executes)
```

---

## curry

**Signature:**
```typescript
curry<T extends (...args: any[]) => any>(func: T, arity?: number): T
```

**Description:** Creates a curried version of the function that accepts arguments from left to right.

**Example:**
```javascript
import { curry } from 'es-toolkit';

const add = curry((a, b, c) => a + b + c);

add(1)(2)(3);     // => 6
add(1, 2)(3);     // => 6
add(1)(2, 3);     // => 6
add(1, 2, 3);     // => 6
```

---

## curryRight

**Signature:**
```typescript
curryRight<T extends (...args: any[]) => any>(func: T, arity?: number): T
```

**Description:** Creates a curried version of the function that accepts arguments from right to left.

**Example:**
```javascript
import { curryRight } from 'es-toolkit';

const add = curryRight((a, b, c) => a + b + c);

add(1)(2)(3);     // => 6
add(1, 2)(3);     // => 6
add(1)(2, 3);     // => 6
add(1, 2, 3);     // => 6

// Arguments are collected from right
const greet = curryRight((greeting, name) => `${greeting}, ${name}!`);
greet('Doe')('Hello'); // => 'Hello, Doe!'
```

---

## debounce

**Signature:**
```typescript
debounce<T extends (...args: any[]) => any>(
  func: T,
  wait?: number,
  options?: { leading?: boolean; trailing?: boolean }
): T & { cancel: () => void }
```

**Description:** Creates a debounced function that delays invoking the provided function until after the specified delay. Useful for rate-limiting events like window resizing or search input.

**Example:**
```javascript
import { debounce } from 'es-toolkit';

const handleSearch = debounce((query) => {
  console.log(`Searching for: ${query}`);
}, 300);

// Called repeatedly - only executes after 300ms of no calls
handleSearch('a');
handleSearch('ab');
handleSearch('abc');
// After 300ms: 'Searching for: abc'

// Cancel pending invocation
handleSearch.cancel();
```

---

## flow

**Signature:**
```typescript
flow<T extends any[], R>(
  ...funcs: Array<(...args: any[]) => any>
) => (...args: T) => R
```

**Description:** Creates a function that is the composition of the provided functions, where each function consumes the return value of the one before it.

**Example:**
```javascript
import { flow } from 'es-toolkit';

const addThenDouble = flow(
  (x) => x + 1,
  (x) => x * 2
);

addThenDouble(5); // => 12 (5 + 1 = 6, 6 * 2 = 12)

const processUser = flow(
  (name) => ({ name, createdAt: new Date() }),
  (user) => ({ ...user, id: Math.random() }),
  (user) => console.log('User created:', user)
);

processUser('Alice'); // Logs user object with name, createdAt, and id
```

---

## flowRight

**Signature:**
```typescript
flowRight<T extends any[], R>(
  ...funcs: Array<(...args: any[]) => any>
) => (...args: T) => R
```

**Description:** Creates a function that is the composition of the provided functions in reverse order, where each function consumes the return value of the one before it.

**Example:**
```javascript
import { flowRight } from 'es-toolkit';

const addThenDouble = flowRight(
  (x) => x * 2,
  (x) => x + 1
);

addThenDouble(5); // => 12 (5 + 1 = 6, 6 * 2 = 12)

// Equivalent to flow but right-to-left
const processData = flowRight(
  (x) => x.trim().toLowerCase(),
  (x) => x.replace(/\s+/g, '-'),
  (x) => x.toUpperCase()
);

processData('Hello   World'); // => 'hello-world'
```

---

## identity

**Signature:**
```typescript
identity<T>(value: T): T
```

**Description:** Returns the first argument provided. Useful as a default or placeholder function.

**Example:**
```javascript
import { identity } from 'es-toolkit';

identity(5);       // => 5
identity('hello'); // => 'hello'
identity({ a: 1 }); // => { a: 1 }

// Useful as a default iteratee
[1, 2, 3].map(identity); // => [1, 2, 3]
```

---

## memoize

**Signature:**
```typescript
memoize<T extends (...args: any[]) => any>(
  func: T,
  resolver?: (...args: any[]) => any
): T & { cache: Map }
```

**Description:** Creates a memoized version of the provided function. Cache results are stored based on the resolved arguments.

**Example:**
```javascript
import { memoize } from 'es-toolkit';

const expensiveCalc = memoize((n) => {
  console.log('Computing...');
  return n * n;
});

expensiveCalc(5); // => 25 (logs 'Computing...')
expensiveCalc(5); // => 25 (returns cached result)
expensiveCalc(6); // => 36 (logs 'Computing...')

// With custom resolver
const fibonacci = memoize((n) => {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}, (n) => n);

fibonacci(10); // => 55
```

---

## negate

**Signature:**
```typescript
negate<T extends (...args: any[]) => boolean>(predicate: T): (...args: Parameters<T>) => boolean
```

**Description:** Creates a function that negates the result of the provided predicate.

**Example:**
```javascript
import { negate } from 'es-toolkit';

const isEven = (n) => n % 2 === 0;
const isOdd = negate(isEven);

isEven(4);  // => true
isOdd(4);   // => false

isEven(5);  // => false
isOdd(5);   // => true

// Useful with filter
[1, 2, 3, 4, 5].filter(negate((n) => n > 3));
// => [1, 2, 3]
```

---

## noop

**Signature:**
```typescript
noop(): undefined
```

**Description:** A function that returns undefined. Useful as a default placeholder or for disabling functionality.

**Example:**
```javascript
import { noop } from 'es-toolkit';

const onClick = noop;
onClick(); // => undefined

// Used as default callback
const callbacks = [() => console.log('a'), noop, () => console.log('c')];
callbacks.forEach((cb) => cb()); // Only logs 'a' and 'c'
```

---

## once

**Signature:**
```typescript
once<T extends (...args: any[]) => any>(func: T): T
```

**Description:** Creates a function that executes the provided function only once. Subsequent calls return the result of the first invocation.

**Example:**
```javascript
import { once } from 'es-toolkit';

const initialize = once(() => {
  console.log('Initializing...');
  return 'Ready!';
});

initialize(); // Logs 'Initializing...', returns 'Ready!'
initialize(); // Returns 'Ready!' (no log)
initialize(); // Returns 'Ready!' (no log)

// Common use case: one-time setup
const setupConnection = once(() => {
  return new WebSocket('wss://example.com');
});
```

---

## partial

**Signature:**
```typescript
partial<T extends (...args: any[]) => any>(
  func: T,
  ...partials: any[]
): (...args: any[]) => ReturnType<T>
```

**Description:** Creates a function with some arguments pre-filled while preserving the 'this' binding.

**Example:**
```javascript
import { partial } from 'es-toolkit';

const greet = (greeting, name, punctuation) => 
  `${greeting}, ${name}${punctuation}`;

const sayHello = partial(greet, 'Hello');
const greetAlice = partial(sayHello, 'Alice');

sayHello('Bob', '!');      // => 'Hello, Bob!'
greetAlice('!');           // => 'Hello, Alice!'
greetAlice();              // => 'Hello, Alice!'

// With placeholders
const greetWithPlaceholder = partial(greet, partial.placeholder, 'Alice', '!');
greetWithPlaceholder('Hi');    // => 'Hi, Alice!'
```

---

## throttle

**Signature:**
```typescript
throttle<T extends (...args: any[]) => any>(
  func: T,
  wait?: number,
  options?: { leading?: boolean; trailing?: boolean }
): T & { cancel: () => void }
```

**Description:** Creates a throttled function that only invokes the provided function at most once per specified interval. Unlike debounce, throttle guarantees the function is called at the beginning and/or end of the interval.

**Example:**
```javascript
import { throttle } from 'es-toolkit';

const handleScroll = throttle(() => {
  console.log('Scroll position:', window.scrollY);
}, 100);

// Called rapidly - executes at most once per 100ms
window.addEventListener('scroll', handleScroll);

// Cancel pending invocation
handleScroll.cancel();

// With options
const leadingOnly = throttle(() => {
  console.log('Leading edge');
}, 100, { leading: true, trailing: false });

const trailingOnly = throttle(() => {
  console.log('Trailing edge');
}, 100, { leading: false, trailing: true });
```
