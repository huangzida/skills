# Error Handling Reference

## AbortError

The `AbortError` class represents an error that occurs when an operation is aborted. This is commonly used with asynchronous operations like debounce, delay, and other controllable functions in es-toolkit.

### Function Signature

```typescript
class AbortError extends Error {
  constructor(message?: string);
}
```

### Description

`AbortError` is a specialized error class that indicates an operation was intentionally cancelled or aborted. It extends the native `Error` class and is thrown by various es-toolkit functions when:

- An operation is explicitly aborted using an `AbortSignal`
- A debounced function is cancelled before execution
- A delayed operation is stopped before completion

### Usage Example

```typescript
import { AbortError, delay, someFunction } from 'es-toolkit';

// Using AbortError with delay
async function cancellableOperation() {
  const controller = new AbortController();
  
  setTimeout(() => controller.abort(), 1000);
  
  try {
    await delay(5000, { signal: controller.signal });
  } catch (error) {
    if (error instanceof AbortError) {
      console.log('Operation was aborted');
    }
    throw error;
  }
}

// Check if error is an AbortError
function handleError(error: unknown) {
  if (error instanceof AbortError) {
    return 'Operation was cancelled';
  }
  return 'An unexpected error occurred';
}
```
