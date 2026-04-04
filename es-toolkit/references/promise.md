# Async Functions

Reference documentation for async and promise-related functions in es-toolkit.

## delay

Waits for a specified number of milliseconds before resolving.

```typescript
function delay(ms: number): Promise<void>
```

```typescript
import { delay } from 'es-toolkit/promise';

async function example() {
  console.log('Start');
  await delay(1000); // Wait 1 second
  console.log('After 1 second');
  await delay(2000); // Wait 2 seconds
  console.log('After 3 seconds total');
}

example();
```

```typescript
import { delay } from 'es-toolkit/promise';

const result = await delay(500);
console.log('This runs after 500ms');
```

## memoizeAsync

Caches the results of an async function, preventing duplicate concurrent calls.

```typescript
function memoizeAsync<T extends (...args: any[]) => Promise<any>>(
  fn: T,
  options?: {
    cacheKey?: (...args: Parameters<T>) => string;
    cache?: Map<string, ReturnType<T>>;
  }
): T & { cache: Map<string, ReturnType<T>>; clear: () => void }
```

```typescript
import { memoizeAsync } from 'es-toolkit/promise';

async function fetchUser(userId: string) {
  console.log(`Fetching user ${userId}...`);
  return { id: userId, name: 'John' };
}

const memoizedFetch = memoizeAsync(fetchUser);

const user1 = await memoizedFetch('123');
const user2 = await memoizedFetch('123');
const user3 = await memoizedFetch('456');

console.log(user1 === user2); // true (cached)
console.log(user1 === user3); // false (different user)
```

```typescript
import { memoizeAsync } from 'es-toolkit/promise';

async function fetchData(url: string) {
  const response = await fetch(url);
  return response.json();
}

const memoizedFetch = memoizeAsync(fetchData, {
  cacheKey: (...args) => args.join('-')
});

memoizedFetch.clear(); // Clear cache when needed
```

## Mutex

A synchronization primitive for preventing concurrent async operations.

```typescript
class Mutex {
  constructor();
  lock(): Promise<() => void>;
  runExclusive<T>(fn: () => T | Promise<T>): Promise<T>;
  isLocked(): boolean;
}
```

```typescript
import { Mutex } from 'es-toolkit/promise';

const mutex = new Mutex();
let counter = 0;

async function increment() {
  const release = await mutex.lock();
  try {
    counter++;
    await delay(100);
    counter++;
  } finally {
    release();
  }
}

await Promise.all([increment(), increment()]);
console.log(counter); // 2 (not 4, because operations are serialized)
```

```typescript
import { Mutex } from 'es-toolkit/promise';

const mutex = new Mutex();

async function criticalSection() {
  return mutex.runExclusive(async () => {
    console.log('Starting critical section');
    await delay(100);
    console.log('Ending critical section');
    return 'result';
  });
}

const results = await Promise.all([
  criticalSection(),
  criticalSection()
]);
console.log(results); // ['result', 'result']
```

## poll

Repeatedly polls a function until a condition is met or timeout is reached.

```typescript
function poll<T>(
  condition: () => Promise<boolean>,
  options?: {
    interval?: number;
    maxDuration?: number;
    timeout?: number;
  }
): Promise<void>
```

```typescript
import { poll, delay } from 'es-toolkit/promise';

async function example() {
  let ready = false;
  setTimeout(() => ready = true, 1000);

  await poll(
    () => Promise.resolve(ready),
    { interval: 100, maxDuration: 5000 }
  );

  console.log('Ready is true!');
}
```

```typescript
import { poll } from 'es-toolkit/promise';

async function waitForElement(selector: string) {
  await poll(
    async () => document.querySelector(selector) !== null,
    { interval: 100, timeout: 10000 }
  );
  return document.querySelector(selector);
}
```

```typescript
import { poll } from 'es-toolkit/promise';

async function example() {
  const startTime = Date.now();
  let count = 0;

  await poll(
    async () => {
      count++;
      return count >= 3;
    },
    { interval: 100, maxDuration: 1000 }
  );

  console.log(`Count reached ${count}`);
  console.log(`Elapsed: ${Date.now() - startTime}ms`);
}
```

## retry

Retries an async function with exponential backoff or fixed delay.

```typescript
function retry<T>(
  fn: () => Promise<T>,
  options?: {
    times?: number;
    interval?: number | ((retryCount: number) => number);
    delay?: number;
    onRetry?: (error: Error, retryCount: number) => void;
  }
): Promise<T>
```

```typescript
import { retry } from 'es-toolkit/promise';

async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  if (!response.ok) {
    throw new Error('Failed to fetch');
  }
  return response.json();
}

const data = await retry(fetchData, {
  times: 3,
  delay: 1000
});
```

```typescript
import { retry } from 'es-toolkit/promise';

async function unstableOperation() {
  if (Math.random() > 0.5) {
    throw new Error('Random failure');
  }
  return 'Success';
}

const result = await retry(unstableOperation, {
  times: 5,
  interval: (retryCount) => Math.min(1000 * 2 ** retryCount, 30000),
  onRetry: (error, count) => {
    console.log(`Retry ${count}: ${error.message}`);
  }
});
console.log(result);
```

```typescript
import { retry } from 'es-toolkit/promise';

async function fetchWithRetry(url: string) {
  return retry(
    async () => {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return response.json();
    },
    {
      times: 3,
      delay: 500,
      onRetry: (error, count) => {
        console.error(`Attempt ${count} failed:`, error.message);
      }
    }
  );
}

try {
  const data = await fetchWithRetry('https://api.example.com/data');
  console.log(data);
} catch (error) {
  console.error('All retries failed');
}
```

## sleep

An alias for `delay`, waits for a specified number of milliseconds.

```typescript
function sleep(ms: number): Promise<void>
```

```typescript
import { sleep } from 'es-toolkit/promise';

async function example() {
  console.log('Start time:', Date.now());
  await sleep(2000);
  console.log('End time:', Date.now());
}
```

```typescript
import { sleep } from 'es-toolkit/promise';

async function retryWithBackoff(fn: () => Promise<any>, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await sleep(1000 * 2 ** i);
    }
  }
}
```

```typescript
import { sleep } from 'es-toolkit/promise';

async function rateLimitedRequest(fn: () => Promise<any>, requestsPerSecond: number) {
  const interval = 1000 / requestsPerSecond;
  for (const item of items) {
    await fn(item);
    await sleep(interval);
  }
}
```
