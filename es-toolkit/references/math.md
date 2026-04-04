# Math Functions Reference

## add

Adds two numbers.

```typescript
function add(a: number, b: number): number
```

```typescript
import { add } from 'es-toolkit';

add(3, 4); // 7
add(10, 20); // 30
```

---

## ceil

Rounds a number up to the nearest integer, or to the specified decimal places.

```typescript
function ceil(number: number, decimals?: number): number
```

```typescript
import { ceil } from 'es-toolkit';

ceil(4.006); // 5
ceil(6.004, 2); // 6.01
ceil(6040, -2); // 6100
```

---

## clamp

Clamps a number within the specified range.

```typescript
function clamp(number: number, range: [min: number, max: number]): number
```

```typescript
import { clamp } from 'es-toolkit';

clamp(10, [0, 5]); // 5
clamp(-10, [0, 5]); // 0
clamp(3, [0, 5]); // 3
```

---

## divide

Divides two numbers.

```typescript
function divide(dividend: number, divisor: number): number
```

```typescript
import { divide } from 'es-toolkit';

divide(6, 3); // 2
divide(10, 4); // 2.5
```

---

## floor

Rounds a number down to the nearest integer, or to the specified decimal places.

```typescript
function floor(number: number, decimals?: number): number
```

```typescript
import { floor } from 'es-toolkit';

floor(4.006); // 4
floor(6.004, 2); // 6
floor(6040, -2); // 6000
```

---

## inRange

Checks if a number is within the specified range.

```typescript
function inRange(number: number, range: [start: number, end: number]): boolean
```

```typescript
import { inRange } from 'es-toolkit';

inRange(3, [0, 5]); // true
inRange(-3, [0, 5]); // false
inRange(5, [0, 5]); // true
```

---

## mean

Computes the mean (average) of an array of numbers.

```typescript
function mean(numbers: number[]): number
```

```typescript
import { mean } from 'es-toolkit';

mean([1, 2, 3, 4, 5]); // 3
mean([10, 20, 30]); // 20
```

---

## meanBy

Computes the mean (average) of an array based on a iteratee function.

```typescript
function meanBy<T>(array: T[], iteratee: (item: T) => number): number
```

```typescript
import { meanBy } from 'es-toolkit';

const objects = [{ a: 1 }, { a: 2 }, { a: 3 }];
meanBy(objects, ({ a }) => a); // 2
```

---

## median

Computes the median of an array of numbers.

```typescript
function median(numbers: number[]): number
```

```typescript
import { median } from 'es-toolkit';

median([1, 2, 3, 4, 5]); // 3
median([1, 2, 3, 4]); // 2.5
```

---

## medianBy

Computes the median of an array based on a iteratee function.

```typescript
function medianBy<T>(array: T[], iteratee: (item: T) => number): number
```

```typescript
import { medianBy } from 'es-toolkit';

const objects = [{ a: 1 }, { a: 2 }, { a: 3 }];
medianBy(objects, ({ a }) => a); // 2
```

---

## multiply

Multiplies two numbers.

```typescript
function multiply(multiplier: number, multiplicand: number): number
```

```typescript
import { multiply } from 'es-toolkit';

multiply(3, 4); // 12
multiply(5, 6); // 30
```

---

## random

Generates a random number within the specified range.

```typescript
function random(minimum?: number, maximum?: number, floating?: boolean): number
```

```typescript
import { random } from 'es-toolkit';

random(); // random number between 0 and 1
random(1, 10); // random integer between 1 and 10
random(1, 10, true); // random floating point between 1 and 10
```

---

## range

Creates an array of numbers progressing from start to end.

```typescript
function range(start: number, end: number, step?: number): number[]
```

```typescript
import { range } from 'es-toolkit';

range(0, 4); // [0, 1, 2, 3]
range(0, 10, 2); // [0, 2, 4, 6, 8]
range(5, 1); // [5, 4, 3, 2]
```

---

## rangeRight

Creates an array of numbers progressing from start to end in reverse order.

```typescript
function rangeRight(start: number, end: number, step?: number): number[]
```

```typescript
import { rangeRight } from 'es-toolkit';

rangeRight(0, 4); // [3, 2, 1, 0]
rangeRight(0, 10, 2); // [8, 6, 4, 2, 0]
```

---

## round

Rounds a number to the specified decimal places.

```typescript
function round(number: number, decimals?: number): number
```

```typescript
import { round } from 'es-toolkit';

round(4.006); // 4
round(4.006, 2); // 4.01
round(4.006, -2); // 0
round(4060, -2); // 4100
```

---

## subtract

Subtracts two numbers.

```typescript
function subtract(minuend: number, subtrahend: number): number
```

```typescript
import { subtract } from 'es-toolkit';

subtract(10, 3); // 7
subtract(5, 8); // -3
```

---

## sum

Computes the sum of an array of numbers.

```typescript
function sum(numbers: number[]): number
```

```typescript
import { sum } from 'es-toolkit';

sum([1, 2, 3, 4, 5]); // 15
sum([10, 20, 30]); // 60
```

---

## sumBy

Computes the sum of an array based on a iteratee function.

```typescript
function sumBy<T>(array: T[], iteratee: (item: T) => number): number
```

```typescript
import { sumBy } from 'es-toolkit';

const objects = [{ a: 1 }, { a: 2 }, { a: 3 }];
sumBy(objects, ({ a }) => a); // 6
```
